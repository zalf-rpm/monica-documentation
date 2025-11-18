# Troubleshooting Common Installation Issues

This section lists the common problems users may encounter during installation or setup of MONICA, along with solutions.  
It covers Python environment issues, dependency conflicts, Docker/Singularity problems, and configuration file errors.

---

## 1. Python / Environment Issues

### **Error:** `ModuleNotFoundError: No module named 'numpy'`
**Cause:** Dependencies not installed.  

**Fix:**

```bash
pip install -r requirements.txt
```

### **Error:** `MONICA fails on Python 3.12 or lower than 3.8`

**Cause:** Use Python 3.8–3.11 for full compatibility with MONICA’s required libraries.

**Fix:** Create a virtual environment with a supported version:

```bash
conda create -n monica python=3.10
conda activate monica
```

### **Error:** `pip: command not found`

**Cause:** Python installation missing or PATH not updated.

**Fix:**
- Reinstall Python and select “Add Python to PATH”

- Or call pip directly:

```bash
python -m pip install -r requirements.txt
```

## 2. Virtual Environment Issues

### **Error:** Environment not activating `(command not found: activate)`

**Fix:** (Windows):

```bash
monica-env\Scripts\activate
```
**Fix:** (Linux):

```bash
source monica-env/bin/activate
```
### **Error:** Dependencies conflict when installing

**Fix:** (clean reinstallation):

```bash
rm -rf monica-env
python -m venv monica-env
pip install -r requirements.txt
```
## 3. Docker Issues

### **Error**: docker: `command not found`

Install Docker from the official documentation:
[Get Docker](https://docs.docker.com/get-docker/)

### **Error:** Permission denied running Docker

**Fix:**  (Linux):

```bash
sudo usermod -aG docker $USER
newgrp docker
```
### **Error:** Container cannot bind ports 6666 or 7777

**Fix:** 

```bash
docker ps
docker stop <container_id>
```

## 4. Singularity Issues (HPC)

### **Error:** FATAL: `container creation failed`

**Fix:**

- Store the `.sif` file in your home directory.
- Check bind paths:

```bash
singularity exec -B /scratch:/scratch monica.sif ls /
```
### **Error:** Failed to bind climate data folder

**Fix:**
Make sure the directory exists on the host:

```bash
ls /shared/data/monica/climate-data
```

### **Error:** Ports 6666/7777/8888 unavailable

Proxy already running or firewall blocking.

**Fix:**

```bash
singularity instance list
singularity instance stop monica_proxy
```

## 5. HPC (SLURM) Issues

### **Error:** sbatch: command not found

SLURM module not loaded.

**Fix:**
```bash
module load slurm
```

### **Error:** Job stuck in PENDING

Check reasons:

```bash
squeue -u $USER
```

Common causes:
- Partition full
- Not enough CPUs or memory
- Wrong partition name

### **Error:** Worker cannot connect to proxy

Verify:

- Correct proxy node hostname (FQDN)

- Internal ports match:

   `monica_intern_in_port=6677`

   `monica_intern_out_port=7788`

- Network visibility between cluster nodes

## 6.  MONICA Server / Proxy Issues

### **Error:** Connection refused

Proxy not running.

**Fix:** Start the proxy first (Docker, Singularity, or ZMQ mode).

### **Error:** Worker receives no tasks

**Cause:** Proxy autostart is disabled.

**Fix:**

```bash
export monica_autostart_proxies=true
```

### **Error:** JSON decode errors

Usually caused by malformed sim.json, crop.json, or site.json.

**Fix:** Validate JSON:

```bash
python -m json.tool sim.json
```

### **Error:** Wrong parameter-set linked to config

Double-check:

- sim.json → correct project

- crop.json → correct crop + rotation

- site.json → correct soil + climate data

## 7. File / Permission Issues

### **Error:** Permission denied writing logs

**Fix:** Create the required directories:

```bash
mkdir -p ~/log/supervisor/monica/proxy
mkdir -p ~/log/supervisor/monica/worker
```

### **Error:** Cannot write to /var/log inside the container

Always bind a writable host directory:

```bash
-B ~/log/supervisor/monica:/var/log
```

## 8. Climate / Data Issues 

### **Error:** “Climate file not found”

Check:

- The path in sim.json
- File exists in the mounted folder
- Docker/Singularity bind paths include the climate directory

### **Error:** Wrong climate file format

Ensure:

- CSV headers match MONICA expectations
- Required variables: Tmin, Tmax, precip, radiation, etc.

## 9. Miscellaneous Issues

### **Error:** `monica-zmq-server: command not found`

MONICA not installed or PATH not set.

**Fix:** Run via Python:

```bash
python -m monica.zmq.server
```

### **Error:** Missing environment variables

Check that all required environment variables are exported:

- `monica_intern_in_port`
- `monica_intern_out_port`
- `monica_consumer_port`
- `monica_producer_port`



