# The `site.json` configuration file

The **`site.json`** configuration file contains parameters describing the simulation site, including:

- geographic and topographic properties
- the soil profile
- initial soil conditions
- general environmental parameters
- references to soil process parameter sets

Site-specific parameters are defined under the top-level `SiteParameters` key. General parameter sets, such as `EnvironmentParameters` or `SoilMoistureParameters`, can be:

- included from the MONICA parameter database
- included from external JSON files
- defined directly in `site.json`
- included and selectively overriden

The `DEFAULT` pseudo-key can be used to load a standard parameter object before applying locally defined overrides.

---

## The soil profile

The `SoilProfileParameters` key contains an array of objects describing the soil horizons supplied by the user.

```json
{
  "SiteParameters": {
    "SoilProfileParameters": [
      {
        "Thickness": 0.3,
        "KA5TextureClass": "Sl2"
      }
    ]
  }
}
```

At least one soil profile entry should be provided.

### Internal layer representation

By default, MONICA represents the soil profile using:

- `NumberOfLayers`: 20
- `LayerThickness`: `0.1` m
- Total profile depth: `2.0` m

Both `NumberOfLayers` and `LayerThickness` can be changed in `SiteParameters`. Consequently, 20 layers of 10 cm are the default representation, not an invariant internal representation.

```json
{
  "SiteParameters": {
    "NumberOfLayers": 30,
    "LayerThickness": 0.05
  }
}
```

The resulting total profile depth is:

```
NumberOfLayers * LayerThickness
```

### Converting user-defined horizons into internal layers

MONICA converts the user-defined soil horizons into equal-sized internal layers.

For every entry except the final one:

1. Its `Thickness` is converted to meters.
2. The thickness is divided by `LayerThickness`.
3. The result is rounded to an integer number of internal layers.
4. The soil properties are copied into that number of layers.

The final soil profile entry is repeated until the configured internal profile is full. Its `Thickness` therefore does not limit its final extent.

If earlier entries already fill the internal profile, subsequent entries are ignored.

If `Thickness` is omitted from a non-final entry, the configured `LayerThickness` is used.

Nevertheless, specifying `Thickness` explicitly is strongly recommended because it makes the intended horizon boundaries clear. `Thickness` can be supplied as:

- a number in meters
- a value-unit array using `m`, `dm`, `cm`, or `mm`

For example:

```json
"Thickness": 0.3
```

```json
"Thickness": [30, "cm"]
```

```json
"Thickness": [300, "mm"]
```

Because horizon thicknesses are rounded to complete internal layers, horizon boundaries should preferably be multiples of `LayerThickness`.

### Minimum information needed for a soil horizon

To characterize a soil horizon, provide either:

- `KA5TextureClass`
- Both `Sand` and `Clay`

Only one property from each of the following pairs normally needs to be supplied:

- `SoilRawDensity` or `SoilBulkDensity`
- `SoilOrganicCarbon` or `SoilOrganicMatter`

If both members of a pair are supplied, they should be mutually consistent.

The hydraulic properties can either be supplied explicitly or calculated by the configured pedotransfer function:

- `FieldCapacity`
- `PermanentWiltingPoint`
- `PoreVolume`

Explicitly providing measured soil properties can reduce the number of estimated parameters, but does not necessarily improve a simulation unless the measurements are representative and internally consistent.

### Pedotransfer functions

The `pwpFcSatFunction` property selects the method used to calculate missing hydraulic properties.

The default is:

```json
"pwpFcSatFunction": "Wessolek2009"
```

Supported methods in the current implementation are: `Wessolek2009`, `VanGenuchten`, `VanGenuchtenVereecken`, `VanGenuchtenToth`, `Toth`

The input data required for a successful calculation depends on the selected function. For example, `Wessolek2009` uses the KA5 texture class together with density and organic matter information.

### Automatic parameter calculation

MONICA derives the following values where possible:

