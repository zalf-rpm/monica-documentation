# Soil Moisture Parameters

This section provides an overview of the key-value pairs in the `UserSoilMoistureParameters` JSON file used by MONICA. These parameters control how water movement, evaporation, snow dynamics, and hydraulic conductivity are simulated in the soil.

---

## List of soil moisture parameters

| Parameter Name                        | Type    | Unit     | Description                                                                                      | Example                                          |
|---------------------------------------|---------|----------|--------------------------------------------------------------------------------------------------|--------------------------------------------------|
| `CorrectionRain`                      | Float   |          | Correction factor applied to rainfall input                                                      | `"CorrectionRain": 1`                            |
| `CorrectionSnow`                      | Float   |          | Correction factor applied to snowfall input                                                      | `"CorrectionSnow": 1.14`                         |
| `CriticalMoistureDepth`               | Float   | m        | Soil depth used to calculate critical moisture for evaporation reduction                         | `"CriticalMoistureDepth": 0.3`                   |
| `EvaporationZeta`                     | Float   | mm       | Parameter controlling the reduction of soil evaporation with increasing soil dryness             | `"EvaporationZeta": 40`                          |
| `GroundwaterDischarge`                | Float   | mm d-1   | Rate at which groundwater discharges from the soil profile                                       | `"GroundwaterDischarge": 3`                      |
| `HydraulicConductivityRedux`          | Float   |          | Reduction factor applied to hydraulic conductivity to account for macropore flow or bypass       | `"HydraulicConductivityRedux": 0.1`              |
| `KcFactor`                            | Float   |          | Crop coefficient used to scale reference evapotranspiration to actual evapotranspiration         | `"KcFactor": 0.75`                               |
| `MaxPercolationRate`                  | Float   | mm d-1   | Maximum rate at which water can percolate downward through the soil                              | `"MaxPercolationRate": 10`                       |
| `MaximumEvaporationImpactDepth`       | Float   | dm       | Maximum soil depth affected by evaporation processes                                             | `"MaximumEvaporationImpactDepth": 5`             |
| `MoistureInitValue`                   | Float   | m3 m-3   | Initial soil moisture value used when no measured data is available                              | `"MoistureInitValue": 0`                         |
| `NewSnowDensityMin`                   | Float   | kg dm-3  | Minimum density of newly fallen snow                                                             | `"NewSnowDensityMin": 0.1`                       |
| `RefreezeParameter1`                  | Float   |          | First parameter controlling the rate of refreezing of melted snow                               | `"RefreezeParameter1": 1.5`                      |
| `RefreezeParameter2`                  | Float   |          | Second parameter controlling the rate of refreezing of melted snow                              | `"RefreezeParameter2": 0.36`                     |
| `RefreezeTemperature`                 | Float   | °C       | Temperature threshold below which refreezing of liquid water in the snowpack occurs             | `"RefreezeTemperature": -1.7`                    |
| `SaturatedHydraulicConductivity`      | Float   | mm d-1   | Hydraulic conductivity of the soil when fully saturated                                         | `"SaturatedHydraulicConductivity": 8640`         |
| `SnowAccumulationTresholdTemperature` | Float   | °C       | Temperature threshold below which precipitation is accumulated as snow                          | `"SnowAccumulationTresholdTemperature": 1.8`     |
| `SnowMaxAdditionalDensity`            | Float   | kg dm-3  | Maximum additional density that snow can reach due to compaction                                | `"SnowMaxAdditionalDensity": 0.25`               |
| `SnowMeltTemperature`                 | Float   | °C       | Temperature threshold above which snowmelt begins                                               | `"SnowMeltTemperature": 0.31`                    |
| `SnowPacking`                         | Float   |          | Rate at which snow compacts over time                                                           | `"SnowPacking": 0.01`                            |
| `SnowRetentionCapacityMax`            | Float   | kg kg-1  | Maximum fraction of liquid water the snowpack can retain before runoff occurs                   | `"SnowRetentionCapacityMax": 0.17`               |
| `SnowRetentionCapacityMin`            | Float   | kg kg-1  | Minimum fraction of liquid water retained in the snowpack                                       | `"SnowRetentionCapacityMin": 0.05`               |
| `SurfaceRoughness`                    | Float   | m        | Surface roughness length used in aerodynamic resistance calculations for evaporation            | `"SurfaceRoughness": 0.02`                       |
| `TemperatureLimitForLiquidWater`      | Float   | °C       | Temperature below which all water in the snowpack is considered frozen                          | `"TemperatureLimitForLiquidWater": -3`           |
| `XSACriticalSoilMoisture`             | Float   | m3 m-3   | Critical soil moisture threshold used in the excess water and surface runoff calculation        | `"XSACriticalSoilMoisture": 0.1`                 |
| `type`                                | String  |          | Declares that this JSON defines soil moisture parameters                                        | `"type": "UserSoilMoistureParameters"`           |

!!! note
    All parameters apply globally to the soil profile unless otherwise specified. These values are typically set in the `general/soilmoisture.json` file within the `monica-parameters` repository.

---

## Example: `soilmoisture.json`

```json
{
    "CorrectionRain": 1,
    "CorrectionSnow": 1.14,
    "CriticalMoistureDepth": 0.3,
    "EvaporationZeta": 40,
    "GroundwaterDischarge": 3,
    "HydraulicConductivityRedux": 0.1,
    "KcFactor": 0.75,
    "MaxPercolationRate": 10,
    "MaximumEvaporationImpactDepth": 5,
    "MoistureInitValue": 0,
    "NewSnowDensityMin": 0.1,
    "RefreezeParameter1": 1.5,
    "RefreezeParameter2": 0.36,
    "RefreezeTemperature": -1.7,
    "SaturatedHydraulicConductivity": 8640,
    "SnowAccumulationTresholdTemperature": 1.8,
    "SnowMaxAdditionalDensity": 0.25,
    "SnowMeltTemperature": 0.31,
    "SnowPacking": 0.01,
    "SnowRetentionCapacityMax": 0.17,
    "SnowRetentionCapacityMin": 0.05,
    "SurfaceRoughness": 0.02,
    "TemperatureLimitForLiquidWater": -3,
    "XSACriticalSoilMoisture": 0.1,
    "type": "UserSoilMoistureParameters"
}
```