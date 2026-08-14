# Environment Parameters

The `EnvironmentParameters` JSON object defines environmental conditions used by MONICA, including atmospheric gas concentrations, groundwater dynamics, surface albedo, wind measurement height, nitrate leaching depth, and the numerical timestep factor used by several soil process modules.

In a MONICA site configuration, these values normally appear under the `EnvironmentParameters` property.

---

## List of environment parameters

| Parameter Name             | Type             | Unit            | Description                                                                                                                                                                                                                                                                             | Example                           |
|----------------------------|------------------|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| `Albedo`                   | Number           |                 | Fraction of incoming shortwave radiation reflected by the surface. The physically meaningful range is `0` (fully absorbed) to `1` (fully reflected).                                                                                                                                    | `"Albedo": 0.23`                  |
| `rcp`                      | String or Number |                 | Representative Concentration Pathway used to estimate atmospheric CO<sub>2</sub> when neither climate data, a year-specific value, nor a positive fixed concentration is available. Supported scenarios are `rcp19`, `rcp26`, `rcp34`, `rcp45`, `rcp60`, `rcp70`, and `rcp85`.          | `"rcp": "rcp85"`                  |
| `AtmosphericCO2`           | Number           | ppm             | Fixed atmospheric CO<sub>2</sub> concentration. Used when climate data contain no CO<sub>2</sub> value and `AtmosphericCO2s` contains no entry for the current year. Set to `0` to enable RCP-based calculation.                                                                        | `"AtmosphericCO2": 0`             |
| `AtmosphericCO2s`          | Object           | ppm             | Mapping from calendar years to atmospheric CO<sub>2</sub> concentrations. Year-specific values take precedence over `AtmosphericCO2` and the RCP calculation, but not over CO<sub>2</sub> supplied by climate data.                                                                     | `"AtmosphericCO2s": {}`           |
| `AtmosphericO3`            | Number           | ppb             | Fixed atmospheric ozone concentration. Used when climate data contain no ozone value and `AtmosphericO3s` contain no entry for the current year.                                                                                                                                        | `"AtmosphericO3": 0`              |
| `AtmosphericO3s`           | Object           | ppb             | Mapping from calendar years to atmospheric ozone concentrations. Year-specific values take precedence over `AtmosphericO3`, but not over ozone supplied by climate data.                                                                                                                | `"AtmosphericO3s": {}`            |
| `LeachingDepth`            | Number           | m               | Soil depth used to evaluate nitrate leaching and the associated lower-boundary water flux. It should be consistent with the depth and layer structure of the simulated soil profile.                                                                                                    | `"LeachingDepth": 1.6`            |
| `MaxGroundwaterDepth`      | Number           | m below surface | Maximum annual groundwater depth below the soil surface. This is the deepest position of the groundwater table and should normally be greater than or equal to `MinGroundwaterDepth`.                                                                                                   | `"MaxGroundwaterDepth": 1.0`      |
| `MinGroundwaterDepth`      | Number           | m below surface | Minimum annual groundwater depth below the soil surface. This is the shallowest position of the groundwater table and is reached approximately during `MinGroundwaterDepthMonth`.                                                                                                       | `"MinGroundwaterDepth": 0.5`      |
| `MinGroundwaterDepthMonth` | Integer          | month           | Month during which the seasonal groundwater curve approximately reaches `MinGroundwaterDepth`. Use values from `1` to `12`.                                                                                                                                                             | `"MinGroundwaterDepthMonth": 3`   |
| `WindSpeedHeight`          | Number           | m above ground  | Height at which the input wind speed was measured. MONICA uses this value when converting wind speed for evapotranspiration calculations.                                                                                                                                               | `"WindSpeedHeight": 2`            |
| `timeStep`                 | Number           | d               | Numerical time-step factor used by several soil processes calculations, including soil temperature, soil moisture and frost, and nitrate transport. Use `1` with standard daily climate data. *Note: This parameter does not change the calendar interval of the main simulation loop.* | `"timeStep": 1`                   |
| `type`                     | String           |                 | Declares that this JSON defines environment parameters                                                                                                                                                                                                                                  | `"type": "EnvironmentParameters"` |

---

## Atmospheric CO<sub>2</sub> selection

For every simulation date, MONICA selects atmospheric CO<sub>2</sub> using the following precedence:

1. CO<sub>2</sub> supplied directly by the climate data.
2. The value in `AtmosphericCO2s` for the current calendar year.
3. An RCP-derived concentration when the fixed `AtmosphericCO2` value does not enable a fixed concentration.
4. The fixed value in `AtmosphericCO2`.

For normal configurations, set `AtmosphericCO2` to:

- a realistic positive concentration, such as `400`, to use a fixed value,
- or `0` to enable RCP-based calculation.

---

### Example

```json
{
    "rcp": "rcp45",
    "AtmosphericCO2": 0,
    "AtmosphericCO2s": {
        "2020": 412,
        "2021": 414
    }
}
```

In this example:

- Climate data CO<sub>2</sub> takes precedence whenever it is present.
- `412 ppm` is used in 2020 when climate data CO<sub>2</sub> is absent.
- `414 ppm` is used in 2021 when climate data CO<sub>2</sub> is absent.
- For other years, MONICA calculates CO<sub>2</sub> from the `rcp45` trajectory.

---

## Atmospheric ozone selection

MONICA selects atmospheric ozone using the following precedence:

1. Ozone supplied directly by the climate data.
2. The value in `AtmosphericO3s` for the current calendar year.
3. The fixed value in `AtmosphericO3`.

Unlike CO<sub>2</sub>, atmospheric ozone is not calculated from the selected RCP scenario.

---

## Groundwater dynamics

For each simulation date, MONICA first checks whether measured groundwater information is available for that exact date. Measured groundwater information is configured separately from the `EnvironmentParameters` object. When a measured value is available, MONICA uses it. Otherwise, it calculates groundwater depth from an annual sinusoidal curve defined by:

- `MaxGroundwaterDepth`
- `MinGroundwaterDepth`
- `MinGroundwaterDepthMonth`

`MinGroundwaterDepthMonth` specifies approximately when the curve reaches `MinGroundwaterDepth`, the shallowest groundwater position. The curve reaches `MaxGroundwaterDepth`, the deepest position, approximately six months later. `MaxGroundwaterDepth` should therefore normally be greater than or equal to `MinGroundwaterDepth`.

!!! note "Groundwater depth terminology"

    Groundwater depth is measured downward from the soil surface. A larger numerical value therefore represents a deeper groundwater table.

    For example:

    ```json
    {
        "MaxGroundwaterDepth": 1.0,
        "MinGroundwaterDepth": 0.5,
        "MinGroundwaterDepthMonth": 3
    }
    ```

    The calculated groundwater depth is approximately `0.5 m` in March and approximately `1.0 m` six months later.

---

## Example: `environment.json`

```json
{
    "Albedo": 0.23,
    "rcp": "rcp85",
    "AtmosphericCO2": 0,
    "AtmosphericCO2s": {},
    "LeachingDepth": 1.6,
    "MaxGroundwaterDepth": 1.0,
    "MinGroundwaterDepth": 0.5,
    "MinGroundwaterDepthMonth": 3,
    "WindSpeedHeight": 2,
    "timeStep": 1,
    "type": "EnvironmentParameters"
}
```