| Property                | Behavior when omitted                                                   |
|-------------------------|-------------------------------------------------------------------------|
| `Sand`                  | Derived from `KA5TextureClass`                                          |
| `Clay`                  | Derived from `KA5TextureClass`                                          |
| `KA5TextureClass`       | Derived from `Sand` and `Clay` if both are available                    |
| Silt content            | Calculated internally as `1.0 - Sand - Clay`                            |
| `FieldCapacity`         | Calculated by the selected pedotransfer function                        |
| `PermanentWiltingPoint` | Calculated by the selected pedotransfer function                        |
| `PoreVolume`            | Calculated by the selected pedotransfer function                        |
| `Lambda`                | Calculated by the sand and clay content when both are greater than zero |
| `SoilRawDensity`        | Derived from `SoilBulkDensity` and clay content                         |
| `SoilBulkDensity`       | Derived from `SoilRawDensity` and clay content                          |
| `SoilOrganicCarbon`     | Derived from `SoilOrganicMatter`                                        |
| `SoilOrganicMatter`     | Derived from `SoilOrganicCarbon`                                        |

After calculation, MONICA applies lower safeguards to hydraulic properties:

- `FieldCapacity`: at least `0.05`
- `PermanentWiltingPoint`: at least `0.01`
- `PoreVolume`: at least `0.10`

### Soil Properties

The following properties can be specified for each soil profile entry.

| Configuration variable      | Unit                | Description                                         | Default                  | Example                                                                 | Note                                                                                                                       |
|-----------------------------|---------------------|-----------------------------------------------------|--------------------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| **`Thickness`**             | m                   | User-defined horizon thickness                      | internal layer thickness | `"Thickness": 0.3`                                                      | Unit array can use `m`, `dm`, `cm`, or `mm`. The final entry fills the remaining internal profile.                         |
| **`Sand`**                  | kg kg-1             | Soil sand fraction                                  | unset                    | `"Sand": 0.45` or `"Sand": [45, "%"]`                                   | `[45, "%"]` is converted to `0.45`                                                                                         |
| **`Clay`**                  | kg kg-1             | Soil clay fraction                                  | unset                    | `"Clay": 0.12` or `"Clay": [12, "%"]`                                   | `[12, "%"]` is converted to `0.12`                                                                                         |
| **`pH`**                    |                     | Soil pH                                             | `6.9`                    | `"pH": 7.2`                                                             | Valid range: 0-14                                                                                                          |
| **`Sceleton`**              | m3 m-3              | Volumetric stone content                            | `0.0`                    | `"Sceleton": 0.05` or `"Sceleton": [5, "%"]`                            | Positive values above `0.8` are limited to `0.8`.                                                                          |
| **`Lambda`**                |                     | Soil water conductivity coefficient                 | calculated               | `"Lambda": 0.5`                                                         | Calculated from sand and clay when possible                                                                                |
| **`FieldCapacity`**         | m3 m-3              | Volumetric water content at field capacity          | calculated               | `"FieldCapacity": 0.25` or `"FieldCapacity": [25, "%"]`                 | A bare number is interpreted as a fraction. `[25, "%"]` is converted to `0.25`.                                            |
| **`PoreVolume`**            | m3 m-3              | Saturated volumetric water content                  | calculated               | `"PoreVolume": 0.45` or `"PoreVolume": [45, "%"]`                       | A bare number is interpreted as a fraction. `[45, "%"]` is converted to `0.45`. Also referred to internally as saturation. |
| **`PermanentWiltingPoint`** | m3 m-3              | Volumetric water content at permanent wilting point | calculated               | `"PermanentWiltingPoint": 0.10` or `"PermanentWiltingPoint": [10, "%"]` | A bare number is interpreted as a fraction. `[10, "%"]` is converted to `0.10`.                                            |
| **`KA5TextureClass`**       |                     | German KA5 soil texture class                       | derived or unset         | `"KA5TextureClass": "Sl2"`                                              | Texture class matching is case-insensitive internally.                                                                     |
| **`SoilAmmonium`**          | kg NH4-N m-3        | Initial ammonium-N concentration                    | `0.0005`                 | `"SoilAmmonium": 0.001`                                                 |                                                                                                                            |
| **`SoilNitrate`**           | kg NO3-N m-3        | Initial nitrate-N concentration                     | `0.005`                  | `"SoilNitrate": 0.02`                                                   |                                                                                                                            |
| **`CN`**                    |                     | Soil C/N ratio                                      | `10.0`                   | `"CN": 9.5`                                                             |                                                                                                                            |
| **`SoilRawDensity`**        | kg m-3              | Soil raw density                                    | derived or unset         | `"SoilRawDensity": 1450`                                                | Can be derived from bulk density and clay content                                                                          |
| **`SoilBulkDensity`**       | kg m-3              | Soil bulk density                                   | derived or unset         | `"SoilBulkDensity": 1300`                                               | Can be derived from raw density and clay content                                                                           |
| **`SoilOrganicCarbon`**     | mass %              | Soil organic carbon concentration                   | derived or unset         | `"SoilOrganicCarbon": 0.8` or `"SoilOrganicCarbon": [0.8, "%"]`         | A bare value of `0.8` means 0.8%, not a fraction of 0.8. `[0.8, "%"]` is equivalent.                                       |
| **`SoilOrganicMatter`**     | kg OM kg-1          | Soil organic matter fraction                        | derived or unset         | `"SoilOrganicMatter": 0.015` or `"SoilOrganicMatter": [1.5, "%"]`       | `[1.5, "%"]` is converted to `0.015`                                                                                       |
| **`SoilMoisturePercentFC`** | % of field capacity | Initial soil moisture relative to field capacity    | `100.0`                  | `"SoilMoisturePercentFC": 80.0`                                         | Valid range: 0-100                                                                                                         |

