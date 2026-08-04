# Running MONICA on HPC with Singularity

This guide describes how to run MONICA in an HPC environment using Singularity.

The recommended setup uses:

- one proxy service, which handles communication and job coordination
- one or more worker jobs on compute nodes, which execute simulations

The same container image is used for both components.

---

## 1. Architecture

| Component | Role                                                                        | Typical location  |
|-----------|-----------------------------------------------------------------------------|-------------------|
| Proxy     | Receives requests, manages input/output communication, and distributes work | Dedicated service |
| Workers   | Execute MONICA simulations assigned by the proxy                            | Compute nodes     |

The proxy node must be reachable from the compute nodes. Login nodes should only be used if permitted by the HPC administrator.

---

## 2. Prerequisites

You need:

- Singularity installed
- access to a Docker Hub image tag
- a shared filesystem accessible from all relevant nodes
- a hostname or IP address through which workers can reach the proxy
- permission to run a long-lived proxy service

---

## 3. Download the container image

Choose a MONICA version that exists as a Docker Hub tag:

```bash
VERSION=2.2.1.170
IMAGE=/shared/apps/monica/monica-cluster_${VERSION}.sif

singularity pull --name "$IMAGE" "docker://zalfrpm/monica-cluster:${VERSION}"
```

Store the resulting image in a shared, read-only location:

```bash
/shared/apps/monica/monica-cluster_2.2.1.170.sif
```

Verify that the image is available on a compute node:

```bash
singularity inspect "$IMAGE"
```

---

## 4. Create shared directories

Create directories for logs and input data:

```bash
mkdir -p "$HOME/log/supervisor/monica/proxy"
mkdir -p "$HOME/log/supervisor/monica/worker"
mkdir -p "/shared/data/monica/climate-data"
```

The following paths are used inside the container:

| Host path                            | Container path              | Purpose             |
|--------------------------------------|-----------------------------|---------------------|
| `$HOME/log/supervisor/monica/proxy`  | `/var/log`                  | Proxy logs          |
| `$HOME/log/supervisor/monica/worker` | `/var/log`                  | Worker logs         |
| `/shared/data/monica/climate-data`   | `/monica_data/climate-data` | Shared climate data |

The host directories must be accessible and writable by the user running Singularity.

---

## 5. Run the Proxy

The proxy exposes the following ports:

| Port | Purpose                  |
|------|--------------------------|
| 6677 | Worker input connection  |
| 7788 | Worker output connection |
| 7777 | Proxy consumer endpoint  |
| 6666 | Proxy producer endpoint  |

Create `run_monica_proxy.sh`:

```shell
#!/bin/bash -x
#SBATCH --job-name=monica_proxy
#SBATCH --ntasks=1
#SBATCH --time=7-00:00:00
#SBATCH --partition=long
#SBATCH --output=monica_proxy_%x_%j.out
#SBATCH --error=monica_proxy_%x_%j.err

VERSION=2.2.1.170
IMAGE=/shared/apps/monica/monica-cluster_${VERSION}.sif

LOG_DIR="$HOME/log/supervisor/monica/proxy"
mkdir -p "$LOG_DIR"

export SINGULARITYENV_monica_intern_in_port=6677
export SINGULARITYENV_monica_intern_out_port=7788
export SINGULARITYENV_monica_consumer_port=7777
export SINGULARITYENV_monica_producer_port=6666

export SINGULARITYENV_monica_autostart_proxies=true
export SINGULARITYENV_monica_autostart_worker=false
export SINGULARITYENV_monica_auto_restart_proxies=true
export SINGULARITYENV_monica_auto_restart_worker=false

# Run in the foreground so that SLURM manages the service lifetime
exec singularity run -B "$LOG_DIR":/var/log "$IMAGE"
```

Submit the proxy job:

```bash
sbatch run_monica_proxy.sh
```

Find the node running the proxy:

```bash
squeue -u "$USER" -n monica_proxy -o "%.18i %.20j %.20 B %.10T"
```

Use the node hostname as `PROXY_SERVER` in the worker script. Confirm that the hostname is resolvable and reachable from compute nodes.

Stop the proxy by cancelling its job:

