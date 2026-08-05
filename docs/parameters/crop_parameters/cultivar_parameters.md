# Cultivar-level crop parameters

This section describes the JSON properties used to configure cultivar-level crop parameters in MONICA. 

Cultivar parameters describe characteristics that may differ between cultivars of the same species. They complement, but do not replace, the corresponding species parameters.

---

## JSON conventions

- `Number` means a JSON number and may contain an integer or decimal value.
- `Integer` means a whole-number JSON value.
- Stage-dependent arrays must contain one entry for each developmental stage defined by the species parameters.
- Organ-dependent matrix rows must contain one entry for every species organ.
- Organ IDs in yield components are one-base:

        1. root
        2. leaf
        3. steam or shoot
        4. storage organ, such as fruit, grain, or ear

- Numeric values may be written directly:

    ```json
    "MaxCropHeight": 0.83
    ```

    They may also contain unit metadata:

    ```json
    "MaxCropHeight": [0.83, "m"]
    ```
  
- Stage-dependent arrays may similarly contain unit metadata:

    ```json
    "StageTemperatureSum": [[148, 284, 380, 180, 420, 25], "°C d"]
    ```
  
MONICA reads the numeric value or array but does not validate the unit string or perform unit conversion. Values must already use the units documented below.

---

## General and crop-cycle parameters

| Parameter Name | Type    | Unit | Description                                                                                                                               | Example                          |
|----------------|---------|------|-------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------|
| `type`         | String  |      | Serialization metadata identifying the object as cultivar parameters                                                                      | `"type": "CultivarParameters"`   |
| `CultivarName` | String  |      | Identifier or name of the cultivar                                                                                                        | `"CultivarName": "winter wheat"` |
| `Description`  | String  |      | Optional textual description of the cultivar                                                                                              | `"Description": ""`              |
| `Perennial`    | Boolean |      | Indicates whether the cultivar is perennial                                                                                               | `"Perennial": false`             |
| `WinterCrop`   | Boolean |      | Indicates whether the cultivar is treated as a winter crop. This flag is used by operations that distinguish winter and non-winter crops. | `"WinterCrop": true`             |

---

## Photosynthesis and crop-size parameters

| Parameter Name                | Type   | Unit                 | Description                                                                                                                | Example                              |
|-------------------------------|--------|----------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| `MaxAssimilationRate`         | Number | kg CO2 ha-1 leaf h-1 | Maximum leaf-area-based CO<sub>2</sub> assimilation rate                                                                   | `"MaxAssimilationRate": 30`          |
| `LightExtinctionCoefficient`  | Number |                      | Beer–Lambert light-extinction coefficient used to calculate the fraction of radiation intercepted by the crop canopy       | `"LightExtinctionCoefficient": 0.8`  |
| `MaxCropHeight`               | Number | m                    | Maximum potential crop height                                                                                              | `"MaxCropHeight": [0.83,"m"]`        |
| `CropHeightP1`                | Number |                      | Slope parameter of the sigmoid crop-height development function                                                            | `"CropHeightP1": 6`                  |
| `CropHeightP2`                | Number |                      | Relative-development position parameter of the sigmoid crop-height function                                                | `"CropHeightP2": 0.5`                |
| `CropSpecificMaxRootingDepth` | Number | m                    | Cultivar-specific maximum potential rooting depth. The effective rooting depth may also be constrained by soil properties. | `"CropSpecificMaxRootingDepth": 1.3` |

---

## Development-stage parameters

The outer index of every stage-dependent array corresponds to a developmental stage.

