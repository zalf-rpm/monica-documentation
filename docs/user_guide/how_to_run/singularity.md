# Running MONICA on HPC with Singularity

This guide explains how to run the MONICA agro-ecosystem model in a High-Performance Computing (HPC) environment using Singularity. It follows best practices for distributed runs: a proxy service (I/O handler) and multiple worker instances (simulation executors).

## 1. Overview

MONICA runs in a **two-layer architecture** within an HPC (High-Performance Computing) environment.

| Component | Role | Typical Host |
|------------|------|---------------|
| **Proxy** | Manages input/output, job coordination, and communication between client and workers. | Login or head node (long-running service). |
| **Workers** | Execute the MONICA simulations based on tasks assigned by the proxy. | Compute nodes (parallel jobs, e.g., submitted via SLURM). |

The same **Singularity container image** is used for both the proxy and the worker components:

```
monica-cluster_<tag>.sif
```

## 2. Build the Singularity Image

Pull the container image directly from Docker Hub:

```
singularity pull docker://zalfrpm/monica-cluster:<tag>
```
This builds a **Singularity image using the Docker image as a blueprint**.

**Note:** Replace tag with the desired MONICA version (e.g., 3.6.32).

Resulting image:

```
monica-cluster_<tag>.sif
```
Store this .sif in a shared, read-only location accessible to all compute nodes, e.g.:

```
/shared/apps/monica/monica-cluster_2.2.1.170.sif
```
## 3. Configure Shared Directories

Before launching, create directories accessible from all nodes:

```
mkdir -p ~/log/supervisor/monica/proxy
mkdir -p ~/log/supervisor/monica/worker
mkdir -p /shared/data/monica/climate-data
```

- /log/supervisor/monica — persistent logs

- /shared/data/monica/climate-data — shared input data accessible to all nodes

## 4. Run the Proxy (Head Node)

The proxy should run as a persistent Singularity instance on a node reachable by workers.

Create `run_monica_proxy.sh`:

```
#!/bin/bash -x
#SBATCH --job-name=monica_proxy
#SBATCH --output=~/log/supervisor/monica/proxy/%x_%j.out
#SBATCH --error=~/log/supervisor/monica/proxy/%x_%j.err
#SBATCH --ntasks=1
#SBATCH --time=7-00:00:00   # run for up to 7 days
#SBATCH --partition=long    # adjust to your HPC queue

VERSION=2.2.1.170 # or higher
SINGULARITY_IMAGE=/shared/apps/monica/monica-cluster_${VERSION}.sif

# Log binding
LOGOUT=/var/log
MOUNT_LOG=~/log/supervisor/monica/proxy

# MONICA environment
export SINGULARITYENV_monica_intern_in_port=6677
export SINGULARITYENV_monica_intern_out_port=7788
export SINGULARITYENV_monica_consumer_port=7777
export SINGULARITYENV_monica_producer_port=6666

export SINGULARITYENV_monica_autostart_proxies=true
export SINGULARITYENV_monica_autostart_worker=false
export SINGULARITYENV_monica_auto_restart_proxies=true
export SINGULARITYENV_monica_auto_restart_worker=false

# Start instance and run proxy
singularity instance start -B $MOUNT_LOG:$LOGOUT ${SINGULARITY_IMAGE} monica_proxy
nohup singularity run instance://monica_proxy > /dev/null 2>&1 &
```
Submit it:

```
sbatch run_monica_proxy.sh
```

To stop the proxy later:

```
singularity instance stop monica_proxy
```
## 5. Run Workers
Workers connect to the proxy and execute simulations. Each worker node can host multiple MONICA worker processes.

Create `run_monica_worker.sh`:

```
#!/bin/bash -x
#SBATCH --job-name=monica_worker
#SBATCH --output=~/log/supervisor/monica/worker/%x_%j.out
#SBATCH --error=~/log/supervisor/monica/worker/%x_%j.err
#SBATCH --cpus-per-task=10
#SBATCH --time=24:00:00
#SBATCH --partition=compute

VERSION=2.2.1.170
SINGULARITY_IMAGE=/shared/apps/monica/monica-cluster_${VERSION}.sif

# Data mounts
MOUNT_DATA=/shared/data/monica/climate-data
DATADIR=/monica_data/climate-data

# Logging
LOGOUT=/var/log
MOUNT_LOG=~/log/supervisor/monica/worker

# Worker configuration
NUM_WORKER=$SLURM_CPUS_PER_TASK
PROXY_SERVER=<proxy_host_or_IP>   # replace with proxy node hostname

INTERN_IN_PORT=6677
INTERN_OUT_PORT=7788

# Environment variables
export SINGULARITYENV_monica_instances=$NUM_WORKER
export SINGULARITYENV_monica_intern_in_port=${INTERN_IN_PORT}
export SINGULARITYENV_monica_intern_out_port=${INTERN_OUT_PORT}
export SINGULARITYENV_monica_proxy_in_host=$PROXY_SERVER
export SINGULARITYENV_monica_proxy_out_host=$PROXY_SERVER

export SINGULARITYENV_monica_autostart_proxies=false
export SINGULARITYENV_monica_autostart_worker=true
export SINGULARITYENV_monica_auto_restart_proxies=false
export SINGULARITYENV_monica_auto_restart_worker=true

# Execute MONICA worker inside container
srun singularity run \
  -B $MOUNT_DATA:$DATADIR,$MOUNT_LOG:$LOGOUT \
  --pwd / \
  ${SINGULARITY_IMAGE}
```
Submit to SLURM:

```
sbatch run_monica_worker.sh
```
**Note:**  If you do not use a job scheduler such as SLURM, you can start the worker manually as a Singularity service using:

```
singularity instance start -B $MOUNT_LOG:$LOGOUT ${SINGULARITY_IMAGE} monica_worker
```



