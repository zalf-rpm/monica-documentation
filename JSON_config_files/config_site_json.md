# **SITE JSON**
**site.json** holds all input data and parameters which could be considers site specific. Besides the key **SiteParameters** in the top-level JSON object, there might be a few JSON objects which set global/general soil and environment specific parameters. These can either be included from the SQLite database (table user_parameter) or the filesystem or be defined directly in **site.json**.  In order to facilitate easy overwriting of the standard parameters, they are included via the **DEFAULT** parameter pseudo-key.

## The soil profile

The key **SoilProfileParameters** contains a **JSON array** of **JSON objects** which describe the layers of the soil profile. MONICA uses internally **20** layers each with a **10cm** thickness. The **SoilProfileParameters** has to contain at least one layer with a minimal set of soil properties (**KA5TextureClass**, **SoilOrganicCarbon/Matter** and **SoilRaw/BulkDensity**). Additionally to the soil properties a key **Thickness** which describes the height of the soil layer. The specified soil profile will internally be normalized to **20** **10cm** layers. If the sizes of the layers won't add up to **2m** (MONICAs used profile depth), the last layer will be extend up to **2m**. Equally, the layers will be cut at **2m** profile depth.

Not all properties are necessary, but the more soil properties are specified, the more exact the calculations will be. If only the **KA5TextureClass** is given, average values for **Sand** and **Clay** content will be used as well as **FieldCapacity**, **PermanentWiltingPoint**, **PoreVolume** and **Lambda** will be calculated based on them.

Also just one of **SoilRawDensity** and **SoilBulkDensity** as well as **SoilOrganicCarbon** and **SoilOrganicMatter** have to be given. If both values are being supplied, they should match.

The following table shows the names of the soil properties which can be set on an individual layer.

Name of config file variable | Unit | Description
---------------------------- | ---- | -----------
**Thickness** | m | thickness of the soil layer
**Sand** | kg kg-1 (% [0-1]) | soil sand content, a percentage between 0 and 1
**Clay** | kg kg-1 (% [0-1]) | soil clay content, a percentage between 0 and 1
**pH** | | soil pH value
**Sceleton** | % [0-1] | soil stone content, a percentage between 0 and 1
**Lambda** | | soil water conductivity coefficient
**FieldCapacity** | m3 m-3 | fieldcapacity
**PoreVolume** | m3 m-3 | saturation
**PermanentWiltingPoint** | m3 m-3 | permanent wilting point
**KA5TextureClass** | | KA5 soil texture
**SoilAmmonium** | kg NH4-N m-3 | initial soil ammonium content
**SoilNitrate** | kg NO3-N m-3 | initial soil nitrate content
**CN** | | soil C/N ratio
**SoilRawDensity** | kg m-3 | soil raw density
**SoilBulkDensity** | kg m-3 | soil bulk density
**SoilOrganicCarbon** | % [0-100] ([kg C kg-1] * 100) | soil organic carbon, a percentage between 0 and 100
**SoilOrganicMatter** | kg OM kg-1 (% [0-1]) | soil organic matter
**SoilMoisturePercentFC** | % [0-100] | initial soil moisture in percent of field capacity


Further site specific variables.

Name of config file variable | Unit | Description
---------------------------- | ---- | -----------
**Latitude** | [decimal degree] | site's latitude
**Slope** | [height m * length m-1]   [0-1] | site's slope
**HeightNN** | [m] | site's height above sea level
**NDeposition** | | N deposition via atmosphere

## Environment parameters for the whole simulation

The following environment parameters for a whole simulation can be adjusted by referencing either the correct json file or overwriting selectively the parameters read from the default file. 

### Atmospheric CO2 (O3)

Currently the following procedure is being applied.

1. Check if **co2** or **o3** field is present in daily climate data. If so use the supplied values!
2. If the parameter **AtmosphericCO2s** (**AtmosphericO3s**) is set to an JSON object of "year" to "CO2 value" mappings. Then use these yearly values.
3. If the parameter **AtmosphericCO2** (**AtmosphericCO2**) is set to 0 or is negative. Let **MONICA** calculate the CO2 concentration depending on the year.
4. If none of the points 1.-3. is true, use the value of **AtmosphericCO2** (**AtmosphericO3**) for the whole simulation.


Name of parameter | Unit | Default value | Description
----------------- | ---- | ------------- | -----------
**Albedo** | | 0.23 | 
**AtmosphericCO2** | ppm | 0.0 | 
**AtmosphericCO2s** | ppm | unset | example value: {"1991": 360, "1992": 370, "1993": 380}
**AtmosphericO3**, | ppm | 0.0 
**AtmosphericO3s** | ppm | unset | 
**WindSpeedHeight** | m s-1 | 2.0 | 
**LeachingDepth** | m | 0.0. | 
**MaxGroundwaterDepth** | m | 18.0 | 
**MinGroundwaterDepth** | m | 20.0 | 
**MinGroundwaterDepthMonth** | m | 3.0 |


## Example **site.json** file

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