### Recommended minimal soil horizon

The following entry contains enough information for the default `Wessolek2009` calculation:

```json
{
  "Thickness": [0.3, "m"],
  "KA5TextureClass": "Sl2",
  "SoilOrganicCarbon": [0.8, "%"],
  "SoilBulkDensity": 1350,
  "SoilMoisturePercentFC": 100
}
```

The selected pedotransfer function and its supporting parameter files must be available for missing hydraulic properties to be calculated.

---

## Site-specific parameters

The following parameters are accepted inside `SiteParameters`.

| Parameter                      | Unit             | Default        | Description                                                                                                                                                   |
|--------------------------------|------------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`Latitude`**                 | decimal degrees  | `52.5`         | Geographic latitude of the site                                                                                                                               |
| **`Slope`**                    | m m-1            | `0.01`         | Surface slope (height m * length m-1)                                                                                                                         |
| **`HeightNN`**                 | m                | `50.0`         | Elevation above sea level                                                                                                                                     |
| **`NDeposition`**              | kg N ha-1 y-1    | `30.0`         | Annual atmospheric nitrogen deposition                                                                                                                        |
| **`NumberOfLayers`**           |                  | `20`           | Number of equal-sized internal soil layers                                                                                                                    |
| **`LayerThickness`**           | m                | `0.1`          | Thickness of each internal soil layer                                                                                                                         |
| **`pwpFcSatFunction`**         |                  | `Wessolek2009` | Function used to calculate missing field capacity, pore volume, and permanent wilting point                                                                   |
| **`ImpenetrableLayerDepth`**   | m                | `-1`           | Depth below the soil surface beyond which roots cannot grow. Values greater than zero limit the crop's maximum rooting depth. `-1` disables this restriction. |

---

## Environment parameters

Parameters that apply to the entire simulation are specified under `EnvironmentParameters`.

They can be loaded from an external parameter file and selectively overridden:

```json
{
  "EnvironmentParameters": {
    "DEFAULT": ["include-from-file", "../monica-parameters/user-parameters/hermes-environment.json"],
    "LeachingDepth": 2.0,
    "WindSpeedHeight": 2.5
  }
}
```

### Atmospheric CO<sub>2</sub> handling

For every simulation day, MONICA determines the atmospheric CO<sub>2</sub> concentration in the following order:

1. **Daily climate data**

    If the daily climate data contain `co2`, that value is used.

2. **Year-specific concentration**
    
    Otherwise, if `AtmosphericCO2s` contains a value for the current year, that value is used.

3. **Dynamic calculation**

    Otherwise, if `AtmosphericCO2` is zero or negative, MONICA calculates the concentration from the simulation date and the selected `rcp` pathway.

4. **Constant concentration**

    Otherwise, `AtmosphericCO2` is used as a constant value.

For practical configurations, use `AtmosphericCO2 <= 0` to request dynamic calculation.

### Atmospheric O<sub>3</sub> handling

For every simulation day, MONICA determines the atmospheric O<sub>3</sub> concentration in the following order:

1. If the daily climate data contain `o3`, that value is used.
2. Otherwise, if `AtmosphericO3s` contains a value for the current year, that value is used.
3. Otherwise, the constant `AtmosphericO3` is used.