```bash
scancel <proxy-job-id>
```

---

## 6. Run Workers with SLURM

Create `run_monica_worker.sh`:

```shell
#!/bin/bash -x
#SBATCH --job-name=monica_worker
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=10
#SBATCH --time=24:00:00
#SBATCH --partition=compute
#SBATCH --output=monica_worker_%x_%j.out
#SBATCH --error=monica_worker_%x_%j.err

VERSION=2.2.1.170
IMAGE=/shared/apps/monica/monica-cluster_${VERSION}.sif

# Replace this with the hostname or IP address of the proxy node
PROXY_SERVER=<proxy-hostname-or-ip>

MOUNT_DATA=/shared/data/monica/climate-data
MOUNT_LOG="$HOME/log/supervisor/monica/worker"

DATADIR=/monica_data/climate-data
LOGOUT=/var/log

mkdir -p "$MOUNT_LOG"

NUM_WORKER="${SLURM_CPUS_PER_TASK:-10}"

export SINGULARITYENV_monica_instances="$NUM_WORKER"
export SINGULARITYENV_monica_intern_in_port=6677
export SINGULARITYENV_monica_intern_out_port=7788

export SINGULARITYENV_monica_proxy_in_host="$PROXY_SERVER"
export SINGULARITYENV_monica_proxy_out_host=$PROXY_SERVER

export SINGULARITYENV_monica_autostart_proxies=false
export SINGULARITYENV_monica_autostart_worker=true
export SINGULARITYENV_monica_auto_restart_proxies=false
export SINGULARITYENV_monica_auto_restart_worker=true

srun --ntasks=1 singularity run -B "$MOUNT_DATA:$DATADIR,$MOUNT_LOG:$LOGOUT" --pwd / "$IMAGE"
```

Submit the worker job:

```bash
sbatch run_monica_worker.sh
```

Each worker job starts `SLURM_CPUS_PER_TASK` MONICA worker processes. Ensure that this matches the resources allocated by the scheduler.

To start multiple independent worker jobs:

```bash
for i in {1..10}; do
  sbatch run_monica_worker.sh
done
```

---

## 7. Run a worker without a scheduler

On a node where direct execution is permitted:

```shell
VERSION=2.2.1.170
IMAGE=/shared/apps/monica/monica-cluster_${VERSION}.sif

PROXY_SERVER=<proxy-hostname-or-ip>
MOUNT_DATA=/shared/data/monica/climate-data
MOUNT_LOG="$HOME/log/supervisor/monica/worker"

mkdir -p "$MOUNT_LOG"

export SINGULARITYENV_monica_instances=10
export SINGULARITYENV_monica_intern_in_port=6677
export SINGULARITYENV_monica_intern_out_port=7788
export SINGULARITYENV_monica_proxy_in_host="$PROXY_SERVER"
export SINGULARITYENV_monica_proxy_out_host="$PROXY_SERVER"

export SINGULARITYENV_monica_autostart_proxies=false
export SINGULARITYENV_monica_autostart_worker=true
export SINGULARITYENV_monica_auto_restart_proxies=false
export SINGULARITYENV_monica_auto_restart_worker=true

exec singularity run -B "$MOUNT_DATA:/monica_data/climate-data,$MOUNT_LOG:/var/log" --pwd / "$IMAGE"
```

This process remains attached to the terminal. Use the HPC scheduler or an approved service mechanism for persistent production workloads.

---

## 8. Troubleshooting

Check the proxy logs:

```bash
ls -la "$HOME/log/supervisor/monica/proxy"
```

Check the worker logs:
```bash
ls -la "$HOME/log/supervisor/monica/worker"
```

Verify network connectivity from a compute node:

```bash
nc -vz <proxy-hostname-or-ip> 6677
nc -vz <proxy-hostname-or-ip> 7788
```

Common problems include:

- using `localhost` as `PROXY_SERVER` when the proxy runs on another node
- using a climate-data path that is not shared across compute nodes
- insufficient permissions on the mounted log directory
- firewall rules blocking ports `6677` or `7788`
- requesting fewer CPUs that the configured `monica_instances` value
- using a Docker image tag that does not exist