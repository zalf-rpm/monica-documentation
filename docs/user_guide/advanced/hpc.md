# Running Multiple MONICA Instances on HPC Clusters

MONICA can run as a ZeroMQ server and receive simulation requests from clients or a ZeroMQ proxy. Multiple MONICA server processes can be started to process independent simulations concurrently on one or more compute nodes.

> A single `monica-zmq-server` process does not automatically use multiple CPU cores. Parallel execution requires multiple MONICA server processes.

---

## 1. Server modes

MONICA supports three common configurations:

1. Direct request/reply mode
2. Proxy-based worker mode
3. Pipeline mode using separate input and output sockets

For HPC workloads, proxy-based worker mode is generally the most suitable because multiple workers can connect to the same proxy.

---

## 2. Display command-line options

Show the available options with:

```
monica-zmq-server -h
```
or

```
monica-zmq-server --help
```

Display version information with:

```
monica-zmq-server --version
```

The version command prints the MONICA version and the ZeroMQ version used by the executable.

---

## 3. Command syntax

```
monica-zmq-server [options]
```

---

## 4. Command-line options


| Short option | Long option               | Description                                                                                 | Default                              |
|--------------|---------------------------|---------------------------------------------------------------------------------------------|--------------------------------------|
| `-h`         | `--help`                  | Display help and exit.                                                                      | —                                    |
| `-v`         | `--version`               | Display MONICA and ZeroMQ version information.                                              | —                                    |
| `-d`         | `--debug`                 | Enable debug output.                                                                        | Disabled                             |
| `-s`         | `--serve-address`         | Address on which direct request/reply requests are accepted.                                | `tcp://*:6666`                       |
| `-p`         | `--proxy-address`         | Connect to one or more ZeroMQ proxy backend addresses. Addresses can be comma-separated.    | Use an explicit proxy backed address |
| `-bi`        | `--bind-input`            | Bind the input socket locally.                                                              | —                                    |
| `-ci`        | `--connect-input`         | Connect the input socket to a remote address.                                               | Default                              |
| `-i`         | `--input-address`         | Input address or comma-separated input addresses.                                           | `tcp://localhost:6666`               |
| `-bo`        | `--bind-output`           | Bind the output socket locally.                                                             | —                                    |
| `-co`        | `--connect-output`        | Connect the output socket to a remote address.                                              | Default                              |
| `-o`         | `--output-address`        | Output address or comma-separated output addresses. Enables pipeline mode.                  | `tcp://localhost:7777`               |
| `-or`        | `--router-output-address` | Use a ROUTER output socket instead of the normal PUSH output socket. Enables pipeline mode. | —                                    |
| `-c`         | `--control-address`       | Address of the ZeroMQ control publisher to which the server subscribes.                     | `tcp://localhost:8888`               |

When the `--output-adress` or `--router-output-address` is supplied, MONICA uses pipeline mode. In that mode, input and output socket options must be configured explicitly when the server is binding locally.

---

## 5. Direct request/reply mode

Direct mode is useful for a single MONICA server or for manually addressing individual servers.

Start a server that accepts requests on port `6666`:

```
monica-zmq-server -s tcp://*:6666
```

Client connects to:

```
tcp://server-hostname:6666
```

In direct mode, requests and results use the same ZeroMQ request/reply socket. The server does not use a separate output port.

To run several independent servers on the same host, each server must use a different listening port:

```
monica-zmq-server -s tcp://*:6666
monica-zmq-server -s tcp://*:6667
monica-zmq-server -s tcp://*:6668
```

Clients must distribute requests among those addresses themselves.

---

## 6. Proxy-based worker mode

A proxy distributes requests from clients to multiple MONICA workers.

---

### 6.1 Start the proxy

The default proxy configuration uses:

- Frontend port: `5555`
- Backend port: `5566`

Start the proxy with:

```
monica-zmq-proxy --frontend-port 5555 --backend-port 5566
```

Clients connect to the proxy frontend:

```
tcp://proxy-hostname:5555
```

---

### 6.2 Starting MONICA workers

Each worker connects to the proxy backend:

```
monica-zmq-server -p tcp://proxy-hostname:5566
```

Start multiple workers on the same node or on different nodes using the same backend address:

