# Crop Parameters

This section provides an overview of the key-value pairs in the `UserCropParameters` JSON file used by MONICA. These parameters define global crop physiological settings that apply across all crops in the simulation, controlling processes such as respiration, nitrogen demand, light use, and stomatal conductance.

---

## List of crop parameters

| Parameter Name                   | Type    | Unit             | Description                                                                                                                          | Example                                        |
|----------------------------------|---------|------------------|--------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|
| `CanopyReflectionCoefficient`    | Float   |                  | Fraction of incoming radiation reflected by the crop canopy. Used in radiation interception calculations                            | `"CanopyReflectionCoefficient": 0.08`          |
| `GrowthRespirationParameter1`    | Float   |                  | First parameter controlling the rate of growth respiration, scaling carbon cost of new biomass production                           | `"GrowthRespirationParameter1": 0.1`           |
| `GrowthRespirationParameter2`    | Float   | °C               | Temperature parameter used in the growth respiration function                                                                       | `"GrowthRespirationParameter2": 38`            |
| `GrowthRespirationRedux`         | Float   |                  | Reduction factor applied to growth respiration to account for efficiency differences between crops                                  | `"GrowthRespirationRedux": 0.7`                |
| `MaintenanceRespirationParameter1` | Float | d-1              | First parameter controlling the rate of maintenance respiration, representing the baseline metabolic cost of existing biomass       | `"MaintenanceRespirationParameter1": 0.08`     |
| `MaintenanceRespirationParameter2` | Float | °C               | Temperature parameter used in the maintenance respiration function                                                                  | `"MaintenanceRespirationParameter2": 44`       |
| `MaxCropNDemand`                 | Float   | kg N ha-1 d-1    | Maximum daily nitrogen demand of the crop, setting an upper limit on nitrogen uptake from the soil                                  | `"MaxCropNDemand": 6`                          |
| `MinimumAvailableN`              | Float   | kg N m-3         | Minimum soil nitrogen concentration below which the crop cannot take up any further nitrogen                                        | `"MinimumAvailableN": 0.000075`                |
| `MinimumNConcentrationRoot`      | Float   | kg N kg DM-1     | Minimum nitrogen concentration in root biomass, below which root growth is limited                                                  | `"MinimumNConcentrationRoot": 0.005`           |
| `ReferenceAlbedo`                | Float   |                  | Reference surface albedo used to calculate the reference evapotranspiration for a standard grass surface                           | `"ReferenceAlbedo": 0.23`                      |
| `ReferenceLeafAreaIndex`         | Float   | m2 m-2           | Leaf area index of the reference grass surface used in evapotranspiration calculations                                              | `"ReferenceLeafAreaIndex": 1.44`               |
| `ReferenceMaxAssimilationRate`   | Float   | kg CO2 ha-1 h-1  | Maximum assimilation rate of the reference grass surface, used to scale crop-specific assimilation rates                           | `"ReferenceMaxAssimilationRate": 30`           |
| `SaturationBeta`                 | Float   |                  | Shape parameter controlling the saturation response of photosynthesis to light (light response curve curvature)                    | `"SaturationBeta": 2.5`                        |
| `StomataConductanceAlpha`        | Float   |                  | Scaling parameter for stomatal conductance, controlling the sensitivity of stomata to environmental conditions                     | `"StomataConductanceAlpha": 40`                |
| `Tortuosity`                     | Float   |                  | Tortuosity factor describing the path length of nitrogen diffusion through the soil to the root surface                            | `"Tortuosity": 0.002`                          |
| `type`                           | String  |                  | Declares that this JSON defines global crop parameters                                                                              | `"type": "UserCropParameters"`                 |

!!! note
    These are global crop parameters that apply to all crops in the simulation. They differ from species and cultivar parameters, which are crop-specific. The reference parameters (`ReferenceAlbedo`, `ReferenceLeafAreaIndex`, `ReferenceMaxAssimilationRate`) define a standard grass reference surface consistent with FAO-56 Penman-Monteith evapotranspiration calculations.

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