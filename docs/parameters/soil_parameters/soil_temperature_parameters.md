# Soil Temperature Parameters

This section provides an overview of the key-value pairs in the `SoilTemperatureModuleParameters` JSON file used by MONICA. These parameters define the thermal properties of soil components (air, humus, water, quartz) and control how soil temperature is simulated across layers.

---

## List of soil temperature parameters

| Parameter Name                  | Type    | Unit       | Description                                                                                          | Example                                               |
|---------------------------------|---------|------------|------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| `BaseTemperature`               | Float   | °C         | Base temperature used in the soil temperature model as a lower boundary condition                   | `"BaseTemperature": 9.5`                              |
| `InitialSurfaceTemperature`     | Float   | °C         | Initial temperature at the soil surface at the start of the simulation                              | `"InitialSurfaceTemperature": 10`                     |
| `DensityAir`                    | Float   | kg m-3     | Density of air used in heat capacity calculations                                                   | `"DensityAir": [1.25, "kg/m3"]`                       |
| `SpecificHeatCapacityAir`       | Float   | J kg-1 K-1 | Specific heat capacity of air at approximately 300 K                                                | `"SpecificHeatCapacityAir": [1005, "J/(kg*K)"]`       |
| `DensityHumus`                  | Float   | kg m-3     | Density of soil humus fraction used in heat capacity calculations                                   | `"DensityHumus": [1300, "kg/m3"]`                     |
| `SpecificHeatCapacityHumus`     | Float   | J kg-1 K-1 | Specific heat capacity of the humus fraction                                                        | `"SpecificHeatCapacityHumus": [1920, "J/(kg*K)"]`     |
| `DensityWater`                  | Float   | kg m-3     | Density of water used in heat capacity calculations                                                 | `"DensityWater": [1000, "kg/m3"]`                     |
| `SpecificHeatCapacityWater`     | Float   | J kg-1 K-1 | Specific heat capacity of water                                                                     | `"SpecificHeatCapacityWater": [4192, "J/(kg*K)"]`     |
| `QuartzRawDensity`              | Float   | kg m-3     | Density of quartz (mineral soil fraction) used in heat capacity calculations                        | `"QuartzRawDensity": [2650, "kg/m3"]`                 |
| `SpecificHeatCapacityQuartz`    | Float   | J kg-1 K-1 | Specific heat capacity of the quartz fraction                                                       | `"SpecificHeatCapacityQuartz": [750, "J/(kg*K)"]`     |
| `NTau`                          | Float   |            | Empirical parameter controlling the dampening of temperature fluctuations with soil depth           | `"NTau": 0.65`                                        |
| `SoilAlbedo`                    | Float   |            | Fraction of incoming solar radiation reflected by the soil surface (0 = fully absorbed, 1 = fully reflected) | `"SoilAlbedo": 0.7`                          |
| `SoilMoisture`                  | Float   | m3 m-3     | Default volumetric soil moisture used in temperature calculations when no dynamic value is available | `"SoilMoisture": 0.25`                               |
| `type`                          | String  |            | Declares that this JSON defines soil temperature module parameters                                  | `"type": "SoilTemperatureModuleParameters"`           |

!!! note
    Parameters provided as arrays (e.g., `[1.25, "kg/m3"]`) contain a value and a unit string. The unit string is for documentation purposes only and is not interpreted by the model.

---

## Example: `soiltemperature.json`

```json
{
    "BaseTemperature": 9.5,
    "InitialSurfaceTemperature": 10,

    "DensityAir": [1.25, "kg/m3"],
    "SpecificHeatCapacityAir": [1005, "J/(kg*K)"],

    "DensityHumus": [1300, "kg/m3"],
    "SpecificHeatCapacityHumus": [1920, "J/(kg*K)"],

    "DensityWater": [1000, "kg/m3"],
    "SpecificHeatCapacityWater": [4192, "J/(kg*K)"],

    "QuartzRawDensity": [2650, "kg/m3"],
    "SpecificHeatCapacityQuartz": [750, "J/(kg*K)"],

    "NTau": 0.65,
    "SoilAlbedo": 0.7,
    "SoilMoisture": 0.25,

    "type": "SoilTemperatureModuleParameters"
}
```