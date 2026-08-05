# Crop Residue Parameters

This section describes the JSON properties used to configure crop residue parameters in MONICA. 

Crop residue parameters determine how plant material is divided between added organic matter (AOM) pools, how those pools decompose, and how residue carbon and nitrogen enter the soil organic matter (SOM) model.

The parameters are used for material returned after harvest, cutting, root turnover, or incorporation of an entire crop.

---

## JSON conventions

- `Number` means a JSON number and may contain an integer or decimal value.
- Fractions are expressed on a scale from `0` to `1`.
- Numeric parameters may be written directly:

    ```json
    "AOM_DryMatterContent": 1
    ```

- They may also include unit and description metadata:

    ```json
    "AOM_DryMatterContent": [1.0, "kg DM kg FM-1", "Dry matter content of added organic matter"]
    ```

- MONICA reads the first numeric element. Unit and description strings are not validated, intepreted, or converted. Values must therefore already use the units documented below.
- Missing numeric parameters generally retain their default value of zero. A zero value is not necessarily meaningful or safe for every parameter.

---

## General parameters

| Parameter Name | Type    | Unit | Description                                                                            | Example                           |
|----------------|---------|------|----------------------------------------------------------------------------------------|-----------------------------------|
| `type`         | String  |      | Serialization metadata identifying the object as crop residue parameters               | `"type": "CropResidueParameters"` |
| `species`      | String  |      | Crop species associated with the residue parameter set                                 | `"species": "wheat"`              |
| `residueType`  | String  |      | Optional identifier for the residue type or parameter variant, such as straw or roots. | `"residueType": ""`               |

---

## Residue composition

| Parameter Name         | Type   | Unit          | Description                                                                                                                                                                                                                         | Example                     |
|------------------------|--------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------|
| `AOM_DryMatterContent` | Number | kg DM kg-1 FM | Residue dry-matter fraction relative to fresh mass. A value of `1` represents completely dry material.                                                                                                                              | `"AOM_DryMatterContent": 1` |
| `CorgContent`          | Number | kg C kg-1 DM  | Organic carbon content per unit residue dry matter. If the value is missing, zero, or negative, MONICA uses `0.45 kg C kg-1 DM`.                                                                                                    | `"CorgContent": 0.46`       |
| `NConcentration`       | Number | kg N kg-1 DM  | Nitrogen concentration used to derive the organic-pool C:N balance. When residues originate from a simulated crop, MONICA normally supplies the crop's dynamically calculated residue N concentration instead of this static value. | `"NConcentration": 0.005`   |
| `AOM_NH4Content`       | Number | kg N kg-1 DM  | Ammonium nitrogen content per unit residue dry matter. This nitrogen is added directly to the soil ammonium pool.                                                                                                                   | `"AOM_NH4Content": 0.0002`  |
| `AOM_NO3Content`       | Number | kg N kg-1 DM  | Nitrate nitrogen content per unit residue dry matter. This nitrogen is added directly to the soil nitrate pool.                                                                                                                     | `"AOM_NO3Content": 0.0001`  |
| `AOM_CarbamidContent`  | Number | kg N kg-1 DM  | Carbamide/urea nitrogen content per unit residue dry matter. This is normally zero for crop residues.                                                                                                                               | `"AOM_CarbamidContent": 0`  |

---

## AOM pool allocation

Residue organic carbon is divided between a rapidly decomposing AOM pool and a slowly decomposing AOM pool.

| Parameter Name        | Type   | Unit    | Description                                                                         | Example                       |
|-----------------------|--------|---------|-------------------------------------------------------------------------------------|-------------------------------|
| `PartAOM_to_AOM_Fast` | Number | kg kg-1 | Fraction of the residue organic matter assigned to the rapidly decomposing AOM pool | `"PartAOM_to_AOM_Fast": 0.12` |
| `PartAOM_to_AOM_Slow` | Number | kg kg-1 | Fraction of the residue organic matter assigned to the slowly decomposing AOM pool  | `"PartAOM_to_AOM_Slow": 0.88` |

These two fractions should normally sum to `1`:

```
PartAOM_to_AOM_Fast + PartAOM_to_AOM_Slow = 1
```

---

## Decomposition coefficients

| Parameter Name             | Type   | Unit | Description                                                                                  | Example                              |
|----------------------------|--------|------|----------------------------------------------------------------------------------------------|--------------------------------------|
| `AOM_FastDecCoeffStandard` | Number | d-1  | Decomposition rate coefficient of the rapidly decomposing AOM pool under standard conditions | `"AOM_FastDecCoeffStandard": 0.012`  |
| `AOM_SlowDecCoeffStandard` | Number | d-1  | Decomposition rate coefficient of the slowly decomposing AOM pool under standard conditions  | `"AOM_SlowDecCoeffStandard": 0.0012` |

Actual decomposition is additionally affected by environmental conditions and soil organic matter model parameters.

---

## AOM carbon-to-nitrogen ratios

