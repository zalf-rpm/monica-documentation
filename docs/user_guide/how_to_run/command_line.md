# Running MONICA from the Command Line (CLI & CMD)

This section explains how to run MONICA as a standalone program using:
- **Windows CMD / PowerShell**
- **Linux Terminal**

The commands are the same on all systems, except for file paths (`\` vs `/`) and optional `.exe` suffix on Windows.

---

## 1. Available MONICA Executables

MONICA installs several command-line tools:

| Executable | Purpose |
|-----------|---------|
| **monica-run** | Run a standalone MONICA simulation using a `sim.json` file |
| **monica-zmq-proxy** | Distribute jobs to MONICA workers using ZeroMQ |
| **monica-zmq-server** | Worker process for ZeroMQ-based execution |
| **monica-capnp-proxy** | Proxy using Cap’n Proto, able to start many MONICA threads |
| **monica-capnp-server** | MONICA server using Cap’n Proto RPC |

Most users only need **`monica-run`** for local simulations.

---

## 2. Basic Usage

A MONICA simulation is started by passing a single `sim.json` file:

### **Windows CMD**

```cmd
monica-run.exe Examples\Hohenfinow2\sim.json > out.csv
```
### **Linux**

```bash
monica-run Examples/Hohenfinow2/sim.json > out.csv
```

This runs the Hohenfinow2 example and writes output to out.csv.

---

### 3. Command-Line Options (monica-run -h)

To view all available flags:

```bash
monica-run -h
```

Typical output:

```
monica-run [options] path-to-sim.json

Options:
 -h  | --help                       Show this help message
 -v  | --version                    Show version information
 -d  | --debug                      Print debug output
 -w  | --write-output-files         Write MONICA output files
 -op | --path-to-output DIR         Output directory (default: .)
 -o  | --path-to-output-file FILE   Path to output file
 -c  | --path-to-crop FILE          Path to crop.json   (default: ./crop.json)
 -s  | --path-to-site FILE          Path to site.json   (default: ./site.json)
 -cl | --path-to-climate FILE       Path to climate.csv (default: ./climate.csv)
```

**Important:**
Any parameter provided via CLI overrides values inside sim.json.

---

### 4. Examples

#### 4.1 Run a simulation with debug messages

For Windows:

```bash
monica-run.exe -d Examples\Hohenfinow2\sim.json
```

For Linux:

```bash
monica-run -d Examples/Hohenfinow2/sim.json
```

#### 4.2 Write results to a custom output file

```bash 
monica-run -o results.csv Examples/Hohenfinow2/sim.json
```

#### 4.3 Override crop, site, or climate file

For windows:

```bash
monica-run ^
  -c alt_crop.json ^
  -s alt_site.json ^
  -cl alt_climate.csv ^
  Examples\Hohenfinow2\sim.json
```
*(Windows CMD uses ^ for line breaks; Linux uses \.)*

For Linux:

```bash
monica-run \
  -c alt_crop.json \
  -s alt_site.json \
  -cl alt_climate.csv \
  Examples/Hohenfinow2/sim.json
```

---

### 5. Windows CMD Notes
Adding MONICA to PATH

If monica-run is not recognized, add installation folder to PATH:

```bash
setx PATH "%PATH%;C:\Users\%USERNAME%\MONICA"
```

Running from PowerShell

```bash
.\monica-run.exe sim.json
```

Path separators

Windows: \

Linux/macOS: /

---

### 6. Directory-Based Execution

If all required JSON files (sim.json, site.json, crop.json) are inside a folder:

```bash
monica-run config/sim.json
```

Folder structure example:

```bash
config/
├── sim.json
├── site.json
├── crop.json
└── climate.csv
```
---

### 8. Converting Old HERMES-Style Configuration to the New MONICA JSON Format

After installing MONICA version **≥ 2.0.1**, a folder named **`monica-ini-to-json`** is included in the installation directory.  
This folder contains a **Python 2.7 conversion script** that reads a classic HERMES-style `monica.ini` file (including crop, fertilizer, irrigation, and weather files) and generates the corresponding MONICA JSON configuration files:

- `sim.json`
- `site.json`
- `crop.json`
- `climate.csv`

---

#### 8.1 Requirements

You must have **Python 2 (2.7)** installed.

---

### 8.2 Running the Conversion Script

Navigate to the `monica-ini-to-json` directory and run:

**Windows CMD Example:**
```cmd
C:\Users\USER-NAME\MONICA\monica-ini-to-json>python monica-ini-to-json.py monica.ini=PATH-TO-monica.ini
```

The script automatically uses the default template filevwhich is located in the same folder:

```pgsql
conversion-template-sim.json
```

#### 8.3 Viewing Available Options

To see all parameters run:

```pgsql
usage: [python] monica-ini-to-json.py 
       [monica.ini=path-to-monica.ini] 
       [sim.json=path-to-template-sim.json] 
       [out-suffix=suffix-to-append-to-sim-site-crop-climate.files]

defaults: {
  'out-suffix': '-from-monica-ini',
  'monica.ini': './monica.ini',
  'sim.json': './conversion-template-sim.json'
}
```

You can run this script from any directory, but then you must provide the full path to the template `sim.json`.

#### 8.4 Output Naming and File Safety

The converter automatically adds a suffix (default: `-from-monica-ini`) to prevent overwriting existing files:

Example outputs:

```pgsql
sim-from-monica-ini.json
site-from-monica-ini.json
crop-from-monica-ini.json
climate-from-monica-ini.csv
```
To disable the suffix

```ini
out-suffix=""
```
#### 8.5 Climate File Handling

The converter assembles all weather data into a single `climate.csv` file.

Handling rules:

- If global radiation values in the HERMES files are all zero, then sunhours will be included.

- Otherwise, only global radiation (globrad) is written.

- Only the weather records between startdate and enddate (as defined in `monica.ini`) are included.

#### 8.6 Year Interpretation (Century Switching)

Two global parameters control how 2-digit years in HERMES files are interpreted:

```python
YEAR_BORDERS = [[0, 30, 2000], [31, 99, 1900]]
YEAR_SUFFIX_DIGITS = 3
```

Explanation:

- `YEAR_SUFFIX_DIGITS = 3`
        → weather file names end with the last 3 digits of the year.

- YEAR_BORDERS defines the century:

        - Years ending 00–30 → interpreted as 2000–2030

        - Years ending 31–99 → interpreted as 1931–1999

Examples:

- `020506` → interpreted as **2006-05-02**

- `020598` → interpreted as **1998-05-02**

#### 8.7 Important Notes and Limitations

The converter cannot detect all conditions automatically. Users must check the output manually.

Key points:

- If ``incorporate = 1` in the HERMES fertilizer file → converter creates an **OrganicFertilization** workstep.

- If `incorporate = 0` → converter creates a **MineralFertilization** workstep.

- Crop and fertilizer names are linked via MONICA’s reference mechanism:

```css
["ref", "xxx", "name"]
```


Ensure the referenced names exist in the template or adjust manually.

- If crop or fertilizer references are correct, the generated JSON files should work immediately with MONICA.


The `monica-ini-to-json` script provides an automated way to migrate from old HERMES `.ini` configurations to the MONICA v2 JSON schema. Users must review the outputs for correctness, but the script greatly simplifies transition to the modern MONICA format.
