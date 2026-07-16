# The `site.json` configuration file

**`site.json`** holds all input data and parameters that can be considered site-specific. Besides the key `SiteParameters` in the top-level JSON object, there might be a few JSON objects that set global or general soil and environment-specific parameters. These can either be included from the SQLite database (table *user_parameter*), from the filesystem, or defined directly in `site.json`. To facilitate easy overwriting of the standard parameters, they are included via the `DEFAULT` parameter pseudo-key.

---

## The soil profile

The key `SoilProfileParameters` contains a **JSON array** of **JSON objects** describing the layers of the soil profile. 

### Internal normalization

MONICA internally represents the soil profile as **20** layers, each with a thickness of **10 cm**, resulting in a total profile depth of **2 m**. 

* Each user-defined layer must specify a `Thickness`.
* `Thickness` can be given as a simple number (default unit: **m**) or as an array containing a value and unit, for example `[30, "cm"]`.
* The user-defined profile is automatically normalized to the internal 20-layer representation.
* If the specified profile is **shallower than 2 m**, the last layer is extended until a depth of **2 m** is reached.
* If the specified profile is **deeper than 2 m**, all layers below **2 m** are ignored.

### Required and optional properties

At least **one soil layer** must be defined. Every layer requires a `Thickness` value.

To characterize the soil, either `KA5TextureClass` or both `Sand` and `Clay` should be provided.

Likewise, only one value from each of the following pairs is typically required:

* `SoilRawDensity` or `SoilBulkDensity`
* `SoilOrganicCarbon` or `SoilOrganicMatter`

If both values of a pair are supplied, they should be consistent.

Providing additional soil properties explicitly generally improves the accuracy of the simulated hydraulic characteristics by reducing the number of internally estimated parameters.

### Automatic parameter calculation

If properties are omitted, MONICA derives them whenever possible:

| Property                                               | Behavior if omitted                                                                          |
|--------------------------------------------------------|----------------------------------------------------------------------------------------------|
| `Sand`, `Clay`                                         | Derived from `KA5TextureClass` if available                                                  |
| `Silt`                                                 | Always calculated internally as `1.0 - Sand - Clay`                                          |
| `FieldCapacity`, `PermanentWiltingPoint`, `PoreVolume` | Estimated using the configured pedotransfer functions                                        |
| `Lambda`                                               | Calculated from `Sand` and `Clay` content                                                    |
| `SoilBulkDensity`/`SoilRawDensity`                     | Derived from the other density value and clay content                                        |
| `SoilOrganicCarbon`/`SoilOrganicMatter`                | Derived from the other organic matter/carbon value using MONICA's internal conversion factor |

### Soil Properties

The following table lists the soil properties that can be specified for each soil layer.

| Name of config file variable   | Unit                          | Description                                           | Default  | Example                                                                       | Note                                                                                      |
|--------------------------------|-------------------------------|-------------------------------------------------------|----------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Thickness**                  | m (default), cm, mm           | Thickness of the soil layer                           |          | `"Thickness": 0.3` or `"Thickness": [30, "cm"]` or `"Thickness": [300, "mm"]` |                                                                                           |
| **Sand**                       | kg kg-1 (fraction [0-1])      | Soil sand content                                     |          | `"Sand": 0.45` or `"Sand": [45, "%"]`                                         | If `%` is specified, `Sand` is internally converted into fraction [0-1].                  |
| **Clay**                       | kg kg-1 (fraction [0-1])      | Soil clay content                                     |          | `"Clay": 0.12` or `"Clay": [12, "%"]`                                         | If `%` is specified, `Clay` is internally converted into fraction [0-1].                  |
| **pH**                         |                               | Soil pH value                                         | `6.9`    | `"pH": 7.2`                                                                   |                                                                                           |
| **Sceleton**                   | fraction [0-1]                | Stone content                                         | `0.0`    | `"Sceleton": 0.05` or `"Sceleton": [5, "%"]`                                  | Values are internally limited to **0.8**                                                  |
| **Lambda**                     |                               | Soil water conductivity coefficient                   |          | `"Lambda": 0.5`                                                               |                                                                                           |
| **FieldCapacity**              | m3 m-3 (fraction [0-1])       | Volumetric water content at field capacity            |          | `"FieldCapacity": 0.25` or `"FieldCapacity": [25, "%"]`                       | If `%` is specified, `FieldCapacity` is internally converted into fraction [0-1].         |
| **PoreVolume**                 | m3 m-3 (fraction [0-1])       | Saturated volumetric water content                    |          | `"PoreVolume": 0.45` or `"PoreVolume": [45, "%"]`                             | If `%` is specified, `PoreVolume` is internally converted into fraction [0-1].            |
| **PermanentWiltingPoint**      | m3 m-3 (fraction [0-1])       | Volumetric water content at permanent wilting point   |          | `"PermanentWiltingPoint": 0.10` or `"PermanentWiltingPoint": [10, "%"]`       | If `%` is specified, `PermanentWiltingPoint` is internally converted into fraction [0-1]. |
| **KA5TextureClass**            |                               | German KA5 soil texture class                         |          | `"KA5TextureClass": "Sl2"`                                                    |                                                                                           |
| **SoilAmmonium**               | kg NH4-N m-3                  | Initial soil ammonium concentration                   | `0.0005` | `"SoilAmmonium": 0.001`                                                       |                                                                                           |
| **SoilNitrate**                | kg NO3-N m-3                  | Initial soil nitrate concentration                    | `0.005`  | `"SoilNitrate": 0.02`                                                         |                                                                                           |
| **CN**                         |                               | Soil C/N ratio                                        | `10.0`   | `"CN": 9.5`                                                                   |                                                                                           |
| **SoilRawDensity**             | kg m-3                        | Soil raw density                                      |          | `"SoilRawDensity": 1450`                                                      |                                                                                           |
| **SoilBulkDensity**            | kg m-3                        | Soil bulk density                                     |          | `"SoilBulkDensity": 1300`                                                     |                                                                                           |
| **SoilOrganicCarbon**          | % [0-100] ([kg C kg-1] * 100) | Soil organic carbon                                   |          | `"SoilOrganicCarbon": 0.8` or `"SoilOrganicCarbon": [0.8, "%"]`               | SOC is internally converted into fraction [0-1].                                          |
| **SoilOrganicMatter**          | kg OM kg-1 (fraction [0-1])   | Soil organic matter                                   |          | `"SoilOrganicMatter": 0.015` or `"SoilOrganicMatter": [1.5, "%"]`             | If `%` is specified, `SoilOrganicMatter` is internally converted into fraction [0-1].     |
| **SoilMoisturePercentFC**      | % [0-100]                     | Initial soil moisture as percentage of field capacity | `100.0`  | `"SoilMoisturePercentFC": 80.0`                                               |                                                                                           |

