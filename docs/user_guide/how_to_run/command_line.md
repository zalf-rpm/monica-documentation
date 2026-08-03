# Running MONICA from the Command Line

MONICA can be run as a standalone command-line program on Windows and Linux.

The main executable is `monica-run` which runs a simulation from a `sim.json` configuration file.

---

## 1. MONICA Executables

The build and Windows installer provide the following executables:

| Executable                   | Purpose                                     |
|------------------------------|---------------------------------------------|
| `monica-run`                 | Run a local MONICA simulation               |
| `monica-zmq-proxy`           | Forward and distribute ZeroMQ jobs          |
| `monica-zmq-server`          | Process MONICA jobs received through ZeroMQ |
| `monica-capnp-proxy`         | Forward Cap'n Proto RPC requests            |
| `monica-capnp-server`        | Run MONICA through Cap'n Proto RPC          |
| `monica-capnp-fbp-component` | MONICA Cap'n Proto FBP component            |

Most users running a local simulation only need **`monica-run`**.

---

## 2. Basic Usage

A simulation is started by passing a `sim.json` file.

=== "Windows CMD"

    ```cmd
    monica-run.exe Examples\Hohenfinow2\sim.json
    ```

=== "PowerShell"

    ```powershell
    .\monica-run.exe Examples\Hohenfinow2\sim.json
    ```

=== "Linux"

    ```bash
    ./monica-run Examples/Hohenfinow2/sim.json
    ```

The paths to `crop.json`, `site.json`, and `climate.csv` are normally read from `sim.json`. Relative paths are resolved to the directory containing `sim.json`.

For the repository examples, use:

```
./monica-run installer/Hohenfinow2/sim.json
```

On Windows, the standard installer places the examples in:

```
%USERPROFILE\MONICA\Examples\Hohenfinow2
```

---

## 3. Output Files

The recommended way to write output to a specific file is the `-o` option:

```
monica-run.exe -o results.csv Examples\Hohenfinow2\sim.json
```

```
./monica-run -o results.csv Examples/Hohenfinow2/sim.json
```

The output file format and default filename are controlled by the `output` section in `sim.json`.

MONICA also writes simulation output to standard output when no output file is configured. Standard-output redirection is therefore possible:

```
./monica-run Examples/Hohenfinow2/sim.json > results.csv
```

However, using `-o` is preferred because the simulation configuration may already enable file output.

---

## 4. Command-Line Options

Use `-h` or `--help` to display the available options:

```
monica-run -h
```

Current options include:

| Flag  | Long Flag                       | Description                       |
|-------|---------------------------------|-----------------------------------|
| `-h`  | `--help`                        | Show help                         |
| `-v`  | `--version`                     | Show version information          |
| `-d`  | `--debug`                       | Enable debug output               |
| `-sd` | `--start-date ISO-DATE`         | Override the climate start date   |
| `-ed` | `--end-date ISO-DATE`           | Override the climate end date     |
| `-m`  | `--write-multiple-output-files` | Write one file per output section |
| `-op` | `--path-to-output DIRECTORY`    | Specify an output directory       |
| `-o`  | `--path-to-output-file FILE`    | Specify an output file            |
| `-c`  | `--path-to-crop FILE`           | Specify the crop file             |
| `-s`  | `--path-to-site FILE`           | Specify the site file             |
| `-w`  | `--path-to-climate FILE`        | Specify the climate file          |

!!! note

    - Command-line crop, site, and climate paths override the corresponding values from `sim.json`.

---

## 5. Examples

### Enable debug output

=== "Windows CMD"

    ```cmd
    monica-run.exe -d Examples\Hohenfinow2\sim.json
    ```

=== "PowerShell"

    ```powershell
    .\monica-run.exe -d Examples\Hohenfinow2\sim.json
    ```

=== "Linux"

    ```bash
    ./monica-run -d Examples/Hohenfinow2/sim.json
    ```

### Select an output file

```bash 
monica-run -o results.csv Examples/Hohenfinow2/sim.json
```

### Override crop, site, and climate files

=== "Windows CMD"

    ```cmd
    monica-run.exe -c alt_crop.json -s alt_site.json -w alt_climate.csv Examples\Hohenfinow2\sim.json
    ```

=== "PowerShell"

    ```powershell
    .\monica-run.exe -c alt_crop.json -s alt_site.json -w alt_climate.csv Examples\Hohenfinow2\sim.json
    ```

=== "Linux"

    ```bash
    monica-run -c alt_crop.json -s alt_site.json -w alt_climate.csv Examples/Hohenfinow2/sim.json
    ```

### Restrict the simulation period

```
monica-run -sd 1995-01-01 -ed 1995-12-31 Examples/Hohenfinow2/sim.json
```

### Write one file per output section

```
monica-run -m Examples/Hohenfinow2/sim.json
```

---

## 6. Windows Installation and PATH

The Windows installer adds the MONICA executable directory to the user `PATH`.

After installation, open a new CMD or PowerShell window before running MONICA.

