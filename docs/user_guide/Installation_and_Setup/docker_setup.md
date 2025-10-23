# Docker setup

## 1. Overview

MONICA provides an official Docker image — **`zalfrpm/monica-cluster`** — built on **Debian 9**.  
The container runs MONICA with:
- an **input proxy**
- an **output proxy**
- and a configurable number of **MONICA worker processes**

Each container can act as a standalone MONICA “cluster” on a multi-core machine.

---

## 2. Pull the Docker Image

Download the official image from Docker Hub:

```
docker pull zalfrpm/monica-cluster:<tag>
```
