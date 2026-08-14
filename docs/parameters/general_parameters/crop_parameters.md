# Crop Module Parameters

Crop module parameters are shared settings used by MONICA's crop growth model. Crop-specific properties are documented separately under species and cultivar parameters.

---

## List of crop parameters

| Parameter Name                     | Type                         | Unit                    | Description                                                                                              | Example                                             |
|------------------------------------|------------------------------|-------------------------|----------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| `CanopyReflectionCoefficient`      | Float                        |                         | Fraction of incoming radiation reflected by the crop canopy                                              | `"CanopyReflectionCoefficient": 0.08`               |
| `GrowthRespirationParameter1`      | Float                        | °C-1                    | Temperature sensitivity of growth respiration                                                            | `"GrowthRespirationParameter1": 0.1`                |
| `GrowthRespirationParameter2`      | Float                        | °C                      | Reference temperature for growth respiration                                                             | `"GrowthRespirationParameter2": 38`                 |
| `MaintenanceRespirationParameter1` | Float                        | °C-1                    | Temperature sensitivity of maintenance respiration                                                       | `"MaintenanceRespirationParameter1": 0.08`          |
| `MaintenanceRespirationParameter2` | Float                        | °C                      | Reference temperature for the maintenance respiration                                                    | `"MaintenanceRespirationParameter2": 44`            |
| `MaxCropNDemand`                   | Float                        | kg N ha-1 d-1           | Maximum daily crop nitrogen demand                                                                       | `"MaxCropNDemand": 6`                               |
| `MinimumAvailableN`                | Float                        | kg N m-2 per soil layer | Amount of mineral nitrogen left in a soil layer after crop uptake                                        | `"MinimumAvailableN": 0.000075`                     |
| `MinimumNConcentrationRoot`        | Float                        | kg N kg-1 DM            | Minimum nitrogen concentration allowed in root biomass                                                   | `"MinimumNConcentrationRoot": 0.005`                |
| `ReferenceAlbedo`                  | Float                        |                         | Reference surface albedo used to calculate evapotranspiration                                            | `"ReferenceAlbedo": 0.23`                           |
| `ReferenceLeafAreaIndex`           | Float                        | m2 m-2                  | Leaf area index used for the reference canopy calculation                                                | `"ReferenceLeafAreaIndex": 1.44`                    |
| `ReferenceMaxAssimilationRate`     | Float                        | kg CO2 ha-1 h-1         | Maximum assimilation rate of the reference canopy                                                        | `"ReferenceMaxAssimilationRate": 30`                |
| `SaturationBeta`                   | Float                        | kPa                     | Controls the effect of vapor pressure deficit on stomatal resistance                                     | `"SaturationBeta": 2.5`                             |
| `StomataConductanceAlpha`          | Float                        |                         | Relates reference photosynthesis to stomatal conductance                                                 | `"StomataConductanceAlpha": 40`                     |
| `Tortuosity`                       | Float                        |                         | Multiplier used when calculating soil nitrogen diffusion towards roots                                   | `"Tortuosity": 0.002`                               |
| `AdjustRootDepthForSoilProps`      | Boolean                      |                         | Adjusts maximum rooting depth for soil texture and bulk density                                          | `"AdjustRootDepthForSoilProps": true`               |
| `TimeUnderAnoxiaThreshold`         | Integer or array of integers | d                       | Stage-specific time scale used in the oxygen-deficiency response. A single value applies to every stage. | `"TimeUnderAnoxiaThreshold": [4, 4, 4, 4, 4, 4, 4]` |
| `type`                             | String                       |                         | Declares that this JSON defines global crop parameters                                                   | `"type": "UserCropParameters"`                      |

!!! warning "Legacy parameter"

    `GrowthRespirationRedux` is accepted for compatibility with older parameter files but is not used by the current crop growth calculation.

---

## Example: `crop.json`

```json
{
    "CanopyReflectionCoefficient": 0.08,
    "GrowthRespirationParameter1": 0.1,
    "GrowthRespirationParameter2": 38,
    "GrowthRespirationRedux": 0.7,
    "MaintenanceRespirationParameter1": 0.08,
    "MaintenanceRespirationParameter2": 44,
    "MaxCropNDemand": 6,
    "MinimumAvailableN": 0.000075,
    "MinimumNConcentrationRoot": 0.005,
    "ReferenceAlbedo": 0.23,
    "ReferenceLeafAreaIndex": 1.44,
    "ReferenceMaxAssimilationRate": 30,
    "SaturationBeta": 2.5,
    "StomataConductanceAlpha": 40,
    "Tortuosity": 0.002,
    "type": "UserCropParameters"
}
```