If MONICA is not found, run it using its full installation path or add the installation directory to the user PATH manually. The executable installation directory is normally:

```
%LOCALAPPDATA%\MONICA
```

The examples, parameters, and conversion tools are installed below:

```
%USERPROFILE%\MONICA
```

For PowerShell, an executable in the current directory must normally be prefixed with `.\`:

```
.\monica-run.exe .\sim.json
```

Windows uses `\` as the usual path separator. Linux uses `/`.

---

## 7. Directory-Based Execution

A typical simulation directory contains:

```
config/
├── sim.json
├── site.json
├── crop.json
└── climate.csv
```

The `sim.json` file should reference the other files, for example:

```json
{
  "crop": "crop.json",
  "site": "site.json",
  "climate": "climate.csv"
}
```

Run the simulation with:

```
monica-run config/sim.json
```

Relative crop, site, and climate paths are resolved relative to the directory containing `sim.json`.

---

## 8. Converting HERMES Configuration to MONICA JSON

The repository and Windows installer include the Python converter:

```
monica-ini-to-json/
├── monica-ini-to-json.py
├── conversion-template-sim.json
├── conversion-template-site.json
└── conversion-template-crop.json
```

The converter reads a HERMES-style `monica.ini` file and generates:

- `sim-from-monica-ini.json`
- `site-from-monica-ini.json`
- `crop-from-monica-ini.json`
- `climate-from-monica-ini.csv`

The output files are written to the directory containing `monica.ini`.

---

### 8.1 Requirements

The converter requires Python 3.

Check the installed Python version with:

```
python --version
```

On systems where Python 3 is invoked as `python3`:

```
python3 --version
```

---

### 8.2 Running the Converter

From the converter directory:

=== "Windows CMD"

    ```cmd
    python monica-ini-to-json.py monica.ini=C:\path\to\monica.ini
    ```
=== "Linux"

    ```bash
    python3 monica-ini-to-json.py monica.ini=/path/to/monica.ini
    ```

The default template is:

```
./conversion-template-sim.json
```

Therefore, if the script is run from another directory, provide the full path to the template:

```
python3 monica-ini-to-json.py monica.ini=/path/to/monica.ini sim.json=/path/to/conversion-template-sim.json
```

Arguments use the following format:

```
name=value
```

Do not add spaces around the `=` character.

---

### 8.3 Displaying Converter Options

Use `-h`:

```
python3 monica-ini-to-json.py -h
```

The available arguments are:

```
monica.ini=path-to-monica.ini
sim.json=path-to-template-sim.json
out-suffix=suffix-to-append-to-output-files
```

Defaults:

```
monica.ini=./monica.ini
sim.json=./conversion-template-sim.json
out-suffix=-from-monica-ini
```

---

### 8.4 Changing the Output Suffix

To use a custom suffix:

```
python3 monica-ini-to-json.py monica.ini=/path/to/monica.ini out-suffix=-converted
```

This produces files such as:

```
sim-converted.json
site-converted.json
crop-converted.json
climate-converted.csv
```

To disable the suffix:

```
python3 monica-ini-to-json.py monica.ini=/path/to/monica.ini out-suffix=
```

Review the output carefully before overwriting existing configuration files.

---

### 8.5 Climate Date Handling

The converted combines the yearly HERMES weather files into one climate CSV file.

The simulation start and end years are read from the `[simulation_time]` section of `monica.ini`. The converter:

1. Reads the climate file for each year in that range.
2. Combines the records into one climate file.
3. Sets MONICA's `start-date` and `end-date` options to January 1 and December 31 of the selected years.

The converter does not generally remove individual rows outside arbitrary start and end dates.

Radiation handling is as follows:

- If any positive global-radiation value is found, the output keeps global radiation and removes sunhours.

- If no positive global-radiation value is found but sunhours are present, the output keeps sunhours and removes global radiation.

- If neither value is present, both columns may remain.

---

### 8.6 Two-Digit Year Interpretation

The converter uses the following rules:

```python
YEAR_BORDERS = [[0, 30, 2000], [31, 99, 1900]]
YEAR_SUFFIX_DIGITS = 3
```

This means:

- Two-digit years from `00` through `30` are interpreted as 2000-2030.
- Two-digit years from `31` through `99` are interpreted as 1931-1999.
- Climate filenames use the last three digits of the year.

For example:

- `020506` → `2006-05-02`
- `020598` → `1998-05-02`

---

### 8.7 Converter Limitations

The generated files must be reviewed manually.

Important considerations include:

- `incorporate = 1` creates an `OrganicFertilization` workstep.
- `incorporate = 0` creates a `MineralFertilization` workstep.
- Crop references use the form:
```json
["ref", "crops", "crop-name"]
```

- Fertilizer references use the form:
```json
["ref", "fert-params", "fertilizer-name"]
```

The referenced crop and fertilizer names must exist in the configured parameter files or templates.

The converter currently processes the first crop rotation found in the HERMES crop-rotation file. Verify the resulting crop rotation, fertilization, irrigation, tillage, climate, and date settings before using the generated configuration for production simulations.
