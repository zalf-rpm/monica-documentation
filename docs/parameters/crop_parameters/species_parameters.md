# Species-level crop parameters

This section describes the JSON properties used to configure species-level crop parameters in MONICA.

Species parameters describe characteristics shared by all cultivars of a species. Cultivar-specific properties are defined separately.

## JSON conventions

- `Number` means a JSON number and may contain either an integer or a decimal value.
- `Integer` means a whole-number JSON value.
- Stage-dependent arrays must use the same stage order and normally have the same length as `BaseTemperature`.
- Organ-dependent arrays use this order:

        1. root
        2. leaf
        3. steam or shoot
        4. storage organ, such as fruit, grain, or ear

- Missing properties are not necessarily reported as errors. Depending on the property, MONICA may retain a default value. Arrays required by an enabled model component must nevertheless contain the expected number of entries.

---

## General and physiological parameters

| Parameter name                            | Type    | Unit   | Description                                                                                                                  | Example                                        |
|-------------------------------------------|---------|--------|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|
| `type`                                    | String  |        | Serialization metadata identifying the object as species parameters                                                          | `"type": "SpeciesParameters"`                  |
| `SpeciesName`                             | String  |        | Identifier or name of the crop species                                                                                       | `"SpeciesName": "wheat"`                       |
| `CarboxylationPathway`                    | Integer |        | Photosynthetic pathway type: `1` = C3, `2` = C4                                                                              | `"CarboxylationPathway": 1`                    |
| `DefaultRadiationUseEfficiency`           | Number  | g MJ-1 | Default radiation-use-efficiency coefficient used by the crop photosynthesis calculation                                     | `"DefaultRadiationUseEfficiency": 0.5`         |
| `MinimumTemperatureForAssimilation`       | Number  | °C     | Lower temperature threshold for photosynthetic assimilation                                                                  | `"MinimumTemperatureForAssimilation": 4`       |
| `OptimumTemperatureForAssimilation`       | Number  | °C     | Optimum temperature for photosynthetic assimilation                                                                          | `"OptimumTemperatureForAssimilation": 27`      |
| `MaximumTemperatureForAssimilation`       | Number  | °C     | Upper temperature threshold for photosynthetic assimilation                                                                  | `"MaximumTemperatureForAssimilation": 35`      |
| `InitialKcFactor`                         | Number  |        | Crop coefficient at the beginning of crop development                                                                        | `"InitialKcFactor": 0.4`                       |
| `FieldConditionModifier`                  | Number  |        | Multiplier applied to assimilate production to represent unspecified field limitations. A value of `1` applies no reduction. | `"FieldConditionModifier": 1`                  |
| `AssimilateReallocation`                  | Number  |        | Fraction of senescing organ biomass that can be reallocated to the storage organ                                             | `"AssimilateReallocation": 0.3`                |
| `DevelopmentAccelerationByNitrogenStress` | Integer |        | Switch controlling whether nitrogen stress accelerates crop development: normally `0` = disabled and `1` = enabled           | `"DevelopmentAccelerationByNitrogenStress": 1` |
| `DroughtImpactOnFertilityFactor`          | Number  |        | Controls the reduction in reproductive fertility caused by severe drought. A value of `0` disables this response.            | `"DroughtImpactOnFertilityFactor": 0`          |
| `LimitingTemperatureHeatStress`           | Number  | °C     | Limiting temperature used by the crop heat-stress response                                                                   | `"LimitingTemperatureHeatStress": 40`          |
| `CuttingDelayDays`                        | Integer | d      | Number of days for which crop development or growth is delayed after cutting                                                 | `"CuttingDelayDays": 0`                        |

---

## Development-stage parameters

| Parameter name               | Type           | Unit         | Description                                                                                                                                                                                      | Example                                                               |
|------------------------------|----------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| `BaseTemperature`            | Number (Array) | °C           | Base temperature for thermal-time accumulation in each developmental stage. The array length defines the number of developmental stages.                                                         | `"BaseTemperature": [0, 1, 1, 1, 9, 9]`                               |
| `CriticalOxygenContent`      | Number (Array) | m3 m-3       | Critical soil oxygen content for each developmental stage                                                                                                                                        | `"CriticalOxygenContent": [0.08, 0.08, 0.08, 0.08, 0.08, 0.08]`       |
| `StageMaxRootNConcentration` | Number (Array) | kg N kg-1 DM | Maximum root nitrogen concentration in each developmental stage                                                                                                                                  | `"StageMaxRootNConcentration": [0.02, 0.02, 0.012, 0.01, 0.01, 0.01]` |
| `StageMobilFromStorageCoeff` | Number (Array) | fraction     | Stage-specific coefficient controlling mobilization of biomass from the storage organ. If omitted, MONICA initializes one zero for each entry in `CriticalOxygenContent`.                        | `"StageMobilFromStorageCoeff": [0, 0, 0, 0, 0, 0]`                    |
| `StageAtMaxHeight`           | Number         | stage        | Developmental stage at which the crop reaches its maximum height. Normally specified as a whole stage number.                                                                                    | `"StageAtMaxHeight": 3`                                               |
| `StageAtMaxDiameter`         | Number         | stage        | Stage at which maximum crop diameter is reached. Currently has no effect on subsequent model calculations.                                                                                       | `"StageAtMaxDiameter": 2`                                             |
| `StageAfterCut`              | Integer        | stage        | Developmental stage to which the crop is reset after cutting. MONICA converts this value to its zero-based internal stage index by subtracting one. Use `0` only for species that are never cut. | `"StageAfterCut": 0`                                                  |
| `TransitionStageLeafExp`     | Integer        | stage        | Stage through which the early-stage temperature response for leaf expansion is used. `-1` disables the transition.                                                                               | `"TransitionStageLeafExp": -1`                                        |

