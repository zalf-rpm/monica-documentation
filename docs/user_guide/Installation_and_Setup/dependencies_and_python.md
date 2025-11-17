# Dependencies & Python Version (requirements.txt)

MONICA is written in Python and requires specific libraries for data handling, communication, and model execution.  
This section explains how to prepare Python environment and install all dependencies.

---

## 1. Recommended Python Version

Use **Python ≥ 3.8** (tested with Python 3.8 – 3.11).

> Using older versions (≤3.7) or newer releases (≥3.12) may lead to dependency conflicts or unexpected errors.

It is recommended to create a dedicated virtual environment for MONICA to keep dependencies isolated from your system Python.

Create a virtual environment

```
python -m venv monica-env
```

Activate the environment

```
# Windows
monica-env\Scripts\activate

# Linux / macOS
source monica-env/bin/activate
```

## 2. Install Dependencies

All required Python packages are listed in the `requirements.txt` file located in the MONICA repository root.

Install them with:

```
pip install -r requirements.txt
```
This will install all necessary libraries such as:

- numpy, pandas, scipy – numerical & data handling

- pyzmq – ZeroMQ communication (for proxy / worker mode)

- flask or fastapi – web service support (optional)

- matplotlib / seaborn – visualization (optional)

**Example: requirements.txt**

The following example shows the typical dependencies used in MONICA:

```
# MONICA simulation environment dependencies

numpy>=1.19
pandas>=1.3
scipy>=1.7
pyzmq>=23.0
psutil>=5.8
tqdm>=4.62

# Optional service / API / visualization layers
flask>=2.0
fastapi>=0.85
uvicorn>=0.17
gunicorn>=20.1
matplotlib>=3.4
seaborn>=0.11
requests>=2.26
pydantic>=1.8
```


## 3. Updating Dependencies

To update existing installations to the latest compatible versions:

```
pip install -U -r requirements.txt
```

## 4. Verifying Installation

Check that MONICA and its dependencies are correctly installed:

```
python -m monica --version
```
You should see the current MONICA version printed in their terminal.

> Tip: If you are planning to run MONICA on HPC or Singularity, install dependencies inside the container or virtual environment before running simulations.