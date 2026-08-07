# Organic Fertiliser Parameters

This section describes the organic fertiliser parameter files used by MONICA. Each JSON file represents one organic material and defines properties such as dry matter content, mineral nitrogen content, organic matter pool allocation, C:N ratios, and decomposition rates.

The files are located in the `organic-fertilisers/` directory of the [`monica-parameters`](https://github.com/zalf-rpm/monica-parameters) repository.

---

## List of organic fertiliser parameters

| Parameter Name              | Type   | Unit          | Description                                                                                                                                                                                                        | Example                                                   |
|-----------------------------|--------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| `id`                        | String |               | Short identifier used to reference the organic fertiliser in management configurations                                                                                                                             | `"id": "CADLM"`                                           |
| `name`                      | String |               | Full descriptive name of the organic material                                                                                                                                                                      | `"name": "cattle deep-litter manure"`                     |
| `AOM_DryMatterContent`      | Number | kg DM kg FM-1 | Dry matter content of the organic material as a fraction of fresh mass                                                                                                                                             | `"AOM_DryMatterContent": [0.289, "kg DM kg FM-1", "..."]` |
| `AOM_NH4Content`            | Number | kg N kg DM-1  | Ammonium nitrogen content per unit of dry matter                                                                                                                                                                   | `"AOM_NH4Content": [0.007, "kg N kg DM-1", "..."]`        |
| `AOM_NO3Content`            | Number | kg N kg DM-1  | Nitrate nitrogen content per unit of dry matter                                                                                                                                                                    | `"AOM_NO3Content": [0, "kg N kg DM-1", "..."]`            |
| `NConcentration`            | Number | kg N kg DM-1  | Nitrogen concentration supplied with the added organic matter. It is currently `0` in the organic fertiliser parameter files. Organic nitrogen is represented through the AOM pool fractions and their C:N ratios. | `"NConcentration": 0`                                     |
| `AOM_FastDecCoeffStandard`  | Number | d-1           | Decomposition rate coefficient of the fast AOM pool under standard conditions                                                                                                                                      | `"AOM_FastDecCoeffStandard": [0.002, "d-1", "..."]`       |
| `AOM_SlowDecCoeffStandard`  | Number | d-1           | Decomposition rate coefficient of the slow AOM pool under standard conditions                                                                                                                                      | `"AOM_SlowDecCoeffStandard": [0.0002, "d-1", "..."]`      |
| `PartAOM_to_AOM_Fast`       | Number | kg kg-1       | Fraction of the added organic matter assigned to the rapidly decomposing pool                                                                                                                                      | `"PartAOM_to_AOM_Fast": [0.18, "kg kg-1", "..."]`         |
| `PartAOM_to_AOM_Slow`       | Number | kg kg-1       | Fraction of the added organic matter assigned to the slowly decomposing pool                                                                                                                                       | `"PartAOM_to_AOM_Slow": [0.72, "kg kg-1", "..."]`         |
| `CN_Ratio_AOM_Fast`         | Number |               | Carbon to nitrogen ratio of the rapidly decomposing AOM pool                                                                                                                                                       | `"CN_Ratio_AOM_Fast": [7.3, "", "..."]`                   |
| `CN_Ratio_AOM_Slow`         | Number |               | Carbon to nitrogen ratio of the slowly decomposing AOM pool                                                                                                                                                        | `"CN_Ratio_AOM_Slow": [100, "", "..."]`                   |
| `PartAOM_Slow_to_SMB_Fast`  | Number | kg kg-1       | Fraction of slow AOM consumed by the fast soil microbial biomass pool                                                                                                                                              | `"PartAOM_Slow_to_SMB_Fast": [1, "kg kg-1", "..."]`       |
| `PartAOM_Slow_to_SMB_Slow`  | Number | kg kg-1       | Fraction of slow AOM consumed by the slow soil microbial biomass pool                                                                                                                                              | `"PartAOM_Slow_to_SMB_Slow": [0, "kg kg-1", "..."]`       |
| `type`                      | String |               | Declares that this JSON defines organic fertiliser parameters                                                                                                                                                      | `"type": "OrganicFertiliserParameters"`                   |

!!! note "Allocation of added organic matter"
    `PartAOM_to_AOM_Fast` and `PartAOM_to_AOM_Slow` do not have to sum to 1.0.

    When their sum is less than `1.0`, the positive remainder is assigned directly to MONICA's fast soil organic matter (`SOM_Fast`) pool. It does not represent material lost during application.

    Values whose sum exceeds `1.0` should be avoided. In that case, the direct `SOM_Fast` contribution is clamped to zero, but the fast and slow AOM allocations are not reduced.

!!! note "Nitrogen representation" 
    For the organic fertiliser files, the configured pool fractions and C:N ratios determine the organic nitrogen associated with the AOM pools. `AOM_NH4Content` and `AOM_NO3Content` specify the directly added mineral nitrogen fractions.

    Dynamic calculation of `CN_Ratio_AOM_Fast` is used for crop-residue parameters whose configured fast-pool C:N ratio is zero. It is not the normal behavior for the organic fertilisers listed below.

---

## Available organic fertilisers

The following table lists all available organic fertilisers in MONICA with their IDs, full names, and key decomposition properties.

| ID     | Full Name                                              | Dry matter (kg DM kg FM-1) | NH4 content (kg N kg DM-1) | Fast decomposition coefficient (d-1) | Fast AOM fraction |
|--------|--------------------------------------------------------|----------------------------|----------------------------|--------------------------------------|-------------------|
| ASH    | Wood ashes                                             | 1.000                      | 0.000                      | 0.00200                              | 0.10              |
| CADLM  | Cattle deep-litter manure                              | 0.289                      | 0.007                      | 0.00200                              | 0.18              |
| CAM    | Cattle manure                                          | 0.196                      | 0.007                      | 0.00200                              | 0.18              |
| CAS    | Cattle slurry                                          | 0.103                      | 0.032                      | 0.00200                              | 0.18              |
| CAU    | Cattle urine                                           | 0.033                      | 0.146                      | 0.00200                              | 0.18              |
| DGDLM  | Duck or goose deep-litter manure                       | 0.350                      | 0.024                      | 0.00200                              | 0.18              |
| GWC    | Green-waste compost                                    | 0.500                      | 0.002                      | 0.00200                              | 0.18              |
| HODLM  | Horse deep-litter manure                               | 0.260                      | 0.008                      | 0.00200                              | 0.18              |
| MC     | Mushroom compost                                       | 0.390                      | 0.000                      | 0.00200                              | 0.18              |
| MS     | Maize straw                                            | 0.850                      | 0.000                      | 0.00200                              | 0.18              |
| OIC    | Oilseed-rape cake fertiliser (5-1-10)                  | 0.900                      | 0.000                      | 0.05000                              | 0.62              |
| PIDLM  | Pig deep-litter manure                                 | 0.330                      | 0.009                      | 0.00200                              | 0.18              |
| PIM    | Pig manure                                             | 0.039                      | 0.014                      | 0.00200                              | 0.18              |
| PIS    | Pig slurry                                             | 0.054                      | 0.068                      | 0.00200                              | 0.18              |
| PIU    | Pig urine                                              | 0.020                      | 0.162                      | 0.00200                              | 0.18              |
| PIUDK  | Pig slurry-DK                                          | 0.050                      | 0.080                      | 0.00200                              | 0.18              |
| PLW    | Potato liquid waste                                    | 0.020                      | 0.028                      | 0.00200                              | 0.18              |
| PODLM  | Poultry deep-litter manure                             | 0.633                      | 0.037                      | 0.00200                              | 0.18              |
| POM    | Poultry manure                                         | 0.400                      | 0.019                      | 0.00200                              | 0.18              |
| SOY    | Soybean straw                                          | 0.850                      | 0.000                      | 0.00200                              | 0.18              |
| SS     | Sewage sludge                                          | 0.141                      | 0.089                      | 0.00200                              | 0.18              |
| TUDLM  | Turkey deep-litter manure                              | 0.480                      | 0.038                      | 0.00200                              | 0.18              |
| WEEDS  | Weeds                                                  | 0.850                      | 0.000                      | 0.00200                              | 0.18              |
| WS     | Wheat straw                                            | 0.850                      | 0.000                      | 0.00200                              | 0.18              |

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