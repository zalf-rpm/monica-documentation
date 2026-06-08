# Organic Fertiliser Parameters

This section provides an overview of the key-value pairs in the organic fertiliser JSON files used by MONICA. Each file represents one organic material type and defines its dry matter content, nitrogen fractions, and decomposition characteristics. These files are located in the `organic-fertilisers/` folder of the `monica-parameters` repository.

---

## List of organic fertiliser parameters

| Parameter Name              | Type   | Unit            | Description                                                                                                         | Example                                       |
|-----------------------------|--------|-----------------|---------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| `id`                        | String |                 | Short identifier code for the organic material, used to reference it in management configurations                   | `"id": "CADLM"`                               |
| `name`                      | String |                 | Full descriptive name of the organic material                                                                       | `"name": "cattle deep-litter manure"`         |
| `AOM_DryMatterContent`      | Float  | kg DM kg FM-1   | Dry matter content of the organic material as a fraction of fresh mass                                              | `"AOM_DryMatterContent": [0.289, "kg DM kg FM-1", "..."]` |
| `AOM_NH4Content`            | Float  | kg N kg DM-1    | Ammonium nitrogen content per unit of dry matter                                                                    | `"AOM_NH4Content": [0.007, "kg N kg DM-1", "..."]` |
| `AOM_NO3Content`            | Float  | kg N kg DM-1    | Nitrate nitrogen content per unit of dry matter                                                                     | `"AOM_NO3Content": [0, "kg N kg DM-1", "..."]` |
| `NConcentration`            | Float  | kg N kg DM-1    | Total nitrogen concentration of the organic material. If set to 0, MONICA calculates it from the pool parameters   | `"NConcentration": 0`                         |
| `AOM_FastDecCoeffStandard`  | Float  | d-1             | Decomposition rate coefficient of the fast AOM pool under standard conditions                                       | `"AOM_FastDecCoeffStandard": [0.002, "d-1", "..."]` |
| `AOM_SlowDecCoeffStandard`  | Float  | d-1             | Decomposition rate coefficient of the slow AOM pool under standard conditions                                       | `"AOM_SlowDecCoeffStandard": [0.0002, "d-1", "..."]` |
| `PartAOM_to_AOM_Fast`       | Float  | kg kg-1         | Fraction of the added organic matter assigned to the rapidly decomposing pool                                       | `"PartAOM_to_AOM_Fast": [0.18, "kg kg-1", "..."]` |
| `PartAOM_to_AOM_Slow`       | Float  | kg kg-1         | Fraction of the added organic matter assigned to the slowly decomposing pool                                        | `"PartAOM_to_AOM_Slow": [0.72, "kg kg-1", "..."]` |
| `CN_Ratio_AOM_Fast`         | Float  |                 | Carbon to nitrogen ratio of the rapidly decomposing AOM pool                                                        | `"CN_Ratio_AOM_Fast": [7.3, "", "..."]`       |
| `CN_Ratio_AOM_Slow`         | Float  |                 | Carbon to nitrogen ratio of the slowly decomposing AOM pool                                                         | `"CN_Ratio_AOM_Slow": [100, "", "..."]`       |
| `PartAOM_Slow_to_SMB_Fast`  | Float  | kg kg-1         | Fraction of slow AOM consumed by the fast soil microbial biomass pool                                               | `"PartAOM_Slow_to_SMB_Fast": [1, "kg kg-1", "..."]` |
| `PartAOM_Slow_to_SMB_Slow`  | Float  | kg kg-1         | Fraction of slow AOM consumed by the slow soil microbial biomass pool                                               | `"PartAOM_Slow_to_SMB_Slow": [0, "kg kg-1", "..."]` |
| `type`                      | String |                 | Declares that this JSON defines organic fertiliser parameters                                                       | `"type": "OrganicFertiliserParameters"`       |

!!! note
    Parameters are encoded as arrays containing a value, a unit string, and a description string. The unit and description strings are for documentation purposes only and are not interpreted by the model. `PartAOM_to_AOM_Fast` and `PartAOM_to_AOM_Slow` do not need to sum to 1.0 — the remainder represents the fraction lost during application (e.g. as gases).

---

## Fertiliser abbreviations

The following table lists all available organic fertilisers in MONICA with their IDs, full names, and key decomposition properties.