| Parameter Name             | Type           | Unit                 | Description                                                                                                                                                                                                     | Example                                                                      |
|----------------------------|----------------|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `StageTemperatureSum`      | Number (Array) | °C d                 | Thermal-time requirement thath must be accumulated in each developmental stage before the crop advances to the next stage                                                                                       | `"StageTemperatureSum": [[148,284,380,180,420,25],"°C d"]`                   |
| `OptimumTemperature`       | Number (Array) | °C                   | Stage-specific upper temperature used when calculating effective thermal-time accumulation                                                                                                                      | `"OptimumTemperature": [[30,30,30,30,30,30],"°C"]`                           |
| `BaseDaylength`            | Number (Array) | h                    | Stage-specific base or reference day length used with `DaylengthRequirement` to calculate the photoperiod development factor                                                                                    | `"BaseDaylength": [[0,0,7,7,0,0],"h"]`                                       |
| `DaylengthRequirement`     | Number (Array) | h                    | Stage-specific critical day length. Positive values select the long-day response, negative values select the short-day response, and zero disables photoperiod limitation for that stage.                       | `"DaylengthRequirement": [[0,20,20,20,0,0],"h"]`                             |
| `VernalisationRequirement` | Number (Array) | effective d          | Stage-specific effective vernalization-day requirement. A value of zero disables vernalization limitation for that stage.                                                                                       | `"VernalisationRequirement": [0,50,0,0,0,0]`                                 |
| `DroughtStressThreshold`   | Number (Array) |                      | Stage-specific threshold for the ratio of actual to potential transpiration. Below this threshold this threshold, drought responses can affect development, photosynthesis, fertility, and biomass calculation. | `"DroughtStressThreshold": [1,0.9,1,1,0.9,0.8]`                              |
| `SpecificLeafArea`         | Number (Array) | ha leaf kg-1 leaf DM | Stage-specific leaf area per unit leaf dry-matter biomass                                                                                                                                                       | `"SpecificLeafArea": [[0.002,0.0018,0.0017,0.0016,0.0015,0.0015],"ha kg-1"]` |
| `StageKcFactor`            | Number (Array) |                      | Stage-specific crop coefficient used in crop evapotranspiration and interception calculations                                                                                                                   | `"StageKcFactor": [[0.4,0.7,1.1,1.1,0.8,0.25],"1;0"]`                        |

---

## Biomass allocation and senescence

The outer array index represents the developmental stage. Each inner array uses the species organ order: root, leaf, stem/shoot, and storage organ.

| Parameter Name                | Type                     | Unit     | Description                                                                                                                                              | Example                                                                                                                |
|-------------------------------|--------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| `AssimilatePartitioningCoeff` | Number (Array of Arrays) | fraction | Fraction of available assimilates allocated to each organ during each developmental stage. Fractions should normally sum to `1` in active growth stages. | `"AssimilatePartitioningCoeff": [[0.5,0.5,0,0],[0.2,0.2,0.6,0],[0.13,0.2,0.67,0],[0,0,0.03,0.97],[0,0,0,1],[0,0,0,0]]` |
| `OrganSenescenceRate`         | Number (Array of Arrays) | d-1      | Daily senescence rate of each organ during each developmental stage                                                                                      | `"OrganSenescenceRate": [[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0.05,0,0],[0,0.05,0,0]]`                           |
| `ResidueNRatio`               | Number                   |          | Ratio of residue nitrogen concentration to primary-yield nitrogen concentration                                                                          | `"ResidueNRatio": 0.5`                                                                                                 |

---

## Heat-stress parameters

| Parameter Name                  | Type   | Unit | Description                                                                   | Example                                         |
|---------------------------------|--------|------|-------------------------------------------------------------------------------|-------------------------------------------------|
| `CriticalTemperatureHeatStress` | Number | °C   | Temperature threshold above which reproductive heat damage begins             | `"CriticalTemperatureHeatStress": [31,"°C"]`    |
| `BeginSensitivePhaseHeatStress` | Number | °C d | Cumulative thermal time at which the heat-sensitive reproductive phase begins | `"BeginSensitivePhaseHeatStress": [620,"°C d"]` |
| `EndSensitivePhaseHeatStress`   | Number | °C d | Cumulative thermal time at which the heat-sensitive reproductive phase ends   | `"EndSensitivePhaseHeatStress": [740,"°C d"]`   |

---

## Frost and winter-survival parameters

| Parameter Name           | Type   | Unit     | Description                                                                                                                                                                                               | Example                           |
|--------------------------|--------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| `LT50cultivar`           | Number | °C       | Cultivar-specific temperature at which 50% frost mortality is expected                                                                                                                                    | `"LT50cultivar": -24`             |
| `FrostHardening`         | Number | °C-1 d-1 | Coefficient controlling the daily decrease in LT50 caused by cold acclimation                                                                                                                             | `"FrostHardening": 0.014`         |
| `FrostDehardening`       | Number | °C-1 d-1 | Coefficient controlling the daily increase in LT50 caused by deacclimation                                                                                                                                | `"FrostDehardening": 5.05`        |
| `RespiratoryStress`      | Number | °C-1 d-1 | Coefficient controlling the increase in L50 caused by respiratory stress under snow cover.                                                                                                                | `"RespiratoryStress": 0.54`       |
| `LowTemperatureExposure` | Number | °C-1     | Coefficient intended to control additional LT50 changes caused by extreme low-temperature exposure. The corresponding model calculation is currently disabled, so this parameter currently has no effect. | `"LowTemperatureExposure": 0.654` |

