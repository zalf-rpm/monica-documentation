# Soil Moisture Parameters

This section documents the parameters in MONICA's `general/soil-moisture.json` file. They control infiltration, percolation, groundwater discharge, soil evaporation, precipitation partitioning, and snowpack processes.

---

## List of soil moisture parameters

| Parameter Name                        | Type   | Unit    | Description                                                                                                                                                                                                                          | Example                                      |
|---------------------------------------|--------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| `CorrectionRain`                      | Number |         | Multiplier applied to the liquid fraction of net precipitation after rain–snow partitioning                                                                                                                                          | `"CorrectionRain": 1`                        |
| `CorrectionSnow`                      | Number |         | Multiplier applied to the solid fraction of net precipitation after rain–snow partitioning                                                                                                                                           | `"CorrectionSnow": 1.14`                     |
| `CriticalMoistureDepth`               | Number | m       | Legacy parameter retained in the standard parameter file. It is not read or used by the current MONICA core.                                                                                                                         | `"CriticalMoistureDepth": 0.3`               |
| `EvaporationZeta`                     | Number |         | Shape factor used to distribute potential soil evaporation over the evaporation-affected soil layers. The implementation expects a value approximately in the range 0–40.                                                            | `"EvaporationZeta": 40`                      |
| `GroundwaterDischarge`                | Number | mm d-1  | Prescribed water flux used for drainage in soil layers influenced by groundwater                                                                                                                                                     | `"GroundwaterDischarge": 3`                  |
| `HydraulicConductivityRedux`          | Number |         | Multiplier applied to saturated hydraulic conductivity when calculating infiltration. The frost module may modify the effective multiplier according to soil frost and thaw conditions.                                              | `"HydraulicConductivityRedux": 0.1`          |
| `KcFactor`                            | Number |         | Fallback crop coefficient used to calculate potential evapotranspiration as `ET₀ × Kc` when no crop is growing. When a crop is present, the crop module supplies its own Kc value.                                                   | `"KcFactor": 0.75`                           |
| `MaxPercolationRate`                  | Number | mm d-1  | Maximum downward percolation rate allowed for a soil layer during one daily timestep                                                                                                                                                 | `"MaxPercolationRate": 10`                   |
| `MaximumEvaporationImpactDepth`       | Number | dm      | Maximum nominal depth over which potential soil evaporation is distributed. The implementation also uses this value when determining the affected layer indices.                                                                     | `"MaximumEvaporationImpactDepth": 5`         |
| `MoistureInitValue`                   | Number | m3 m-3  | Legacy/reserved initial-moisture parameter. It is read and serialized but is not currently used to initialize soil moisture in the MONICA core.                                                                                      | `"MoistureInitValue": 0`                     |
| `NewSnowDensityMin`                   | Number | kg dm-3 | Minimum density of newly fallen snow                                                                                                                                                                                                 | `"NewSnowDensityMin": 0.1`                   |
| `RefreezeParameter1`                  | Number |         | Coefficient controlling the amount of liquid water that refreezes in the snowpack below `RefreezeTemperature`                                                                                                                        | `"RefreezeParameter1": 1.5`                  |
| `RefreezeParameter2`                  | Number |         | Exponent applied to the temperature difference in the snow-refreezing equation                                                                                                                                                       | `"RefreezeParameter2": 0.36`                 |
| `RefreezeTemperature`                 | Number | °C      | Temperature threshold below which liquid water in the snowpack may refreeze                                                                                                                                                          | `"RefreezeTemperature": -1.7`                |
| `SaturatedHydraulicConductivity`      | Number | mm d-1  | Saturated hydraulic conductivity used by the infiltration calculation. The configured value is applied uniformly to the soil-moisture layers.                                                                                        | `"SaturatedHydraulicConductivity": 8640`     |
| `SnowAccumulationTresholdTemperature` | Number | °C      | Upper temperature threshold for precipitation partitioning. At or above this temperature, precipitation is entirely liquid. Between this value and `TemperatureLimitForLiquidWater`, precipitation is divided between rain and snow. | `"SnowAccumulationTresholdTemperature": 1.8` |
| `SnowMaxAdditionalDensity`            | Number | kg dm-3 | Maximum density increment above `NewSnowDensityMin`. Together, the two parameters define the maximum modeled snow density.                                                                                                           | `"SnowMaxAdditionalDensity": 0.25`           |
| `SnowMeltTemperature`                 | Number | °C      | Base temperature above which snowmelt can occur                                                                                                                                                                                      | `"SnowMeltTemperature": 0.31`                |
| `SnowPacking`                         | Number |         | Fractional increase used when calculating daily compaction of the existing snowpack                                                                                                                                                  | `"SnowPacking": 0.01`                        |
| `SnowRetentionCapacityMax`            | Number | kg kg-1 | Maximum fraction of snow water equivalent that the snowpack can retain as liquid water. Liquid water above the retention capacity is released from the snowpack.                                                                     | `"SnowRetentionCapacityMax": 0.17`           |
| `SnowRetentionCapacityMin`            | Number | kg kg-1 | Minimum fraction of snow water equivalent that the snowpack can retain as liquid water                                                                                                                                               | `"SnowRetentionCapacityMin": 0.05`           |
| `SurfaceRoughness`                    | Number | m       | Average amplitude of surface micro-elevations. It affects temporary surface-water storage and surface-runoff calculations.                                                                                                           | `"SurfaceRoughness": 0.02`                   |
| `TemperatureLimitForLiquidWater`      | Number | °C      | Lower temperature threshold for precipitation partitioning. At or below this temperature, incoming precipitation is entirely snow.                                                                                                   | `"TemperatureLimitForLiquidWater": -3`       |
| `XSACriticalSoilMoisture`             | Number |         | Calibration factor used by the THESEUS soil-evaporation reduction method when calculating critical soil moisture under crop cover                                                                                                    | `"XSACriticalSoilMoisture": 0.1`             |
| `type`                                | String |         | Declares that this JSON defines soil moisture parameters                                                                                                                                                                             | `"type": "UserSoilMoistureParameters"`       |

!!! note
    All parameters apply globally to the soil profile unless otherwise specified. These values are typically set in the `general/soilmoisture.json` file within the `monica-parameters` repository.

---

## Rain and snow partitioning

MONICA divides net precipitation into liquid and soil fractions using two temperature thresholds:

1. At or above `SnowAccumulationTresholdTemperature`, all precipitation is treated as rain.
2. At or below `TemperatureLimitForLiquidWater`, all precipitation is treated as snow.
3. Between the two thresholds, precipitation is divided linearly between rain and snow.

The resulting liquid and solid fractions are multiplied by `CorrectionRain` and `CorrectionSnow`, respectively.

---

## Example: `soil-moisture.json`

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