# Species-level crop parameters

This section provides an overview of the key-value pairs in a JSON file containing **cultivar parameters** for a crop in MONICA. Cultivar parameters describe properties that differ between cultivars of the same species. They do not replace species parameters but complement them.

---

## List of cultivar parameters

| Parameter Name                  | Type                    | Unit             | Description                                                                                                                      | Example                                                                                                                                                                                              |
|---------------------------------|-------------------------|------------------|----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `CultivarName`                  | String                  |                  | The name of the crop cultivar                                                                                                    | `"CultivarName":"winter wheat"`                                                                                                                                                                      |
| `WinterCrop`                    | Boolean                 |                  | Defines if the cultivar is grown as a winter crop (`true`) or not (`false`)                                                      | `"WinterCrop": true`                                                                                                                                                                                 |
| `AssimilatePartitioningCoeff`   | Float (Array of Arrays) |                  | Matrix of assimilate partitioning to the different plant organs per developmental stage                                          | `"AssimilatePartitioningCoeff":[[0.5,0.5,0,0],[0.2,0.2,0.6,0],[0.13,0.2,0.67,0],[0,0,0.03,0.97],[0,0,0,1],[0,0,0,0]]`                                                                                |
| `BaseDaylength`                 | Integer (Array)         | h                | Defines the base daylength below which the plant will not develop                                                                | `"BaseDaylength":[[0,0,7,7,0,0],"h"]`                                                                                                                                                                |
| `BeginSensitivePhaseHeatStress` | Integer                 | °C d             | Defines a temperature sum after which the plant is particularly sensitive to floret sterility through heat                       | `"BeginSensitivePhaseHeatStress":[620,"°C d"]`                                                                                                                                                       |
| `CriticalTemperatureHeatStress` | Integer                 | °C               | Defines the temperature threshold above which the florets take damage                                                            | `"CriticalTemperatureHeatStress":[31,"°C"]`                                                                                                                                                          |
| `CropHeightP1`                  | Float                   | m                | Defines the growth pattern over time                                                                                             | `"CropHeightP1":6`                                                                                                                                                                                   |
| `CropHeightP2`                  | Float                   | m                | Defines the growth pattern over time                                                                                             | `"CropHeightP2":0.5`                                                                                                                                                                                 |
| `CropSpecificMaxRootingDepth`   | Float                   | m                | Defines the maximum rooting depth potential of the plant                                                                         | `"CropSpecificMaxRootingDepth":1.3`                                                                                                                                                                  |
| `DaylengthRequirement`          | Integer (Array)         | h                | Defines the daylength requirement per developmental stage for the plant's development. Negative values denote long-night plants. | `"DaylengthRequirement":[[0,20,20,20,0,0],"h"]`                                                                                                                                                      |
| `Description`                   | String                  |                  | Optional textual description of the cultivar                                                                                     | `"Description":""`                                                                                                                                                                                   |
| `DroughtStressThreshold`        | Float (Array)           |                  | Defines the relation between actual and potential transpiration below which the plant experiences drought stress                 | `"DroughtStressThreshold":[1,0.9,1,1,0.9,0.8]`                                                                                                                                                       |
| `EndSensitivePhaseHeatStress`   | Integer                 | °C d             | Defines a temperature sum after which the plant is not any more sensitive to floret sterility through heat                       | `"EndSensitivePhaseHeatStress":[740,"°C d"]`                                                                                                                                                         |
| `FrostDehardening`              | Float                   | °C-1 d-1         | Modulates the daily change in LT50 due to deacclimation                                                                          | `"FrostDehardening":5.05`                                                                                                                                                                            |
| `FrostHardening`                | Float                   | °C-1 d-1         | Modulates the daily change in LT50 due to acclimation                                                                            | `"FrostHardening":0.014`                                                                                                                                                                             |
| `HeatSumIrrigationEnd`          | Integer                 | °C d             | Accumulated heat sum at which irrigation ends                                                                                    | `"HeatSumIrrigationEnd":1676`                                                                                                                                                                        |
| `HeatSumIrrigationStart`        | Integer                 | °C d             | Accumulated heat sum at which irrigation starts                                                                                  | `"HeatSumIrrigationStart":461`                                                                                                                                                                       |
| `LT50cultivar`                  | Integer                 | °C               | Threshold temperature for the cultivar, below which 50% of the population is killed by frost                                     | `"LT50cultivar":-24`                                                                                                                                                                                 |
| `LatestHarvestDoy`              | Integer                 | day of year      | Latest possible harvest day of year                                                                                              | `"LatestHarvestDoy":-1`                                                                                                                                                                              |
| `LowTemperatureExposure`        | Float                   |                  | Exposure of the cultivar to low temperatures                                                                                     | `"LowTemperatureExposure":0.654`                                                                                                                                                                     |
| `MaxAssimilationRate`           | Integer                 | kg CO2 ha leaf-1 | Maximum assimilation rate                                                                                                        | `"MaxAssimilationRate":30`                                                                                                                                                                           |
| `MaxCropHeight`                 | Float                   | m                | Maximum height of the crop                                                                                                       | `"MaxCropHeight":[0.83,"m"]`                                                                                                                                                                         |
| `OptimumTemperature`            | Integer (Array)         | °C               | Optimum temperatures for different growth stages                                                                                 | `"OptimumTemperature":[[30,30,30,30,30,30],"°C"]`                                                                                                                                                    |
| `OrganIdsForCutting`            | Object (Array)          |                  | Defines the organs that are affected by cutting/mowing                                                                           | `"OrganIdsForCutting":[]`                                                                                                                                                                            |
| `OrganIdsForPrimaryYield`       | Object (Array)          |                  | Defines the organs that are primarily harvested                                                                                  | `"OrganIdsForPrimaryYield":[{"organId":4,"type":"YieldComponent","yieldDryMatter":0.86,"yieldPercentage":0.85}]`                                                                                     |
| `OrganIdsForSecondaryYield`     | Object (Array)          |                  | Defines the organs that are harvested as secondary products                                                                      | `"OrganIdsForSecondaryYield":[{"organId":2,"type":"YieldComponent","yieldDryMatter":0.86,"yieldPercentage":0.85},{"organId":3,"type":"YieldComponent","yieldDryMatter":0.86,"yieldPercentage":0.9}]` |
| `OrganSenescenceRate`           | Float (Array of Arrays) | d-1              | Defines the rate of senescence per organ and developmental phase                                                                 | `"OrganSenescenceRate":[[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0.05,0,0],[0,0.05,0,0]]`                                                                                                          |
| `Perennial`                     | Boolean                 |                  | Indicates whether the cultivar is perennial                                                                                      | `"Perennial":false`                                                                                                                                                                                  |
| `ResidueNRatio`                 | Float                   |                  | Ratio between the N concentration in the residues and in the marketable yield at harvest                                         | `"ResidueNRatio":0.5`                                                                                                                                                                                |
| `RespiratoryStress`             | Float                   |                  | Modulates the daily change in LT50 due to respiratory stress under snow cover                                                    | `"RespiratoryStress":0.54`                                                                                                                                                                           |
| `SpecificLeafArea`              | Float (Array)           | ha kg-1          | Defines the leaf surface per leaf mass for each developmental stage                                                              | `"SpecificLeafArea":[[0.002,0.0018,0.0017,0.0016,0.0015,0.0015],"ha kg-1"]`                                                                                                                          |
| `StageKcFactor`                 | Float (Array)           |                  | Modulates the reference grass evapotranspiration to represent the crop, specific for each developmental stage                    | `"StageKcFactor":[[0.4,0.7,1.1,1.1,0.8,0.25],"1;0"]`                                                                                                                                                 |
| `StageTemperatureSum`           | Integer (Array)         | °C d             | Defines the temperature sum per developmental stage that needs to accumulate to reach the next stage                             | `"StageTemperatureSum":[[148,284,380,180,420,25],"°C d"]`                                                                                                                                            |
| `VernalisationRequirement`      | Integer (Array)         | d                | Defines the number of low-temperature days the crop requires to induce flowering                                                 | `"VernalisationRequirement":[0,50,0,0,0,0]`                                                                                                                                                          |
| `type`                          | String                  |                  | Declares that this JSON defines cultivar parameters                                                                              | `"type":"CultivarParameters"`                                                                                                                                                                        |

!!! note
    Some parameters are encoded as arrays containing values and units. These additional strings are for documentation purposes only and are not interpreted by the model.

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