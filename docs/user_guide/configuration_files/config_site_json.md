# The `site.json` configuration file

**`site.json`** holds all input data and parameters that can be considered site-specific. Besides the key `SiteParameters` in the top-level JSON object, there might be a few JSON objects that set global or general soil and environment-specific parameters. These can either be included from the SQLite database (table *user_parameter*), from the filesystem, or defined directly in `site.json`. To facilitate easy overwriting of the standard parameters, they are included via the `DEFAULT` parameter pseudo-key.

---

## The soil profile

The key `SoilProfileParameters` contains a **JSON array** of **JSON objects** that describe the layers of the soil profile. MONICA internally uses **20** layers, each with a **10 cm** thickness. `SoilProfileParameters` must contain at least one layer with a minimal set of soil properties (`KA5TextureClass`, `SoilOrganicCarbon`/`SoilOrganicMatter`, and `SoilRawDensity`/`SoilBulkDensity`). Additionally, each layer needs a `Thickness` key describing the height of the soil layer. The specified soil profile will be internally normalized to **20** layers of **10 cm** each. If the total thickness of the layers does not add up to **2 m** (MONICA's used profile depth), the last layer will be extended up to **2 m**. Likewise, layers extending beyond **2 m** will be cut at that depth.

Not all properties are required, but the more soil properties are specified, the more accurate the calculations will be. If only the `KA5TextureClass` is given, average values for `Sand` and `Clay` content will be used, and `FieldCapacity`, `PermanentWiltingPoint`, `PoreVolume`, and `Lambda` will be calculated based on them.

Only one of `SoilRawDensity` and `SoilBulkDensity`, as well as one of `SoilOrganicCarbon` and `SoilOrganicMatter`, must be provided. If both values are being supplied, they should match.

The following table shows the names of the soil properties that can be set for an individual layer.

| Name of config file variable   | Unit                          | Description                                         |
|--------------------------------|-------------------------------|-----------------------------------------------------|
| **Thickness**                  | m                             | Thickness of the soil layer                         |
| **Sand**                       | kg kg-1 (fraction [0-1])      | Soil sand content, a fraction between 0 and 1       |
| **Clay**                       | kg kg-1 (fraction [0-1])      | Soil clay content, a fraction between 0 and 1       |
| **pH**                         |                               | Soil pH value                                       |
| **Sceleton**                   | fraction [0-1]                | Soil stone content, a fraction between 0 and 1      |
| **Lambda**                     |                               | Soil water conductivity coefficient                 |
| **FieldCapacity**              | m3 m-3 (fraction [0-1])       | Field capacity                                      |
| **PoreVolume**                 | m3 m-3 (fraction [0-1])       | Saturation                                          |
| **PermanentWiltingPoint**      | m3 m-3 (fraction [0-1])       | Permanent wilting point                             |
| **KA5TextureClass**            |                               | KA5 soil texture class                              |
| **SoilAmmonium**               | kg NH4-N m-3                  | Initial soil ammonium content                       |
| **SoilNitrate**                | kg NO3-N m-3                  | Initial soil nitrate content                        |
| **CN**                         |                               | Soil C/N ratio                                      |
| **SoilRawDensity**             | kg m-3                        | Soil raw density                                    |
| **SoilBulkDensity**            | kg m-3                        | Soil bulk density                                   |
| **SoilOrganicCarbon**          | % [0-100] ([kg C kg-1] * 100) | Soil organic carbon, a percentage between 0 and 100 |
| **SoilOrganicMatter**          | kg OM kg-1 (fraction [0-1])   | Soil organic matter                                 |
| **SoilMoisturePercentFC**      | % [0-100]                     | Initial soil moisture in percent of field capacity  |

Further site-specific variables:

| Name of config file variable   | Unit                            | Description                   |
|--------------------------------|---------------------------------|-------------------------------|
| **Latitude**                   | [decimal degrees]               | Site's latitude               |
| **Slope**                      | [height m * length m-1]   [0-1] | Site's slope                  |
| **HeightNN**                   | [m]                             | Site's height above sea level |
| **NDeposition**                |                                 | N deposition via atmosphere   |

---

## Environment parameters for the whole simulation

The following environment parameters for the entire simulation can be adjusted by referencing either the appropriate JSON file or selectively overwriting parameters read from the default file. 

### Atmospheric CO<sub>2</sub> (O<sub>3</sub>) handling logic

Currently, the following procedure is applied:

1. Check if **co2** or **o3** field is present in the daily climate data. If so, use the supplied values.
2. If the parameter `AtmosphericCO2s` (`AtmosphericO3s`) is set to a JSON object mapping of "year" to "CO2 value", use these yearly values.
3. If the parameter `AtmosphericCO2` (`AtmosphericO3`) is set to 0 or a negative value, let **MONICA** calculate the CO<sub>2</sub> concentration depending on the year.
4. If none of the above apply, use the value of `AtmosphericCO2` (`AtmosphericO3`) for the entire simulation.

### Environment parameter table

| Name of parameter              | Unit  | Default value  | Description                                                                            |
|--------------------------------|-------|----------------|----------------------------------------------------------------------------------------|
| **Albedo**                     |       | 0.23           | Surface reflectivity coefficient                                                       |
| **AtmosphericCO2**             | ppm   | 0.0            | Atmospheric CO<sub>2</sub> concentration                                               |
| **AtmosphericCO2s**            | ppm   | unset          | Example value: `{"1991": 360, "1992": 370, "1993": 380}`                               |
| **AtmosphericO3**,             | ppm   | 0.0            | Atmospheric O<sub>3</sub> concentration                                                |
| **AtmosphericO3s**             | ppm   | unset          |                                                                                        |
| **WindSpeedHeight**            | m     | 2.0            | Height above ground surface at which wind speed is measured                            |
| **LeachingDepth**              | m     | 0.0            | Depth below ground surface at which water and nitrate outflow is determined            |
| **MaxGroundwaterDepth**        | m     | 18.0           | Maximum annual groundwater depth below the ground surface                              |
| **MinGroundwaterDepth**        | m     | 20.0           | Minimum annual groundwater depth below the ground surface                              |
| **MinGroundwaterDepthMonth**   |       | 3.0            | Month in which the minimum average groundwater depth below ground surface is observed  |

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