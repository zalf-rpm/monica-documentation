# HPC Setup

This section explains how to run multiple MONICA simulations on a **High-Performance Computing (HPC)** cluster or across multiple machines. MONICA can be launched in **server mode**, allowing it to receive simulation jobs via **ZeroMQ** and run them in parallel.

---

## 1. Introduction

When running MONICA on HPC systems or clusters, user can start MONICA in **server mode** using the command:

```
monica-zmq-server
```

In this mode, MONICA acts as a server process that listens for incoming simulation requests sent through ZeroMQ sockets.
Each message contains the full configuration for one simulation run.

This setup allows distributed simulations across:

- Multiple CPU cores on the same node,

- Multiple nodes within a cluster,

- Or even different machines connected via a network

---

## 2. Basic Command Usage

User can view available options with:

```
monica-zmq-server -h
```
or

```
monica-zmq-server --help
```

This will display all supported flags and their default values.

---

## 3. Command Syntax

```
monica-zmq-server [options]
```
Example (default single-node configuration):

```
monica-zmq-server --serve-address tcp://*:6666 --output-address tcp://*:7777
```

---

## 4. Available Options

Overview of all configuration option.

| Option | Long Form | Description | Default Value |
|---------|------------|--------------|----------------|
| `-h` | `--help` | Show help output and exit. | — |
| `-v` | `--version` | Print MONICA and ZeroMQ version info. | — |
| `-d` | `--debug` | Enable detailed debug output for troubleshooting. | — |
| `-s` | `--serve-address` | Bind the MONICA server to a specific address (used for direct client connections). | `tcp://*:6666` |
| `-p` | `--proxy-address` | Connect to one or more proxy addresses for receiving work. Multiple addresses can be comma-separated. | `tcp://localhost:6666` |
| `-bi` | `--bind-input` | Bind the input port manually (for use when controlling binding explicitly). | — |
| `-ci` | `--connect-input` | Connect the input port (default mode). | _(default)_ |
| `-i` | `--input-address` | Specify one or more input addresses for receiving simulation jobs (comma-separated). | `tcp://localhost:6666` |
| `-bo` | `--bind-output` | Bind the output port manually (for explicit output binding). | — |
| `-co` | `--connect-output` | Connect the output port (default mode). | _(default)_ |
| `-o` | `--output-address` | Set output address(es) for sending results to clients or proxies. | `tcp://localhost:7777` |
| `-or` | `--router-output-address` | Send results using a router socket instead of the default dealer socket. | `tcp://localhost:7777` |
| `-c` | `--control-address` | Address for MONICA’s control interface (used for starting, stopping, or monitoring processes). | `tcp://localhost:8888` |

---

## 5. Example Configurations

### **Example 1 – Basic Local Setup**

Run MONICA on a single machine, serving jobs locally:

```
monica-zmq-server --serve-address tcp://*:6666 --output-address tcp://*:7777
```
Clients (or scripts) can then send simulation requests to port 6666
and receive results from port 7777.

### **Example 2 – Connecting via Proxy**

If user already have a MONICA proxy running (for example via Docker or Singularity),
then connect MONICA server to it:

```
monica-zmq-server --proxy-address tcp://proxy-hostname:6666 --output-address tcp://proxy-hostname:7777
```
This allows the MONICA instance to communicate through the proxy layer,
distributing workloads across multiple workers.

### **Example  –Cluster Node Setup**

To use MONICA across several nodes (each node runs as a worker connected to a proxy):

1. Run the proxy on the master node (see the Singularity or Docker setup).

2. Start MONICA worker processes on each compute node, connecting them to the proxy:

```
monica-zmq-server --proxy-address tcp://<proxy-node>:6666 --output-address tcp://<proxy-node>:7777
```
3.  Monitor performance via logs or the control port (default: 8888).

**Notes:**

- Use different ports if multiple MONICA servers are running on the same host.

- For distributed runs, make sure all machines can communicate via TCP on the specified ports.

- The control port (8888) can be used for remote process management and monitoring.

- On HPC systems, user can manage MONICA processes via job schedulers like SLURM or PBS using scripts similar to the Singularity examples.