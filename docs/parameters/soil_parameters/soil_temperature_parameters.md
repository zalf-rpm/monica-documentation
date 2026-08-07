# Soil Temperature Parameters

This section describes the parameters accepted by MONICA's soil-temperature module. They control the initialization of the soil-temperature profile and the calculation of soil thermal conductivity and volumetric heat capacity.

---

## List of soil temperature parameters

| Parameter Name               | Type   | Unit       | Description                                                                                                                                                                                                                                                     | Example                                   |
|------------------------------|--------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| `BaseTemperature`            | Number | °C         | Temperature used to initialize the deepest model layer. Together with `InitialSurfaceTemperature`, it defines the initial temperature profile through the soil.                                                                                                 | `"BaseTemperature": 9.5`                  |
| `InitialSurfaceTemperature`  | Number | °C         | Initial soil-surface temperature. The model interpolates between this value and `BaseTemperature` to initialize the temperature of the soil layers.                                                                                                             | `"InitialSurfaceTemperature": 10`         |
| `DensityAir`                 | Number | kg m-3     | Air density used when calculating the volumetric heat capacity of air-filled pore space                                                                                                                                                                         | `"DensityAir": 1.25`                      |
| `SpecificHeatCapacityAir`    | Number | J kg-1 K-1 | Specific heat capacity of air used in the soil heat-capacity calculation                                                                                                                                                                                        | `"SpecificHeatCapacityAir": 1005`         |
| `DensityHumus`               | Number | kg m-3     | Density assigned to the soil organic matter fraction for the heat-capacity calculation                                                                                                                                                                          | `"DensityHumus": 1300`                    |
| `SpecificHeatCapacityHumus`  | Number | J kg-1 K-1 | Specific heat capacity assigned to the soil organic matter fraction                                                                                                                                                                                             | `"SpecificHeatCapacityHumus": 1920`       |
| `DensityWater`               | Number | kg m-3     | Density of water used in heat capacity calculations                                                                                                                                                                                                             | `"DensityWater": 1000`                    |
| `SpecificHeatCapacityWater`  | Number | J kg-1 K-1 | Specific heat capacity of water                                                                                                                                                                                                                                 | `"SpecificHeatCapacityWater": 4192`       |
| `QuartzRawDensity`           | Number | kg m-3     | Density assigned to the mineral soil fraction in the heat-capacity calculation                                                                                                                                                                                  | `"QuartzRawDensity": 2650`                |
| `SpecificHeatCapacityQuartz` | Number | J kg-1 K-1 | Specific heat capacity assigned to the mineral soil fraction                                                                                                                                                                                                    | `"SpecificHeatCapacityQuartz": 750`       |
| `NTau`                       | Number |            | Empirical scaling factor applied to the effective volume and heat storage of the temperature layers. It influences how quickly soil temperatures respond to surface-temperature changes.                                                                        | `"NTau": 0.65`                            |
| `SoilAlbedo`                 | Number |            | Nominal fraction of incoming solar radiation reflected by the soil surface. A value of `0` represents complete absorption and `1` complete reflection. *This parameter is parsed and serialized but is not currently used by the soil-temperature calculation.* | `"SoilAlbedo": 0.7`                       |
| `SoilMoisture`               | Number | m3 m-3     | Constant volumetric soil-water content used when calculating thermal conductivity and heat capacity. The soil-temperature module does not currently use the dynamically simulated water content of individual soil layers for these calculations.               | `"SoilMoisture": 0.25`                    |
| `type`                       | String |            | Declares that this JSON defines soil temperature module parameters                                                                                                                                                                                              | `"type": "UserSoilTemperatureParameters"` |

!!! note
    Parameters provided as arrays (e.g., `[1.25, "kg/m3"]`) contain a value and a unit string. The unit string is for documentation purposes only and is not interpreted by the model.

---

## Example: `soil-temperature.json`

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