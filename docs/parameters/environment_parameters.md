# Environment Parameters

This section provides an overview of the key-value pairs in the `UserEnvironmentParameters` JSON file used by MONICA. These parameters define the environmental conditions of the simulation site, including atmospheric CO2 concentration, groundwater dynamics, albedo, and meteorological settings.

---

## List of environment parameters

| Parameter Name             | Type             | Unit   | Description                                                                                                                                                  | Example                              |
|----------------------------|------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| `Albedo`                   | Float            |        | Surface albedo of the site, representing the fraction of incoming solar radiation reflected by the surface (0 = fully absorbed, 1 = fully reflected)         | `"Albedo": 0.23`                     |
| `rcp`                      | String           |        | Representative Concentration Pathway scenario used to define future atmospheric CO2 trajectories (e.g. `rcp26`, `rcp45`, `rcp60`, `rcp85`)                  | `"rcp": "rcp85"`                     |
| `AtmosphericCO2`           | Float            | ppm    | Atmospheric CO2 concentration used for the entire simulation. If set to `0`, MONICA calculates CO2 automatically based on the selected RCP scenario          | `"AtmosphericCO2": 0`                |
| `AtmosphericCO2s`          | Object           | ppm    | Year-to-CO2-value pairs for specifying yearly CO2 concentrations. Overrides `AtmosphericCO2` for the years provided                                          | `"AtmosphericCO2s": {}`              |
| `LeachingDepth`            | Float            | m      | Soil depth at which nitrate leaching is calculated and reported                                                                                              | `"LeachingDepth": 1.6`               |
| `MaxGroundwaterDepth`      | Float            | m      | Maximum depth of the groundwater table during the year                                                                                                       | `"MaxGroundwaterDepth": 18`          |
| `MinGroundwaterDepth`      | Float            | m      | Minimum depth of the groundwater table during the year                                                                                                       | `"MinGroundwaterDepth": 20`          |
| `MinGroundwaterDepthMonth` | Integer          | month  | Month of the year (1–12) at which the groundwater table reaches its minimum depth                                                                            | `"MinGroundwaterDepthMonth": 3`      |
| `WindSpeedHeight`          | Float            | m      | Height above the ground at which wind speed measurements are taken, used for evapotranspiration calculations                                                 | `"WindSpeedHeight": 2`               |
| `timeStep`                 | Integer          | d      | Simulation time step in days. Typically set to `1` for daily simulations                                                                                     | `"timeStep": 1`                      |
| `type`                     | String           |        | Declares that this JSON defines environment parameters                                                                                                       | `"type": "UserEnvironmentParameters"` |

!!! note
    When `AtmosphericCO2` is set to `0`, MONICA uses the RCP scenario defined in `rcp` to automatically derive yearly CO2 concentrations. If more precise control is needed, yearly values can be specified directly using `AtmosphericCO2s` as key-value pairs (e.g. `{"2020": 412, "2021": 414}`). Values in `AtmosphericCO2s` take precedence over the RCP-derived values for the years specified.

---

## Example: `environment.json`

```json
{
    "Albedo": 0.23,
    "rcp": "rcp85",
    "AtmosphericCO2": 0,
    "AtmosphericCO2s": {},
    "LeachingDepth": 1.6,
    "MaxGroundwaterDepth": 18,
    "MinGroundwaterDepth": 20,
    "MinGroundwaterDepthMonth": 3,
    "WindSpeedHeight": 2,
    "timeStep": 1,
    "type": "UserEnvironmentParameters"
}
```