### Recommended minimal soil layer

A typical soil layer only needs a few parameters. MONICA derives the remaining hydraulic properties automatically.

```json
{
  "Thickness": [0.3, "m"],
  "KA5TextureClass": "Sl2",
  "SoilOrganicCarbon": [0.8, "%"],
  "SoilBulkDensity": 1350,
  "SoilMoisturePercentFC": 100
}
```

---

## Further site-specific variables

| Name of config file variable   | Unit              | Description                          |
|--------------------------------|-------------------|--------------------------------------|
| **Latitude**                   | [decimal degrees] | Site's latitude                      |
| **Slope**                      | [m m-1] [0-1]     | Site's slope (height m * length m-1) |
| **HeightNN**                   | [m]               | Site's height above sea level        |
| **NDeposition**                | [kg N ha-1 y-1]   | Annual N deposition via atmosphere   |

---

## Environment parameters for the whole simulation

The following environment parameters for the entire simulation can be adjusted by referencing either the appropriate JSON file or selectively overwriting parameters read from the default file. 

### Atmospheric CO<sub>2</sub> (O<sub>3</sub>) handling logic

The atmospheric CO<sub>2</sub> and O<sub>3</sub> concentrations in MONICA are determined using the following priority order:

1. **Daily climate data**

    If the field `co2` or `o3` is present in the daily climate input, this value is used directly.

2. **Yearly concentration mapping**
    
    If no daily value is available and `AtmosphericCO2s` or `AtmosphericO3s` is provided as a JSON object mapping years to concentration values, the value corresponding to the simulation year is used.

3. **Dynamic calculation by MONICA**

    If no daily or yearly value is available, and if the parameter `AtmosphericCO2` is set to **0 or a negative value**, MONICA internally calculates the CO<sub>2</sub> concentration based on the simulation date and the selected `rcp` scenario. *Note: There is currently no internal dynamic calculation for O<sub>3</sub>. If no values are provided and `AtmosphericO3 <= 0`, the concentration defaults to 0.*

4. **Constant concentration**

    If none of the above conditions apply, the constant value defined in `AtmosphericCO2` or `AtmosphericO3` is used for the entire simulation period.

### Environment parameter table

