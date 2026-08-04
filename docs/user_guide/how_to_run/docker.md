# Running MONICA in Docker

## Overview

MONICA provides a project-published Docker image for running a MONICA cluster with:

- an input proxy
- an output proxy
- a configurable number of MONICA worker processes

The image is intended for multi-core systems and server environments where MONICA runs as a service.

Docker image:

```
zalfrpm/monica-cluster
```

Available tags can be found on [Docker Hub](https://hub.docker.com/r/zalfrpm/monica-cluster)

The current repository Dockerfile is based on Debian 13. The base image may differ for older image tags.

---

## Requirements

Install Docker:

- Windows: [Docker Desktop](https://www.docker.com/products/docker-desktop)
- Linux: Docker Engine from your distribution's package manager

Verify the installation:

```
docker --version
```

---

## Ports

The cluster exposes two external ports:

| Service      | Container port | Purpose                             |
|--------------|----------------|-------------------------------------|
| Input proxy  | 6666           | Receives MONICA simulation requests |
| Output proxy | 7777           | Provides MONICA simulation results  |

The image also uses internal proxy hosts `6677` and `7788`. These normally do not need to be published to the host.

---

## Basic Usage

```
docker run -p <host-input-port>:6666 -p <host-output-port>:7777 --env monica_instances=<number-of-workers> --rm --name <container-name> zalfrpm/monica-cluster:<version-tag>
```

Example:

The following command starts a cluster with nine MONICA workers:

```
docker run -p 6666:7777 -p 7777:7777 --env monica_instances=9 --rm --name my-monica-cluster zalfrpm/monica-cluster:<version-tag>
```

This configuration provides:

- input proxy at host port `6666`
- output proxy at host port `7777`
- nine MONICA worker processes
- container name `my-monica-cluster`


Replace `<version-tag>` with a tag available on [Docker Hub](https://hub.docker.com/r/zalfrpm/monica-cluster), for example:

```
3.6.60.sand_or_clay_0_fix
```

Use a specific version tag for reproducible runs. The `latest` tag may point to a newer image in the future and can therefore produce different results.

---

## Publishing Random Host Ports

If you want Docker to select random host ports, omit the host-port portion of the mapping:

```
docker run -p 6666 -p 7777 --env monica_instance=9 --rm --name my-monica-cluster zalfrpm/monica-cluster:<version-tag>
```

Find the assigned ports with:

```
docker port my-monica-cluster
```

If the `-p` options are omitted entirely, the ports are not published to the host and cannot normally be accessed from outside the container.

---

## Local-Only Access

Docker publishes ports to all host interfaces by default. To restrict access to the local machine, bind the ports to `127.0.0.1`:

```
docker run -p 127.0.0.1:6666:6666 -p 127.0.0.1:7777:7777 --env monica_instance=9 --rm --name my-monica-cluster zalfrpm/monica-cluster:<version-tag>
```

---

## Pull the Image Before Running

To download a specific image tag:

```
docker pull zalfrpm/monica-cluster:<version-tag>
```

For example:

```
docker pull zalfrpm/monica-cluster:3.6.60.sand_or_clay_0_fix
```

---

## Monitor the Container

List running containers:

```
docker ps
```

View the container logs:

```
docker logs -f my-monica-cluster
```

Display the published port mappings:

```
docker port my-monica-cluster
```

---

## Stop the Container

The standard command runs in the foreground. Press `Ctrl + C` in the terminal running Docker, or stop the container from another terminal:

```
docker stop my-monica-cluster
```

Because the command uses `--rm`, Docker removes the container automatically after it stops.

---

## Run in the Background

To run the cluster in detached mode, add `--detach` or `-d`:

```
docker run -p 6666:6666 -p 7777:7777 --env monica_instances=9 --rm --name my-monica-cluster -d zalfrpm/monica-cluster:<version-tag>
```

Check its status and logs:

```
docker ps
docker logs -f my-monica-cluster
```

---

## Restart the Cluster

Since `-rm` removes the container when it stops, restart if by running the `docker run` command again:

```
docker run -p 6666:6666 -p 7777:7777 --env monica_instance=9 --rm --name my-monica-cluster zalfrpm/monica-cluster:<version-tag>
```

Choose `monica_instance` according to the available CPU cores and memory. More workers do not necessarily improve performance if the host is resource-constrained.