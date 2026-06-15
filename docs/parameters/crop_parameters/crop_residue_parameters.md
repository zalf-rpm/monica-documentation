# Crop Residue Parameters

This section provides an overview of the key-value pairs in the crop residue JSON files used by MONICA. These parameters define how crop residues left on the field after harvest decompose and contribute to soil organic matter. Each crop has its own residue parameter file located in the `crop-residues/` folder of the `monica-parameters` repository.

---

## List of crop residue parameters

| Parameter Name              | Type    | Unit          | Description                                                                                                                    | Example                                            |
|-----------------------------|---------|---------------|--------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------|
| `species`                   | String  |               | The crop species this residue file belongs to                                                                                  | `"species": "wheat"`                               |
| `residueType`               | String  |               | Type of residue (e.g. straw, roots). Left empty if not specified                                                               | `"residueType": ""`                                |
| `AOM_DryMatterContent`      | Float   | kg DM kg FM-1 | Dry matter content of the crop residue as a fraction of fresh mass                                                             | `"AOM_DryMatterContent": [1, "kg DM kg FM-1", "..."]` |
| `NConcentration`            | Float   | kg N kg DM-1  | Total nitrogen concentration of the residue per unit dry matter                                                                | `"NConcentration": [0.005, "kg N kg DM-1", "..."]` |
| `CorgContent`               | Float   | kg C kg DM-1  | Organic carbon content of the residue per unit dry matter                                                                      | `"CorgContent": [0.46, "kg C kg DM-1", "..."]`    |
| `AOM_NH4Content`            | Float   | kg N kg DM-1  | Ammonium nitrogen content per unit dry matter                                                                                  | `"AOM_NH4Content": [0.0002, "kg N kg DM-1", "..."]` |
| `AOM_NO3Content`            | Float   | kg N kg DM-1  | Nitrate nitrogen content per unit dry matter                                                                                   | `"AOM_NO3Content": [0.0001, "kg N kg DM-1", "..."]` |
| `AOM_FastDecCoeffStandard`  | Float   | d-1           | Decomposition rate coefficient of the fast AOM pool under standard conditions                                                  | `"AOM_FastDecCoeffStandard": [0.012, "d-1", "..."]` |
| `AOM_SlowDecCoeffStandard`  | Float   | d-1           | Decomposition rate coefficient of the slow AOM pool under standard conditions                                                  | `"AOM_SlowDecCoeffStandard": [0.0012, "d-1", "..."]` |
| `CN_Ratio_AOM_Fast`         | Float   |               | Carbon to nitrogen ratio of the rapidly decomposing AOM pool                                                                   | `"CN_Ratio_AOM_Fast": [0, "", "..."]`              |
| `CN_Ratio_AOM_Slow`         | Float   |               | Carbon to nitrogen ratio of the slowly decomposing AOM pool                                                                    | `"CN_Ratio_AOM_Slow": [80, "", "..."]`             |
| `PartAOM_to_AOM_Fast`       | Float   | kg kg-1       | Fraction of the residue assigned to the rapidly decomposing AOM pool                                                           | `"PartAOM_to_AOM_Fast": [0.12, "kg kg-1", "..."]` |
| `PartAOM_to_AOM_Slow`       | Float   | kg kg-1       | Fraction of the residue assigned to the slowly decomposing AOM pool                                                            | `"PartAOM_to_AOM_Slow": [0.88, "kg kg-1", "..."]` |
| `PartAOM_Slow_to_SMB_Fast`  | Float   | kg kg-1       | Fraction of slow AOM consumed by the fast soil microbial biomass pool                                                          | `"PartAOM_Slow_to_SMB_Fast": [0.5, "kg kg-1", "..."]` |
| `PartAOM_Slow_to_SMB_Slow`  | Float   | kg kg-1       | Fraction of slow AOM consumed by the slow soil microbial biomass pool                                                          | `"PartAOM_Slow_to_SMB_Slow": [0.5, "kg kg-1", "..."]` |
| `type`                      | String  |               | Declares that this JSON defines crop residue parameters                                                                        | `"type": "CropResidueParameters"`                  |

!!! note
    Parameters are encoded as arrays containing a value, a unit string, and a description string. The unit and description strings are for documentation purposes only and are not interpreted by the model. `CorgContent` is an important parameter, if it is missing from a residue file, MONICA defaults to a fixed value of 0.45 kg C kg DM-1.

---

## Available crop residue files

MONICA provides residue parameter files for a range of crops including wheat, maize, rye, potato, clover-grass ley and more. Since values differ between crops, each crop has its own dedicated file.

The full list of available crop residue files can be found in the `monica-parameters` repository:

👉 [monica-parameters/crop-residues on GitHub](https://github.com/zalf-rpm/monica-parameters/tree/master/crop-residues)

---

## Example: `wheat.json`

```json
{
    "AOM_DryMatterContent": [1, "kg DM kg FM-1", "Dry matter content of added organic matter"],
    "AOM_FastDecCoeffStandard": [0.012, "d-1", "Decomposition rate coefficient of fast AOM at standard conditions"],
    "AOM_NH4Content": [0.0002, "kg N kg DM-1", "Ammonium content in added organic matter"],
    "AOM_NO3Content": [0.0001, "kg N kg DM-1", "Nitrate content in added organic matter"],
    "AOM_SlowDecCoeffStandard": [0.0012, "d-1", "Decomposition rate coefficient of slow AOM at standard conditions"],
    "CN_Ratio_AOM_Fast": [0, "", "C to N ratio of the rapidly decomposing AOM pool"],
    "CN_Ratio_AOM_Slow": [80, "", "C to N ratio of the slowly decomposing AOM pool"],
    "NConcentration": [0.005, "kg N kg DM-1", "Nitrogen content in added organic matter"],
    "CorgContent": [0.46, "kg C kg DM-1", "Carbon content in added organic matter"],
    "PartAOM_Slow_to_SMB_Fast": [0.5, "kg kg-1", "Part of AOM slow consumed by fast soil microbial biomass"],
    "PartAOM_Slow_to_SMB_Slow": [0.5, "kg kg-1", "Part of AOM slow consumed by slow soil microbial biomass"],
    "PartAOM_to_AOM_Fast": [0.12, "kg kg-1", "Part of AOM that is assigned to the rapidly decomposing pool"],
    "PartAOM_to_AOM_Slow": [0.88, "kg kg-1", "Part of AOM that is assigned to the slowly decomposing pool"],
    "residueType": "",
    "species": "wheat",
    "type": "CropResidueParameters"
}
```