| Name of parameter            | Unit | Default value | Description                                                                              | Example                                                      | Note                              |
|------------------------------|------|---------------|------------------------------------------------------------------------------------------|--------------------------------------------------------------|-----------------------------------|
| **Albedo**                   |      | 0.23          | Surface reflectivity coefficient                                                         | `"Albedo": 0.23`                                             |                                   |
| **AtmosphericCO2**           | ppm  | 0.0           | Atmospheric CO<sub>2</sub> concentration                                                 | `"AtmosphericCO2": 420`                                      | Internal calculation used if <= 0 |
| **AtmosphericCO2s**          | ppm  | unset         | Yearly CO<sub>2</sub> values                                                             | `"AtmosphericCO2s": {"1991": 360, "1992": 370, "1993": 380}` |                                   |
| **AtmosphericO3**            | ppm  | 0.0           | Atmospheric O<sub>3</sub> concentration                                                  | `"AtmosphericO3": 0.04`                                      |                                   |
| **AtmosphericO3s**           | ppm  | unset         | Yearly O<sub>3</sub> values                                                              | `"AtmosphericO3s": {"1991": 0.035, "1992": 0.036}`           |                                   |
| **WindSpeedHeight**          | m    | 2.0           | Height above ground surface at which wind speed is measured                              | `"WindSpeedHeight": 2.5`                                     |                                   |
| **LeachingDepth**            | m    | 0.0           | Depth below ground surface at which water and nitrate outflow is determined              | `"LeachingDepth": 2.0`                                       |                                   |
| **MaxGroundwaterDepth**      | m    | 18.0          | Maximum annual groundwater depth below the ground surface                                | `"MaxGroundwaterDepth": 1.0`                                 |                                   |
| **MinGroundwaterDepth**      | m    | 20.0          | Minimum annual groundwater depth below the ground surface                                | `"MinGroundwaterDepth": 0.5`                                 |                                   |
| **MinGroundwaterDepthMonth** |      | 3             | Month (1-12) in which the minimum average groundwater depth is observed                  | `"MinGroundwaterDepthMonth": 3`                              |                                   |
| **rcp**                      |      | rcp85         | Climate scenario used for internal CO<sub>2</sub> calculation when `AtmosphericCO2` <= 0 | `"rcp": "rcp45"`                                             |                                   |

!!! note
    The `rcp` parameter is only used when CO<sub>2</sub> is calculated internally (i.e., `AtmosphericCO2` <= 0 and no climate or yearly values are provided).
    
    Supported scenarios depend on the MONICA version:

    | MONICA version | Supported scenarios |
    |----------------|---------------------|
    | `< 3.6.54` | `rcp26`, `rcp45`, `rcp60`, `rcp85` |
    | `>= 3.6.54` | `rcp19`, `rcp26`, `rcp34`, `rcp45`, `rcp60`, `rcp70`, `rcp85` |

    **SSP Mapping**

    If your climate data is based on SSP scenarios, use the corresponding `rcp` value.

    The following mapping can be used to select the appropriate `rcp` value:

    | SSP scenario | MONICA `rcp` value |
    |--------------|--------------------|
    | `ssp119`     | `rcp19`            |
    | `ssp126`     | `rcp26`            |
    | `ssp245`     | `rcp45`            |
    | `ssp370`     | `rcp70`            |
    | `ssp460`     | `rcp60`            |
    | `ssp585`     | `rcp85`            |

---

## Example **site.json** file

The following example shows a typical `site.json` configuration defining key site parameters such as latitude, slope, and soil profile properties. It also demonstrates how to include external parameter files for soil, environment, and transport processes using the `include-from-file` directive.

```json
{
  "SiteParameters": {
    "Latitude": 52.80939865112305,
    "Slope": 0.1,
    "HeightNN": [0 , "m"],
    "NDeposition": 30,
    "SoilProfileParameters": [
      {
        "Thickness": 0.3,
        "SoilOrganicCarbon": [0.8, "%"],
        "KA5TextureClass": "Sl2",
        "SoilRawDensity": ["ld_eff2trd", 2, ["KA5TextureClass2clay", "Sl2"]],
        "Lambda": ["sandAndClay2lambda", ["KA5TextureClass2sand", "Sl2"], ["KA5TextureClass2clay", "Sl2"]]
      },
      {
        "Thickness": 0.1,
        "SoilOrganicCarbon": [0.15, "%"],
        "KA5TextureClass": "Sl2",
        "SoilRawDensity": ["ld_eff2trd", 2, ["KA5TextureClass2clay", "Sl2"]]
      },
      {
        "Thickness": 1.6,
        "SoilOrganicCarbon": [0.05, "%"],
        "KA5TextureClass": "Sl2",
        "SoilRawDensity": ["ld_eff2trd", 2, ["KA5TextureClass2clay", "Sl2"]]
      }
    ]
  },
  "SoilTemperatureParameters": ["include-from-file", "../monica-parameters/user-parameters/hermes-soil-temperature.json"],
  "EnvironmentParameters": {
    "DEFAULT": ["include-from-file", "../monica-parameters/user-parameters/hermes-environment.json"],
    "LeachingDepth": 2.0,
    "WindSpeedHeight": 2.5
  },
  "SoilOrganicParameters": ["include-from-file", "../monica-parameters/user-parameters/hermes-soil-organic.json"],
  "SoilTransportParameters": ["include-from-file", "../monica-parameters/user-parameters/hermes-soil-transport.json"],
  "SoilMoistureParameters": ["include-from-file", "../monica-parameters/user-parameters/hermes-soil-moisture.json"]
}
```