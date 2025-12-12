# Crop Parameters Overview

MONICA distinguishes between **species-level** and **cultivar-level** crop parameters. These parameters are linked in the simulation setup through the `crop.json` file.

---

## Example: `crop.json`

The `crop.json` file specifies which parameter files are used for each crop in a simulation.

```json
"WW": {
  "cropParams": {
    "species": ["include-from-file", "../monica-parameters/crops/wheat.json"],
    "cultivar": ["include-from-file", "../monica-parameters/crops/wheat/winter-wheat.json"]
  },
  "residueParams": ["include-from-file", "../monica-parameters/crop-residues/wheat.json"]
}
```

In this example:

- `"WW"` is the crop code for **winter wheat**.
- `"species` points to the species-level parameter file.
- `"cultivar` points to the cultivar-level parameter file.
- `"residueParams` points to the crop residue parameter file.

During simulation, MONICA merges the parameters from the **species** and **cultivar** JSON files to form the complete crop definition.

---

## How MONICA uses these parameters

When a workstep in the crop rotation references a crop, for example:

```json
{ "date": "1991-09-22", "type": "Sowing", "crop": ["ref", "crops", "WW"] }
```

MONICA performs the following steps:

1. Loads the **species-level parameters** from `wheat.json`
2. Loads the **cultivar-level parameters** from `winter-wheat.json`
3. Merges them to form the complete winter wheat parameter set
4. Applies **residue parameters** after harvest as defined in `crop-residues/wheat.json`

---

## Available crop parameter files

The current list of available species and cultivar parameter files can be found in the [MONICA parameters](https://github.com/zalf-rpm/monica-parameters/tree/master/crops) repository.

The crop parameters are organized in two levels within the `crops` directory.

- **Species-level files** are the JSON files directly inside the `crops` directory (e.g., `crops/wheat.json`)
- **Cultivar-level files** are located inside subfolders named after the crop species (e.g., `crops/wheat/winter-wheat.json`)