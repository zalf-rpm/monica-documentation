# Dependencies & Python Version (requirements.txt)

MONICA requires a working **Python environment** with compatible libraries to run its simulation engine and tools.

## 1. Recommended Python Version

Use **Python ≥ 3.8** (tested with Python 3.8 – 3.11).

> It is strongly recommended to use a virtual environment to keep dependencies isolated.

Create one using:

```
python -m venv monica-env
source monica-env/bin/activate      # Linux / macOS
monica-env\Scripts\activate         # Windows
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