# Crop Parameters Overview

MONICA organizes crop-specific parameters into two main groups:

- **Species-level parameters** describe properties shared by a crop species, such as wheat. 
- **Cultivar-level parameters** describe properties of a particular cultivar or crop type, such as winter wheat.

A complete crop definition also contains a separate set fo **residue parameters**, which MONICA uses when processing crop residues.

These parameter sets are linked to crop definitions in `crop.json`.

---

## Example: `crop.json`

The `crops` object defines crops that can be reference from the crop rotation. Each key is a user-defined crop code.

```json
{
  "crops": {
    "WW": {
      "is-winter-crop": true,
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/wheat.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/wheat/winter-wheat.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/wheat.json"]
    }
  }
}
```

In this example:

- `WW` is a user-defined crop representing winter wheat.
- `is-winter-crop` identifies the crop as a winter crop.
- `species` references the species-level parameter file.
- `cultivar` references the cultivar-level parameter file.
- `residueParams` references the crop-residue parameter file.

> Crop codes such as WW are defined by the configuration. They are conventions used by a particular setup, not fixed identifiers built into MONICA.

---

## Referencing a crop in the rotation

A sowing workstep can reference a crop defined in the `crops` object:

```json
{ "date": "1991-09-22", "type": "Sowing", "crop": ["ref", "crops", "WW"] }
```

The reference consists of:

1. `ref`, which identifies the value as a reference
2. `crops`, which identifies the object containing the crop definitions
3. `WW`, which identifies the requested crop definition

This allows the same crop definition to be reused in multiple crop rotations or sowing worksteps.

## Parameter loading

When MONICA resolves the `WW` crop reference, it:

1. Loads the species-level parameters from `crops/wheat.json`.
2. Loads the cultivar-level parameters from `crops/wheat/winter-wheat.json`.
3. Merges both objects into the complete crop-parameter set.
4. Loads the residue parameters from `crop-residues/wheat.json` as a separate parameter object.
5. Uses the resulting crop definition when the sowing workstep is executed.

The species and cultivar parameters must be compatible. For example, arrays describing developmental stages or crop organs must have matching dimensions where required.

---

## Residue parameters

Residue parameters are not merged into the species and cultivar crop parameters. They remain a separate part of the crop definition.

MONICA uses them when organic matter from the crop is returned to the soil, for example during:

- harvest
- cutting
- incorporation of crop biomass
- other management operations that leave plant material in or on the soil

The amount of residue returned to the soil can also depend on the management workstep and its export settings.

---

## Resolving parameter-file paths:

Paths used by `include-from-file` may be absolute or relative. Relative paths are resolved using the include-file base path configured for the simulation.

For example, a configuration may define:

```json
{"_include-file-base-path": "../monica-parameters"}
```

The crop definition can then use shorter paths:

```json
{
  "cropParams": {
    "species": ["include-from-file", "crops/wheat.json"],
    "cultivar": ["include-from-file", "crops/wheat/winter-wheat.json"]
  },
  "residueParams": ["include-from-file", "crop-residues/wheat.json"]
}
```

The exact base-path configuration can vary between MONICA integrations and simulation setups.

---

## Available crop parameter files

The available parameter files are maintained in the [MONICA parameters repository](https://github.com/zalf-rpm/monica-parameters/tree/master/crops).

- **Species-level files** are the JSON files directly inside the `crops` directory (e.g., `crops/wheat.json`)
- **Cultivar-level files** are located inside subfolders named after the crop species (e.g., `crops/wheat/winter-wheat.json`)
- **Residue parameter files** are located in the `crop-residues` directory (e.g., `crop-residues/wheat.json`)