```
monica-zmq-server -p tcp://proxy-hostname:5566
monica-zmq-server -p tcp://proxy-hostname:5566
monica-zmq-server -p tcp://proxy-hostname:5566
```

Workers connect to the proxy. They do not bind the backend port themselves. Therefore, workers running on the same host do not need different input or output ports.

---

## 7. Pipeline mode

Pipeline mode uses separate sockets for receiving jobs and sending results. It is useful when clients use separate producer and consumer connections or when separate input and output proxies are required.

Start a locally bound pipeline server:

```
monica-zmq-server -bi -i tcp://*:6666 --bind-output -o tcp://*:7777
```

The input and output socket roles are:

| Role                    | Address                      | Socket |
|-------------------------|------------------------------|--------|
| Receive simulation jobs | `tcp://server-hostname:6666` | PULL   |
| Send simulation results | `tcp://server-hostname:7777` | PUSH   |

Do not combine `--proxy-address` with `--output-address` unless you intentionally want pipeline mode. When pipeline mode is enabled, the proxy address is not used for receiving jobs.

---

## 8. Multiple workers with pipeline proxies

A common distributed setup uses two proxies:

- an input proxy distributes jobs to workers
- an output proxy collects results from workers

Example input proxy:

```
monica-zmq-proxy --pull-push-sockets --frontend-port 6666 --backend-port 6677
```

Example output proxy:

```
monica-zmq-proxy --pull-push-sockets --frontend-port 7788 --backend-port 7777
```

Start each MONICA worker with:

```
monica-zmq-server -i tcp://proxy-hostname:6677 -o tcp://proxy-hostname:7788 -c tcp://proxy-hostname:8899
```

Multiple workers can use the same proxy address:

```
monica-zmq-server -i tcp://proxy-hostname:6677 -o tcp://proxy-hostname:7788 -c tcp://proxy-hostname:8899
monica-zmq-server -i tcp://proxy-hostname:6677 -o tcp://proxy-hostname:7788 -c tcp://proxy-hostname:8899
monica-zmq-server -i tcp://proxy-hostname:6677 -o tcp://proxy-hostname:7788 -c tcp://proxy-hostname:8899
```

The client-facing addresses in this example are:

- Submit jobs to `tcp://proxy-hostname:6666`
- Receive results from `tcp://proxy-hostname:7777`

---

## 9. Running workers through a scheduler

The exact command depends on the HPC scheduler. For example, with SLURM:

```
srun --nodes=2 --ntasks-per-node=4 monica-zmq-server --proxy-address tcp://proxy-hostname:5566
```

This starts one MONICA server process per task. Each process connects to the same proxy backend and can process independent simulation jobs.

For pipeline mode:

```
srun --nodes=2 --ntasks-per-node=4 monica-zmq-server --input-address tcp://proxy-hostname:6677 --output-address tcp://proxy-hostname:7788 --control-address tcp://proxy-hostname:8899
```

Adapt the scheduler options, executable path, environment variables, and module or container setup to the local HPC system.

---

## 10. Cluster requirements

Before starting a distributed run:

- Ensure all nodes can resolve the proxy hostname.
- Allow the required TCP ports through firewalls.
- Use explicit non-localhost addresses for cross-node connections.
- Ensure `MONICA_PARAMETERS` and required input data are available on every worker node.
- Use unique listening ports when multiple processes bind sockets on the same host.
- Use the same MONICA version and compatible configuration on all workers.
- Size the number of worker processes according to the available CPU cores and memory.

The default control address uses `localhost` and is therefore suitable only for a local control publisher. For distributed deployments, configure `--control-address` to point to the host and port where the control publisher is available.

---

## 11. Docker reference configuration

The repository's Docker configuration starts two proxies and multiple MONICA workers.

The relevant defaults are:

- Input proxy frontend: `6666`
- Input proxy backend: `6677`
- Output proxy frontend: `7788`
- Output proxy backend: `7777`
- Number of workers: `3`

This corresponds conceptually to:

```
monica-zmq-proxy -pps -f 6666 --b 6677
monica-zmq-proxy -pps -f 7788 --b 7777

monica zmq-server -i tcp://proxy-hostname:6677 -o tcp://proxy-hostname:7788 -c tcp://proxy-hostname:8899
```

Additional worker processes can be started with the same input, output, and control addresses.