| Parameter Name      | Type   | Unit | Description                                                                                                                                                                                                                       | Example                   |
|---------------------|--------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| `CN_Ratio_AOM_Fast` | Number |      | Carbon to nitrogen ratio of the rapidly decomposing AOM pool. For crop residues, set this to `0`. MONICA then calculates the fast-pool C:N ratio dynamically from the residue carbon and supplied residue nitrogen concentration. | `"CN_Ratio_AOM_Fast": 0`  |
| `CN_Ratio_AOM_Slow` | Number |      | Carbon to nitrogen ratio of the slowly decomposing AOM pool. This value must be greater than zero when `PartAOM_to_AOM_Slow` is greater than zero.                                                                                | `"CN_Ratio_AOM_Slow": 80` |

### `CN_Ratio_AOM_Fast = 0`

MONICA uses a fast-pool C:N ratio of zero as a marker that the material is a crop residue.

For crop residues, MONICA calculates the fast-pool C:N ratio dynamically from:

- residue organic-carbon content
- residue dry-matter content
- the supplied residue nitrogen concentration
- the fast and slow AOM allocation fractions
- `CN_Ratio_AOM_Slow`
- the configured maximum C:N ratio of the fast AOM pool

A nonzero `CN_Ratio_AOM_Fast` causes the material to follow the fixed C:N organic fertilizer pathway instead. Therefore, crop residue files should normally use:

```json
"CN_Ratio_AOM_Fast": [0, "", "Calculated dynamically for crop residues"]
```

---

## Transfer to soil microbial biomass

| Parameter Name             | Type   | Unit    | Description                                                                         | Example                           |
|----------------------------|--------|---------|-------------------------------------------------------------------------------------|-----------------------------------|
| `PartAOM_Slow_to_SMB_Fast` | Number | kg kg-1 | Fraction of decomposed slow-ppol AOM routed to the fast soil microbial biomass pool | `"PartAOM_Slow_to_SMB_Fast": 0.5` |
| `PartAOM_Slow_to_SMB_Slow` | Number | kg kg-1 | Fraction of decomposed slow-pool AOM routed to the slow soil microbial biomass pool | `"PartAOM_Slow_to_SMB_Slow": 0.5` |

These fractions should normally sum to `1`:

```
PartAOM_Slow_to_SMB_Fast + PartAOM_Slow_to_SMB_Slow = 1
```

---

## Dynamically calculated residue nitrogen

When MONICA returns material from a simulated crop to the soil, the residue nitrogen concentration is normally calculated from the crop's current biomass and nitrogen state.

This applies to operations such as:

- harvesting and returning residues
- cutting and leaving cut material on the field
- incorporating the current crop
- transferring dead root biomass to soil organic matter

In these cases, the calculated crop-residue N concentration is passed to the soil organic matter model, and the static `NConcentration` property in the residue file does not determine the residue N concentration.

`NConcentration` can still be relevant when the parameter object is applied through a generic organic matter or organic fertilizer workflow.

---

## Default organic carbon content

If `CorgContent` is missing or not greater than zero, MONICA uses:

```
CorgContent = 0.45 kg C kg⁻¹ DM
```

A positive value explicitly supplied in the JSON overrides this fallback.

---

## Available crop residue files

The [`crop-residues` directory in the `monica-parameters` repository](https://github.com/zalf-rpm/monica-parameters/tree/master/crop-residues) contains residue parameter sets for wheat, maize, rye, potato, clover-grass ley, and many other crops.

---

## Example: wheat residue parameters

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

---

## Consistency requirements

Before using a crop residue parameter object, verify that:

- `AOM_DryMatterContent` lies between `0` and `1`.
- `CorgContent` lies between `0` and `1`, or is omitted to use the `0.45` fallback.
- `NConcentration`, `AOM_NH4Content`, `AOM_NO3Content`, and `AOM_CarbamidContent` are non-negative.
- `PartAOM_to_AOM_Fast` and `PartAOM_to_AOM_Slow` lie between `0` and `1`.
- `PartAOM_to_AOM_Fast` + `PartAOM_to_AOM_Slow` normally equals `1`.
- `AOM_FastDecCoeffStandard` and `AOM_SlowDecCoeffStandard` are non-negative.
- `CN_Ratio_AOM_Fast` is `0` for crop residues.
- `CN_Ratio_AOM_Slow` is greater than zero when the slow-pool fraction is greater than zero.
- `PartAOM_Slow_to_SMB_Fast` and `PartAOM_Slow_to_SMB_Slow` lie between `0` and `1`.
- `PartAOM_Slow_to_SMB_Fast` + `PartAOM_Slow_to_SMB_Slow` normally equals `1`.
- All values already use the documented units because unit strings are not interpreted or converted.

---

## Current implementation limitations

- `CorgContent` is read from JSON and used during normal execution, but it is not currently included in the Cap'n Proto organic matter parameter schema. Reconstructing residue parameters through that schema can therefore lose the configured value and cause MONICA to use the `0.45` fallback.
- `AOM_CarbamidContent` is accepted as JSON input and used by the soil organic matter model. However, the current JSON serializer mistakenly emits a duplicate `AOM_NO3Content` property instead of emitting `AOM_CarbamidContent`.
- For simulated crop residues, the crop's calculated residue N concentration normally overrides the static `NConcentration` value.