---

## Irrigation parameters

| Parameter Name           | Type   | Unit | Description                                                           | Example                         |
|--------------------------|--------|------|-----------------------------------------------------------------------|---------------------------------|
| `HeatSumIrrigationStart` | Number | °C d | Accumulated crop thermal time at which automatic irrigation may begin | `"HeatSumIrrigationStart": 461` |
| `HeatSumIrrigationEnd`   | Number | °C d | Accumulated crop thermal time at which automatic irrigation ends      | `"HeatSumIrrigationEnd": 1676`  |

---

## Yield and cutting components

`OrganIdsForPrimaryYield`, `OrganIdsForSecondaryYield`, and `OrganIdsForCutting` contain yield-component objects.

| Parameter Name              | Type                   | Unit | Description                                                                                                                                    | Example                                                                                                                                                                                               |
|-----------------------------|------------------------|------|------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `OrganIdsForPrimaryYield`   | YieldComponent (Array) |      | Organ fractions included in the primary harvested product                                                                                      | `"OrganIdsForPrimaryYield": [{"organId":4,"type":"YieldComponent","yieldDryMatter":0.86,"yieldPercentage":0.85}]`                                                                                     |
| `OrganIdsForSecondaryYield` | YieldComponent (Array) |      | Organ fractions included in the secondary harvested product                                                                                    | `"OrganIdsForSecondaryYield": [{"organId":2,"type":"YieldComponent","yieldDryMatter":0.86,"yieldPercentage":0.85},{"organId":3,"type":"YieldComponent","yieldDryMatter":0.86,"yieldPercentage":0.9}]` |
| `OrganIdsForCutting`        | YieldComponent (Array) |      | Organ fractions included in the default cutting or mowing yield. An empty array means that no default cutting yield components are configured. | `"OrganIdsForCutting": []`                                                                                                                                                                            |

Each `YieldComponent` contains:

| Parameter Name    | Type    | Unit          | Description                                                                  | Example                    |
|-------------------|---------|---------------|------------------------------------------------------------------------------|----------------------------|
| `type`            | String  |               | Serialization metadata identifying the object as a yield component           | `"type": "YieldComponent"` |
| `organId`         | Integer |               | Species organ referenced by the component                                    | `"organId": 4`             |
| `yieldPercentage` | Number  | fraction      | Fraction of the organ biomass included in the harvested or cut yield         | `"yieldPercentage": 0.85`  |
| `yieldDryMatter`  | Number  | kg DM kg-1 FM | Dry-matter fraction used to convert dry-matter yield into fresh-matter yield | `"yieldDryMatter": 0.86`   |

`organId` must be between `1` and the number of species organs. `yieldPercentage` and `yieldDryMatter` should normally lie in the internal `[0,1]`.

---

## Optional temperature-response parameters

### Leaf-expansion response

These parameters apply only when temperature-dependent leaf expansion (`__enable_T_response_leaf_expansion__`) is enabled in `sim.json`.

| Parameter Name    | Type   | Unit | Description                                                                   | Example                 |
|-------------------|--------|------|-------------------------------------------------------------------------------|-------------------------|
| `EarlyRefLeafExp` | Number | °C   | Reference temperature for the early-stage leaf-expansion temperature response | `"EarlyRefLeafExp": 12` |
| `RefLeafExp`      | Number | °C   | Reference temperature for the later-stage leaf-expansion temperature response | `"RefLeafExp": 20`      |

The species parameter `TransitionStageLeafExp` determines when MONICA changes from the early-stage response to the later-stage response.

### Wang-Engel phenology response

These parameters apply only when the optional Wang-Engel phenological temperature response (`__enable_Phenology_WangEngelTemperatureResponse__`) is enabled in `sim.json`.

