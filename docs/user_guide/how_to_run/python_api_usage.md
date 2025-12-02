# Python API Usage examples

This section shows how to use the official MONICA Python bindings to run MONICA directly from Python. It covers installation, loading input JSON files, running single and multiple simulations, and handling outputs in a reproducible workflow.

## 1. Installation

The MONICA Python API is available via pip (if published) or can be installed from source.

**Install from PIP**

```bash
pip install monica
```
**Install from Source**

```bash
git clone https://github.com/zalf-rpm/monica.git
cd monica
pip install .
```

!!! note ""
   **Note** Python ≥ 3.8 is recommended.

---

## 2. Basic Usage: Running a Single Simulation

Below is a minimal example that loads JSON configuration files and executes a MONICA simulation in memory.

```bash
from monica import run_monica
import json

# Load configuration files
with open("config/general.json") as f:
    general_cfg = json.load(f)

with open("config/site.json") as f:
    site_cfg = json.load(f)

with open("config/crop.json") as f:
    crop_cfg = json.load(f)

with open("config/simulation.json") as f:
    sim_cfg = json.load(f)

# Combine all configurations
config = {
    "general": general_cfg,
    "site": site_cfg,
    "crop": crop_cfg,
    "simulation": sim_cfg
}

# Run MONICA
results = run_monica(config)

# Print selected outputs
print(results["Yield"])
print(results["GHG"])
```

---

## 3. Running Multiple Scenarios (Batch Mode)

Example: Loop over nitrogen levels

```bash
from monica import run_monica
import json

with open("config/simulation.json") as f:
    sim_cfg = json.load(f)

N_levels = [80, 120, 160, 200]
outputs = []

for N in N_levels:
    sim_cfg["CropParameters"]["FertiliserAmount"] = N

    full_cfg = {
        "general": json.load(open("config/general.json")),
        "site": json.load(open("config/site.json")),
        "crop": json.load(open("config/crop.json")),
        "simulation": sim_cfg
    }

    out = run_monica(full_cfg)
    outputs.append({"N": N, "yield": out["Yield"]})

print(outputs)
```

---

## 4. Using Multiple Climate Files

```bash
import glob, json
from monica import run_monica

climate_files = glob.glob("climate/*.json")
results = []

for cf in climate_files:
    cfg = {
        "general": json.load(open("config/general.json")),
        "site": json.load(open("config/site.json")),
        "crop": json.load(open("config/crop.json")),
        "simulation": json.load(open("config/simulation.json"))
    }

    cfg["general"]["climate-data-file"] = cf
    out = run_monica(cfg)

    results.append({
        "climate_file": cf,
        "yield": out["Yield"],
        "nitrogen_balance": out["NBalance"]
    })
```

---

## 5. Saving Output to CSV (Pandas)

```bash
import pandas as pd
from monica import run_monica
import json

cfg = {
    "general": json.load(open("config/general.json")),
    "site": json.load(open("config/site.json")),
    "crop": json.load(open("config/crop.json")),
    "simulation": json.load(open("config/simulation.json"))
}

out = run_monica(cfg)

df = pd.DataFrame([out])
df.to_csv("monica_output.csv", index=False)
```

## 6. Error Handling

```bash
from monica import run_monica, MonicaError

try:
    results = run_monica(config)
except MonicaError as e:
    print("MONICA failed:", e)
```

---

## 7. Tips for Efficient Python Integration

 - Static JSON configurations should be loaded before loops.

 - Use multiprocessing (multiprocessing.Pool) for large scenario sets.

 - Validate JSON files with MONICA schemas before execution.

 - To cut down on I/O time, store climate files locally.

 - Use HPC, Docker, or CLI options for large-scale campaigns.

