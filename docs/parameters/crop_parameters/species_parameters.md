# Species-level crop parameters

This section provides an overview of the key-value pairs in a JSON file containing **species parameters** for a crop in MONICA.

---

## List of species parameters

| Parameter name                            | Type            | Unit              | Description                                                                                                                                     | Example                                                              |
|-------------------------------------------|-----------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| `SpeciesName`                             | String          |                   | The name of the crop species                                                                                                                    | `"SpeciesName":"wheat"`                                              |
| `AbovegroundOrgan`                        | Boolean (Array) |                   | Indicates the presence (`true`) or absense (`false`) of aboveground organs. Each value corresponds to: [1] root, [2] leaf, [3] stem, [4] fruit. | `"AbovegroundOrgan":[false, true, true, true]`                       |
| `AssimilateReallocation`                  | Float           |                   | Fraction of carbon and nitrogen that can be reallocated between organs                                                                          | `"AssimilateReallocation":0.3`                                       |
| `BaseTemperature`                         | Integer (Array) | °C                | Base temperatures for different phenological stages (6-7 stages)                                                                                | `"BaseTemperature":[0, 1, 1, 1, 9, 9]`                               |
| `CarboxylationPathway`                    | Integer         |                   | Photosynthetic pathway type (`1` = C3, `2` = C4)                                                                                                | `"CarboxylationPathway":1`                                           |
| `CriticalOxygenContent`                   | Float (Array)   | m3 m-3            | Minimum soil oxygen content per growth stage                                                                                                    | `"CriticalOxygenContent":[0.08, 0.08, 0.08, 0.08, 0.08, 0.08]`       |
| `CuttingDelayDays`                        | Integer         | day               | Number of days required for plant recovery after cutting                                                                                        | `"CuttingDelayDays":0`                                               |
| `DefaultRadiationUseEfficiency`           | Float           | g MJ-1            | Efficiency of converting solar radiation into biomass                                                                                           | `"DefaultRadiationUseEfficiency":0.5`                                |
| `DevelopmentAccelerationByNitrogenStress` | Integer         |                   | Effect of nitrogen stress on crop development rate                                                                                              | `"DevelopmentAccelerationByNitrogenStress":1`                        |
| `DroughtImpactOnFertilityFactor`          | Integer         |                   | Factor reducing fertility under drought stress                                                                                                  | `"DroughtImpactOnFertilityFactor":0`                                 |
| `FieldConditionModifier`                  | Integer         |                   | Generic reduction factor for unknown field limitations                                                                                          | `"FieldConditionModifier":1`                                         |
| `InitialKcFactor`                         | Float           |                   | Crop coefficient (Kc) for bare soil situation                                                                                                   | `"InitialKcFactor":0.4`                                              |
| `InitialOrganBiomass`                     | Integer (Array) | g m-2             | Initial biomass for [1] root, [2] leaf, [3] stem, [4] fruit                                                                                     | `"InitialOrganBiomass":[53, 53, 0, 0]`                               |
| `InitialRootingDepth`                     | Float           | m                 | Rooting depth at sowing or transplanting                                                                                                        | `"InitialRootingDepth":0.1`                                          |
| `LimitingTemperatureHeatStress`           | Integer         | °C                | Critical temperature threshold for heat stress                                                                                                  | `"LimitingTemperatureHeatStress":40`                                 |
| `LuxuryNCoeff`                            | Float           |                   | Coefficient describing luxury nitrogen uptake (above critical need)                                                                             | `"LuxuryNCoeff":1.3`                                                 |
| `MaxNUptakeParam`                         | Float           | kg m root-1 day-1 | Maximum nitrogen flux into the crop through roots                                                                                               | `"MaxNUptakeParam":3.145`                                            |
| `MinimumNConcentration`                   | Float           | kg kg-1           | Minimum nitrogen concentration in aboveground biomass                                                                                           | `"MinimumNConcentration":0.005`                                      |
| `MinimumTemperatureForAssimilation`       | Integer         | °C                | Minimum temperature for photosynthetic assimilation                                                                                             | `"MinimumTemperatureForAssimilation":0`                              |
| `OptimumTemperatureForAssimilation`       | Integer         | °C                | Optimum temperature for photosynthetic assimilation                                                                                             | `"OptimumTemperatureForAssimilation":27`                             |
| `MaximumTemperatureForAssimilation`       | Integer         | °C                | Maximum temperature for photosynthetic assimilation                                                                                             | `"MaximumTemperatureForAssimilation":35`                             |
| `MinimumTemperatureRootGrowth`            | Integer         | °C                | Minimum temperature for active root growth                                                                                                      | `"MinimumTemperatureRootGrowth":0`                                   |
| `NConcentrationAbovegroundBiomass`        | Float           |                   | Initial nitrogen concentration in aboveground biomass                                                                                           | `"NConcentrationAbovegroundBiomass":0.06`                            |
| `NConcentrationB0`                        | Float           |                   | Curvature parameter of the critical nitrogen concentration curve                                                                                | `"NConcentrationB0":2`                                               |
| `NConcentrationPN`                        | Float           |                   | Shape parameter of the critical nitrogen concentration curve                                                                                    | `"NConcentrationPN":1.6`                                             |
| `NConcentrationRoot`                      | Float           | kg N kg-1         | Initial nitrogen concentration in roots                                                                                                         | `"NConcentrationRoot":0.02`                                          |
| `OrganGrowthRespiration`                  | Float (Array)   |                   | Respiration rates for [1] root, [2] leaf, [3] stem, [4] fruit during growth                                                                     | `"OrganGrowthRespiration":[0.306, 0.315, 0.338, 0.291]`              |
| `OrganMaintenanceRespiration`             | Float (Array)   |                   | Maintenance respiration rates for plant organs                                                                                                  | `"OrganMaintenanceRespiration":[0.01, 0.03, 0.015, 0.01]`            |
| `PartBiologicalNFixation`                 | Integer         |                   | Biological nitrogen fixation                                                                                                                    | `"PartBiologicalNFixation":0`                                        |
| `PlantDensity`                            | Integer         | plant m-2         | Number of plants per square meter (currently not used by the model)                                                                             | `"PlantDensity":220`                                                 |
| `RootDistributionParam`                   | Float           |                   | Parameter of the root length density distribution over soil depth                                                                               | `"RootDistributionParam":0.002787`                                   |
| `RootFormFactor`                          | Integer         |                   | Factor describing root mass distribution over soil depth                                                                                        | `"RootFormFactor":3`                                                 |
| `RootGrowthLag`                           | Integer         |                   | Thermal time delay before root growth begins                                                                                                    | `"RootGrowthLag":-30`                                                |
| `RootPenetrationRate`                     | Float           | m °C-1 d-1        | Rate of vertical root growth through soil                                                                                                       | `"RootPenetrationRate":0.0011`                                       |
| `SamplingDepth`                           | Float           | m                 | Depth used to calculate available soil nitrogen                                                                                                 | `"SamplingDepth":0.9`                                                |
| `SpecificRootLength`                      | Integer         | m g-1             | Length of root after the crop is cut                                                                                                            | `"SpecificRootLength":300`                                           |
| `StageAfterCut`                           | Integer         |                   | Development stage after a cutting event                                                                                                         | `"StageAfterCut":0`                                                  |
| `StageAtMaxDiameter`                      | Integer         |                   | Development stage at which the plant reaches maximum diameter                                                                                   | `"StageAtMaxDiameter":2`                                             |
| `StageAtMaxHeight`                        | Integer         |                   | Development stage at which the plant reaches maximum height                                                                                     | `"StageAtMaxHeight":3`                                               |
| `StageMaxRootNConcentration`              | Float (Array)   | kg kg-1           | Maximum nitrogen concentration in roots per stage                                                                                               | `"StageMaxRootNConcentration":[0.02, 0.02, 0.012, 0.01, 0.01, 0.01]` |
| `StorageOrgan`                            | Boolean (Array) |                   | Indicates storage organs: [1] root, [2] leaf, [3] stem, [4] fruit                                                                               | `"StorageOrgan":[false, false, false, true]`                         |
| `TargetN30`                               | Integer         |                   | Target nitrogen concentration when roots are <= 30 cm deep                                                                                      | `"TargetN30":120`                                                    |
| `TargetNSamplingDepth`                    | Integer         |                   | Target nitrogen concentration at maximum rooting depth                                                                                          | `"TargetNSamplingDepth":230`                                         |
| `type`                                    | String          |                   | Declares that this JSON defines species parameters                                                                                              | `"type":"SpeciesParameters"`                                         |


## Example: `wheat.json`

The following example shows a complete species parameter file for wheat.

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
  "MaxCropDiameter":0.005,
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
  "PlantDensity": 220,
  "RootDistributionParam":0.002787,
  "RootFormFactor":3,
  "RootGrowthLag":-30,
  "RootPenetrationRate":0.0011,
  "SamplingDepth":0.9,
  
  "SpecificRootLength":300,
  "StageAfterCut":0,
  "StageAtMaxDiameter":2,
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
  "TargetNSamplingDepth":230,
  "type":"SpeciesParameters"
}
```