---

## Organ parameters

Organ arrays use the order root, leaf, stem/shoot, and storage organ.

| Parameter name                | Type            | Unit       | Description                                                                                                                            | Example                                                    |
|-------------------------------|-----------------|------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| `AbovegroundOrgan`            | Boolean (Array) |            | Indicates whether each organ contributes to aboveground biomass                                                                        | `"AbovegroundOrgan": [false, true, true, true]`            |
| `StorageOrgan`                | Boolean (Array) |            | Indicates which organ is treated as the crop's storage organ                                                                           | `"StorageOrgan": [false, false, false, true]`              |
| `InitialOrganBiomass`         | Number (Array)  | kg DM ha-1 | Initial dry-matter biomass of each organ at crop establishment                                                                         | `"InitialOrganBiomass": [53, 53, 0, 0]`                    |
| `OrganGrowthRespiration`      | Number (Array)  | fraction   | Organ-specific fraction of allocated assimilates consumed by growth respiration                                                        | `"OrganGrowthRespiration": [0.306, 0.315, 0.338, 0.291]`   |
| `OrganMaintenanceRespiration` | Number (Array)  | d-1        | Organ-specific maintenance-respiration coefficient                                                                                     | `"OrganMaintenanceRespiration": [0.01, 0.03, 0.015, 0.01]` |
| `MaxCropDiameter`             | Number          | m          | Maximum crop diameter. Currently affects only an internal green-area-index variable that is not used by subsequent model calculations. | `"MaxCropDiameter": 0.005`                                 |
| `PlantDensity`                | Integer         | plants m-2 | Number of plants per square meter. Currently has no effect on subsequent model calculations.                                           | `"PlantDensity": 220`                                      |

---

## Nitrogen parameters

| Parameter name                            | Type   | Unit              | Description                                                                                             | Example                                    |
|-------------------------------------------|--------|-------------------|---------------------------------------------------------------------------------------------------------|--------------------------------------------|
| `MinimumNConcentration`                   | Number | kg N kg-1 DM      | Minimum nitrogen concentration in aboveground biomass before maximum nitrogen stress is reached         | `"MinimumNConcentration": 0.005`           |
| `NConcentrationAbovegroundBiomass`        | Number | kg N kg-1 DM      | Initial nitrogen concentration in aboveground biomass                                                   | `"NConcentrationAbovegroundBiomass": 0.06` |
| `NConcentrationRoot`                      | Number | kg N kg-1 DM      | Initial nitrogen concentration in root biomass                                                          | `"NConcentrationRoot": 0.02`               |
| `NConcentrationB0`                        | Number |                   | Coefficient controlling the biomass-dependent curvature of the critical nitrogen-concentration function | `"NConcentrationB0": 2`                    |
| `NConcentrationPN`                        | Number |                   | Scaling parameter of the critical nitrogen-concentration function                                       | `"NConcentrationPN": 1.6`                  |
| `LuxuryNCoeff`                            | Number |                   | Multiplier defining nitrogen uptake above the critical nitrogen requirement                             | `"LuxuryNCoeff": 1.3`                      |
| `PartBiologicalNFixation`                 | Number | fraction          | Fraction of crop nitrogen demand that can be supplied by biological nitrogen fixation                   | `"PartBiologicalNFixation": 0`             |
| `MaxNUptakeParam`                         | Number | kg N m-1 root d-1 | Parameter used to limit daily nitrogen uptake as a function of total root length and crop development   | `"MaxNUptakeParam": 3.145`                 |
| `SamplingDepth`                           | Number | m                 | Soil depth over which mineral nitrogen is evaluated by the automatic Nmin fertilization method          | `"SamplingDepth": 0.9`                     |
| `TargetN30`                               | Number | kg N ha-1         | Target amount of available mineral nitrogen in the upper 0.30 m of the soil                             | `"TargetN30": 120`                         |
| `TargetNSamplingDepth`                    | Number | kg N ha-1         | Target amount of available mineral nitrogen from the soil surface down to `SamplingDepth`               | `"TargetNSamplingDepth": 230`              |

---

## Root parameters