| Parameter Name  | Type   | Unit | Description                                      | Example              |
|-----------------|--------|------|--------------------------------------------------|----------------------|
| `MinTempDev_WE` | Number | °C   | Minimum temperature for phenological development | `"MinTempDev_WE": 0` |
| `OptTempDev_WE` | Number | °C   | Optimum temperature for phenological development | `"OptTempDev_WE": 0` |
| `MaxTempDev_WE` | Number | °C   | Maximum temperature for phenological development | `"MaxTempDev_WE": 0` |

---

## Legacy parameters

| Parameter Name     | Type    | Unit        | Description                                                                                                    | Example                  |
|--------------------|---------|-------------|----------------------------------------------------------------------------------------------------------------|--------------------------|
| `LatestHarvestDoy` | Integer | day of year | Legacy cultivar-level latest-harvest value. It is currently not consumed by the automatic harvest calculation. | `"LatestHarvestDoy": -1` |

---

## Example: `winter-wheat.json`

The following example shows a complete cultivar parameter file for winter wheat.

```json
{
  "CultivarName":"winter wheat",
  "WinterCrop": true,
  "AssimilatePartitioningCoeff":[
    [
      0.5,
      0.5,
      0,
      0
    ],
    [
      0.2,
      0.2,
      0.6,
      0
    ],
    [
      0.13,
      0.2,
      0.67,
      0
    ],
    [
      0,
      0,
      0.03,
      0.97
    ],
    [
      0,
      0,
      0,
      1
    ],
    [
      0,
      0,
      0,
      0
    ]
  ],
  "BaseDaylength":[
    [
      0,
      0,
      7,
      7,
      0,
      0
    ],
    "h"
  ],
  "BeginSensitivePhaseHeatStress":[
    620,
    "°C d"
  ],
  "CriticalTemperatureHeatStress":[
    31,
    "°C"
  ],
  "CropHeightP1":6,
  "CropHeightP2":0.5,
  "CropSpecificMaxRootingDepth":1.3,
  "DaylengthRequirement":[
    [
      0,
      20,
      20,
      20,
      0,
      0
    ],
    "h"
  ],
  "Description":"",
  "DroughtStressThreshold":[
    1,
    0.9,
    1,
    1,
    0.9,
    0.8
  ],
  "EndSensitivePhaseHeatStress":[
    740,
    "°C d"
  ],
  "FrostDehardening":5.05,
  "FrostHardening":0.014,
  "HeatSumIrrigationEnd":1676,
  "HeatSumIrrigationStart":461,
  "LT50cultivar":-24,
  "LatestHarvestDoy":-1,
  "LowTemperatureExposure":0.654,
  "MaxAssimilationRate":30,
  "MaxCropHeight":[
    0.83,
    "m"
  ],
  "OptimumTemperature":[
    [
      30,
      30,
      30,
      30,
      30,
      30
    ],
    "°C"
  ],
  "OrganIdsForCutting":[],
  "OrganIdsForPrimaryYield":[
    {
      "organId":4,
      "type":"YieldComponent",
      "yieldDryMatter":0.86,
      "yieldPercentage":0.85
    }
  ],
  "OrganIdsForSecondaryYield":[
    {
      "organId":2,
      "type":"YieldComponent",
      "yieldDryMatter":0.86,
      "yieldPercentage":0.85
    },
    {
      "organId":3,
      "type":"YieldComponent",
      "yieldDryMatter":0.86,
      "yieldPercentage":0.9
    }
  ],
  "OrganSenescenceRate":[
    [
      0,
      0,
      0,
      0
    ],
    [
      0,
      0,
      0,
      0
    ],
    [
      0,
      0,
      0,
      0
    ],
    [
      0,
      0,
      0,
      0
    ],
    [
      0,
      0.05,
      0,
      0
    ],
    [
      0,
      0.05,
      0,
      0
    ]
  ],
  "Perennial":false,
  "ResidueNRatio":0.5,
  "RespiratoryStress":0.54,
  "SpecificLeafArea":[
    [
      0.002,
      0.0018,
      0.0017,
      0.0016,
      0.0015,
      0.0015
    ],
    "ha kg-1"
  ],
  "StageKcFactor":[
    [
      0.4,
      0.7,
      1.1,
      1.1,
      0.8,
      0.25
    ],
    "1;0"
  ],
  "StageTemperatureSum":[
    [
      148,
      284,
      380,
      180,
      420,
      25
    ],
    "°C d"
  ],
  "VernalisationRequirement":[
    0,
    50,
    0,
    0,
    0,
    0
  ],
  "type":"CultivarParameters"
}
```