# Running MONICA in Docker (Cluster Mode)

---

## Overview
MONICA provides an official Docker image for running a MONICA cluster with input/output proxies and multiple MONICA worker processes.  
This container is intended for multi-core systems and server environments where MONICA is used as a service.

There is a Docker image for MONICA available on Docker Hub:

**Image name:** `zalfrpm/monica-cluster`

- Built on **Debian 9** as the base system.  
- Runs MONICA with:  
  i. an **input proxy**  
  ii. an **output proxy**  
  iii. a configurable number of **MONICA worker processes**  
- Each container is designed to run on a **multiprocessor machine** as a service.

---

## 1. Basic Run Command

```bash
docker run -p <input port>:6666 -p <output port>:7777 \
  --env monica_instances=<number of monica worker> \
  --rm \
  --name <monica cluster name> \
  zalfrpm/monica-cluster:<tag>
```

---

## 2. Parameter Explanation

| Parameter | Description |
|------------|--------------|
| `zalfrpm/monica-cluster:<tag>` | Image name on Docker Hub. You can find available tags [here](https://hub.docker.com/r/zalfrpm/monica-cluster/tags). |
| `-p <input port>:6666` | *(Optional)* Input port. Without this parameter, Docker chooses a random host port. |
| `-p <output port>:7777` | *(Optional)* Output port. Without this parameter, Docker chooses a random host port. |
| `--env monica_instances=<number>` | *(Optional)* Number of MONICA worker processes to start. Default: **3**. |
| `--rm` | *(Optional)* Automatically removes the container after it stops. |
| `--name <cluster name>` | *(Optional)* Assigns a human-readable container name. |

---


### 3. For Example

```
docker run -p 6666:6666 -p 7777:7777 \
  --env monica_instances=9 \
  --rm \
  --name my-monica-cluster \
  zalfrpm/monica-cluster:2.0.3.150
```

This starts:

- input proxy → host port 6666

- output proxy → host port 7777

- 9 MONICA worker processes inside the container

- a container named `my-monica-cluster`

- version 2.0.3.150 of the MONICA cluster image

**Note on Tags**

It is possible to use the tag latest, for example:

```
docker run ... zalfrpm/monica-cluster:latest
```

However, it is recommended to use a specific version tag (e.g., 3.6.32) to ensure reproducible results.
Using latest might pull a newer image later, which could lead to slightly different results.

---

### 4. Step-by-Step Guide (for New Users)


#### Install Docker

- **Windows/macOS:** Install [Docker Desktop](https://www.docker.com).  
- **Linux:** Install Docker Engine via your package manager.  
- Open a terminal:

    **Windows:** PowerShell  
    **macOS/Linux:** Terminal

Check installation:

```
docker --version
```
A version number (for example: Docker version 27.xx...) will appear.

#### Pull the MONICA Image

Choose a tag (recommended: a specific version such as 3.6.32):

```
docker pull zalfrpm/monica-cluster:3.6.32
```
#### Start Monica

Start a MONICA container with 9 worker processes:

```
docker run -p 6666:6666 -p 7777:7777 \
  --env monica_instances=9 \
  --rm \
  --name my-monica-cluster \
  zalfrpm/monica-cluster:3.6.32
```
**Explanation:**

- Exposes input port 6666 and output port 7777
- Runs 9 MONICA workers
- Automatically cleans up on exit
- Uses tag 3.6.32 for reproducibility

This command starts a MONICA cluster with 9 workers, exposing input port 6666 and output port 7777.

#### Verify Container Status

In another terminal, type:

```
docker ps
docker logs -f my-monica-cluster
docker port my-monica-cluster
```

#### Stop the Container
Press Ctrl + C in the running terminal, or from another terminal, type:

```
docker stop my-monica-cluster
```

#### Restart MONICA
Run the same command again:

```
docker run -p 6666:6666 -p 7777:7777 \
  --env monica_instances=9 \
  --rm \
  --name my-monica-cluster \
  zalfrpm/monica-cluster:3.6.32
```