| Parameter name                 | Type   | Unit       | Description                                                                                                                                 | Example                             |
|--------------------------------|--------|------------|---------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------|
| `InitialRootingDepth`          | Number | m          | Rooting depth at crop establishment                                                                                                         | `"InitialRootingDepth": 0.1`        |
| `MinimumTemperatureRootGrowth` | Number | °C         | Minimum temperature for active root growth                                                                                                  | `"MinimumTemperatureRootGrowth": 0` |
| `RootGrowthLag`                | Number | °C d       | Thermal-time threshold or offset controlling the beginning of rooting-depth growth. Negative values are permitted.                          | `"RootGrowthLag": -30`              |
| `RootPenetrationRate`          | Number | m (°C d)-1 | Increase in rooting depth per accumulated degree-day                                                                                        | `"RootPenetrationRate": 0.0011`     |
| `RootDistributionParam`        | Number |            | Legacy root-distribution parameter. Its former calculation is currently commented out. Root distribution is controlled by `RootFormFactor`. | `"RootDistributionParam": 0.002787` |
| `RootFormFactor`               | Number | m-1        | Exponential shape coefficient used to distribute root density over soil depth                                                               | `"RootFormFactor": 3`               |
| `SpecificRootLength`           | Number | m g-1 DM   | Root length per unit root dry-matter biomass. It is used to calculate total root length from root biomass.                                  | `"SpecificRootLength": 300`         |

---

## Perennial-crop dormancy parameters

| Parameter name     | Type    | Unit        | Description                                                                                | Example                 |
|--------------------|---------|-------------|--------------------------------------------------------------------------------------------|-------------------------|
| `DormancyStartDoy` | Integer | day of year | Day of year on which dormancy begins for a perennial crop. `0` means unset.                | `"DormancyStartDoy": 0` |
| `DormancyEndDoy`   | Integer | day of year | Day of year on which dormancy ends and thermal-time accumulation resumes. `0` means unset. | `"DormancyEndDoy": 0`   |

---

## Example species parameter object

The following is a structurally complete illustrative wheat parameter object. Parameter values should be calibrated for the intended cultivar, environment, and model configuration. The example should not be treated as a universally recommended wheat parameterization.

```json
{
  "SpeciesName":"wheat",
  "AbovegroundOrgan":[
    false,
    true,
    true,
    true
  ],
  "AssimilateReallocation":0.3,
  "BaseTemperature":[
    0,
    1,
    1,
    1,
    9,
    9
  ],
  "CarboxylationPathway":1,
  "CriticalOxygenContent":[
    0.08,
    0.08,
    0.08,
    0.08,
    0.08,
    0.08
  ],
  "CuttingDelayDays":0,
  "DefaultRadiationUseEfficiency":0.5,
  "DevelopmentAccelerationByNitrogenStress":1,
  "DroughtImpactOnFertilityFactor":0,
  "FieldConditionModifier":1,
  "InitialKcFactor":0.4,
  "InitialOrganBiomass":[
    53,
    53,
    0,
    0
  ],
  "InitialRootingDepth":0.1,
  "LimitingTemperatureHeatStress":40,
  "LuxuryNCoeff":1.3,
  "MaxNUptakeParam":3.145,
  "MinimumNConcentration":0.005,
  "MinimumTemperatureForAssimilation":0,
  "OptimumTemperatureForAssimilation":27,
  "MaximumTemperatureForAssimilation":35,
  "MinimumTemperatureRootGrowth":0,
  "NConcentrationAbovegroundBiomass":0.06,
  "NConcentrationB0":2,
  "NConcentrationPN":1.6,
  "NConcentrationRoot":0.02,
  "OrganGrowthRespiration":[
    0.306,
    0.315,
    0.338,
    0.291
  ],
  "OrganMaintenanceRespiration":[
    0.01,
    0.03,
    0.015,
    0.01
  ],
  "PartBiologicalNFixation":0,
  "RootFormFactor":3,
  "RootGrowthLag":-30,
  "RootPenetrationRate":0.0011,
  "SamplingDepth":0.9,
  "SpecificRootLength":300,
  "StageAfterCut":0,
  "StageAtMaxHeight":3,
  "StageMaxRootNConcentration":[
    0.02,
    0.02,
    0.012,
    0.01,
    0.01,
    0.01
  ],
  "StorageOrgan":[
    false,
    false,
    false,
    true
  ],
  "TargetN30":120,
  "TargetNSamplingDepth": 230,
  "type": "SpeciesParameters"
}
```

---

## Consistency requirements

Before using a species parameter object, verify the following:

- `BaseTemperature`, `CriticalOxygenContent`, `StageMaxRootNConcentration`, and `StageMobilFromStorageCoeff` contain one entry for every developmental stage.
- `AbovegroundOrgan`, `StorageOrgan`, `InitialOrganBiomass`, `OrganGrowthRespiration`, and `OrganMaintenanceRespiration` contain one entry for every crop organ.
- Exactly one organ is normally marked as the storage organ.
- Stage references are valid for the number of developmental stages.
- `DormancyStartDoy` and `DormancyEndDoy` are set only for perennial crops that use dormancy.