| ID     | Full Name                                              | Dry Matter (kg DM kg FM-1) | NH4 Content (kg N kg DM-1) | Fast Dec. Coeff. (d-1) | Fast Pool Fraction |
|--------|--------------------------------------------------------|----------------------------|----------------------------|------------------------|--------------------|
| ASH    | Wood ashes                                             | 1.000                      | 0.000                      | 0.00200                | 0.10               |
| CADLM  | Cattle deep-litter manure                              | 0.289                      | 0.007                      | 0.00200                | 0.18               |
| CAM    | Cattle manure                                          | 0.196                      | 0.007                      | 0.00200                | 0.18               |
| CAS    | Cattle slurry                                          | 0.103                      | 0.032                      | 0.00200                | 0.18               |
| CAU    | Cattle urine                                           | 0.033                      | 0.146                      | 0.00200                | 0.18               |
| DGDLM  | Duck or goose deep-litter manure                       | 0.350                      | 0.024                      | 0.00200                | 0.18               |
| GWC    | Green-waste compost                                    | 0.500                      | 0.002                      | 0.00200                | 0.18               |
| HODLM  | Horse deep-litter manure                               | 0.260                      | 0.008                      | 0.00200                | 0.18               |
| MC     | Mushroom compost                                       | 0.390                      | 0.000                      | 0.00200                | 0.18               |
| MS     | Maize straw                                            | 0.850                      | 0.000                      | 0.00200                | 0.18               |
| OIC    | Oilseed-rape cake fertiliser (5-1-10)                  | 0.900                      | 0.000                      | 0.05000                | 0.62               |
| PIDLM  | Pig deep-litter manure                                 | 0.330                      | 0.009                      | 0.00200                | 0.18               |
| PIM    | Pig manure                                             | 0.039                      | 0.014                      | 0.00200                | 0.18               |
| PIS    | Pig slurry                                             | 0.054                      | 0.068                      | 0.00200                | 0.18               |
| PIU    | Pig urine                                              | 0.020                      | 0.162                      | 0.00200                | 0.18               |
| PIUDK  | Pig slurry-DK                                          | 0.050                      | 0.080                      | 0.00200                | 0.18               |
| PLW    | Potato liquid waste                                    | 0.020                      | 0.028                      | 0.00200                | 0.18               |
| POM    | Poultry manure                                         | 0.400                      | 0.019                      | 0.00200                | 0.18               |
| SOY    | Soybean straw                                          | 0.850                      | 0.000                      | 0.00200                | 0.18               |
| SS     | Sewage sludge                                          | 0.141                      | 0.089                      | 0.00200                | 0.18               |
| TUDLM  | Turkey deep-litter manure                              | 0.480                      | 0.038                      | 0.00200                | 0.18               |
| WEEDS  | Weeds                                                  | 0.850                      | 0.000                      | 0.00200                | 0.18               |
| WS     | Wheat straw                                            | 0.850                      | 0.000                      | 0.00200                | 0.18               |

---

## Example: `CADLM.json`

```json
{
    "AOM_DryMatterContent": [0.289, "kg DM kg FM-1", "Dry matter content of added organic matter"],
    "AOM_FastDecCoeffStandard": [0.002, "d-1", "Decomposition rate coefficient of fast AOM at standard conditions"],
    "AOM_NH4Content": [0.007, "kg N kg DM-1", "Ammonium content in added organic matter"],
    "AOM_NO3Content": [0, "kg N kg DM-1", "Nitrate content in added organic matter"],
    "AOM_SlowDecCoeffStandard": [0.0002, "d-1", "Decomposition rate coefficient of slow AOM at standard conditions"],
    "CN_Ratio_AOM_Fast": [7.3, "", "C to N ratio of the rapidly decomposing AOM pool"],
    "CN_Ratio_AOM_Slow": [100, "", "C to N ratio of the slowly decomposing AOM pool"],
    "NConcentration": 0,
    "PartAOM_Slow_to_SMB_Fast": [1, "kg kg-1", "Part of AOM slow consumed by fast soil microbial biomass"],
    "PartAOM_Slow_to_SMB_Slow": [0, "kg kg-1", "Part of AOM slow consumed by slow soil microbial biomass"],
    "PartAOM_to_AOM_Fast": [0.18, "kg kg-1", "Part of AOM that is assigned to the rapidly decomposing pool"],
    "PartAOM_to_AOM_Slow": [0.72, "kg kg-1", "Part of AOM that is assigned to the slowly decomposing pool"],
    "id": "CADLM",
    "name": "cattle deep-litter manure",
    "type": "OrganicFertiliserParameters"
}
```