MONICA does not calculate O<sub>3</sub> dynamically. If `AtmosphericO3` is omitted, its default value is zero.

### Environment parameter table

| Name of parameter              | Unit  | Default value | Description                                                                                        | Example                                                      |
|--------------------------------|-------|---------------|----------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| **`Albedo`**                   |       | `0.23`        | Surface reflectivity coefficient                                                                   | `"Albedo": 0.23`                                             |
| **`AtmosphericCO2`**           | ppm   | `0.0`         | Constant atmospheric CO<sub>2</sub> concentration, or dynamic calculation switch when non-positive | `"AtmosphericCO2": 420`                                      |
| **`AtmosphericCO2s`**          | ppm   | unset         | Object mapping years to CO<sub>2</sub> concentrations                                              | `"AtmosphericCO2s": {"1991": 360, "1992": 370, "1993": 380}` |
| **`AtmosphericO3`**            | ppm   | `0.0`         | Constant atmospheric O<sub>3</sub> concentration                                                   | `"AtmosphericO3": 0.04`                                      |
| **`AtmosphericO3s`**           | ppm   | unset         | Object mapping years to O<sub>3</sub> values                                                       | `"AtmosphericO3s": {"1991": 0.035, "1992": 0.036}`           |
| **`WindSpeedHeight`**          | m     | `2.0`         | Height above the ground at which wind speed was measured                                           | `"WindSpeedHeight": 2.5`                                     |
| **`LeachingDepth`**            | m     | `0.0`         | Depth at which water and nitrate outflow are evaluated                                             | `"LeachingDepth": 2.0`                                       |
| **`MaxGroundwaterDepth`**      | m     | `18.0`        | Maximum annual groundwater depth below the ground surface                                          | `"MaxGroundwaterDepth": 1.0`                                 |
| **`MinGroundwaterDept`**       | m     | `20.0`        | Minimum annual groundwater depth below the ground surface                                          | `"MinGroundwaterDepth": 0.5`                                 |
| **`MinGroundwaterDepthMonth`** | month | `3`           | Month (1-12) in which the minimum average groundwater depth is reached                             | `"MinGroundwaterDepthMonth": 3`                              |
| **`rcp`**                      |       | `rcp85`       | Pathway used for dynamic CO<sub>2</sub> calculation                                                | `"rcp": "rcp45"`                                             |

The groundwater parameters are interpolated using an annual sinusoidal function. `MinGroundwaterDepth` is reached during `MinGroundwaterDepthMonth`, while `MaxGroundwaterDepth` is reached approximately six months later.

#### Supported RCP pathways

The `rcp parameter` is only used when:

- daily climate data do not provide CO<sub>2</sub>
- `AtmosphericCO2s` does not contain the current year
- `AtmosphericCO2` requests dynamic calculation

Supported dynamically calculated pathways depend on the MONICA version:

| MONICA version | Supported pathways                                            |
|----------------|---------------------------------------------------------------|
| `< 3.6.52`     | `rcp26`, `rcp45`, `rcp60`, `rcp85`                            |
| `>= 3.6.52`    | `rcp19`, `rcp26`, `rcp34`, `rcp45`, `rcp60`, `rcp70`, `rcp85` |

#### Mapping SSP labels to MONICA pathways

SSP and RCP names describe different scenario frameworks. For MONICA's internal CO<sub>2</sub> calculation, the following mapping selects the pathway with the corresponding approximate radiative forcing level:

| SSP scenario | MONICA `rcp` value |
|--------------|--------------------|
| `ssp119`     | `rcp19`            |
| `ssp126`     | `rcp26`            |
| `ssp245`     | `rcp45`            |
| `ssp370`     | `rcp70`            |
| `ssp460`     | `rcp60`            |
| `ssp585`     | `rcp85`            |

This mapping selects comparable forcing levels. It does not make an SSP scenario identical to an RCP scenario.

---

## Example **site.json** file

The following example defines the site location and soil profile and loads standard parameter sets from external files.

```json
{
  "SiteParameters": {
    "Latitude": 52.80939865112305,
    "Slope": 0.1,
    "HeightNN": [0, "m"],
    "NDeposition": 30,
    "NumberOfLayers": 20,
    "LayerThickness": 0.1,
    "pwpFcSatFunction": "Wessolek2009",
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