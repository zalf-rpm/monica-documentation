# The `sim.json` configuration file

The `sim.json` file contains simulation-specific information such as the start and end date (MONICA will select the appropriate climate data from `climate.csv`) and whether certain global toggles, such as nitrogen response or the use of secondary yields, are turned on or off.

The default soil discretisation consists of 20 layers, each 0.1 m thick. Both `NumberOfLayers` and `LayerThickness` are configurable, but changing them can affect parameterisation and output definitions that assume the defaults.

---

## Reading climate data (`climate.csv-options`)

Climate data are read from delimited text files, normally `.csv` files. Gzip-compressed files whose names end in `.gz` are also supported. The `climate.csv-options` object in `sim.json` configures how these files are read.

The optional `start-date` and `end-date` keys define the inclusive time range used for the MONICA run. Dates must use the ISO format `YYYY-MM-DD`. If either boundary is omitted, that boundary is inferred from the supplied climate data.

The `no-of-climate-file-header-lines` key specifies the total number of non-data lines at the beginning of the file. By default, the first of these lines contains the column names. Any remaining header lines, such as units line, are ignored. For example:

- `1`: Line 1 contains column names. Data starts on line 2.
- `2`: Line 1 contains column names. Line 2 might contain units. Data starts on line 3.

For a file without a header row, provide column names explicitly with the `header` array and set `no-of-climate-file-header-lines` to `0`.

The `csv-separator` key defines the delimiter. This is commonly `,` or `;`, but a tab `\t`, space, or another character can also be used.

MONICA recognizes the following commonly used **Available Climate Data (ACD)** names. Names are case-sensitive.

| ACD element     | Description                                               | Example      | Unit       |
|-----------------|-----------------------------------------------------------|--------------|------------|
| **`day`**       | Day of month                                              | `5`          |            |
| **`month`**     | Month of year                                             | `11`         |            |
| **`year`**      | Year                                                      | `2017`       |            |                                      
| **`iso-date`**  | Date in `YYYY-MM-DD` format                               | `2017-11-05` |            |          
| **`de-date`**   | Date in `DD.MM.YYYY` format                               | `05.11.2017` |            |               
| **`tmin`**      | Minimum daily air temperature                             | `-2`         | °C         |     
| **`tavg`**      | Average daily air temperature                             | `15.3`       | °C         |    
| **`tmax`**      | Maximum daily air temperature                             | `34.7`       | °C         |  
| **`precip`**    | Daily precipitation                                       | `2.3`        | mm         |
| **`globrad`**   | Daily global radiation                                    | `27.431`     | MJ m-2 d-1 |   
| **`sunhours`**  | Daily sunshine duration (prefer `globrad` when available) | `8.5`        | h          | 
| **`wind`**      | Wind speed                                                | `6.7`        | m s-1      |
| **`relhumid`**  | Relative humidity                                         | `90.0`       | %          |
| **`skip`**      | Ignore this column                                        |              |            |

A date must be supplied either through `iso-date`, `de-date`, or the combination of `day`, `month`, and `year`.

If `tavg` is absent, MONICA calculates it as `(tmin + tmax) / 2`. Either `globrad` or `sunhours` must be present. If only `sunhours` is provided, MONICA estimates global radiation using the site latitude.

Unknown column headers are skipped automatically. A column can also be ignored explicitly by mapping its header to `skip`. This is useful, for example, when the file contains multiple columns representing the same climate variable.

Column names can be mapped to ACD names with the `header-to-acd-names` object. Each key is a column name from the input file, and its value is the corresponding ACD name.

A mapping value may instead be a JSON array with three elements:

1. the ACD name
2. an arithmetic operation: `+`, `-`, `*`, `/`
3. a numeric operand

The operation is applied to values read for that climate element. For example, values in `J cm-2` are converted to `MJ m-2` by multiplying by `0.01`.

```json
{
  "climate.csv-options": {
    "start-date": "1991-01-01",
    "end-date": "1997-12-31",
    "no-of-climate-file-header-lines": 1,
    "csv-separator": ",",
    "header-to-acd-names": {
      "DE-date": "de-date",
      "global_radiation": ["globrad", "*", 0.01]
    }
  }
}
```

For a headerless file, the configuration would instead look like:

```json
{
  "climate.csv-options": {
    "no-of-climate-file-header-lines": 0,
    "csv-separator": ",",
    "header": [
      "iso-date",
      "tavg",
      "tmin",
      "tmax",
      "wind",
      "globrad",
      "precip",
      "relhumid"
    ]
  }
}
```

---

## Outputting results from MONICA

Results requested from a MONICA run can be written in CSV format. The requested outputs, their aggregation, and the output destination are configured in `sim.json` under the key `output`.

The following keys are direct members of `output`:

| Key                  | Description                                                                                                                                                          |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`path-to-output`** | Directory in which output files are created                                                                                                                          |
| **`file-name`**      | Name of the output file                                                                                                                                              |
| **`write-file?`**    | When `true`, write output to a file. When `false`, the command-line runner writes output to standard output, unless an output file is specified on the command line. |
| **`events`**         | An array of alternating event definitions and lists of requested outputs                                                                                             |

CSV formatting is configured in the nested `output.csv-options` object:

| Key                            | Description                                                                                                                                       |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| **`include-header-row`**       | Include a row containing output names                                                                                                             |
| **`include-units-row`**        | Include a row containing units                                                                                                                    |
| **`include-aggregation-rows`** | Include two diagnostic rows containing MONICA's normalized output specifications (`m:`) and the originally requested output specifications (`j:`) |
| **`csv-separator`**            | Column separator, for example `","`                                                                                                               |

Missing Boolean values are read as `false` by the command-line runner. Set the three `include-*` options explicitly when the corresponding rows are required.

### Defining events

The `events` key specifies which MONICA results to produce and when to produce them. Its value is a JSON array containing alternating **event definitions** and **output lists**. 

```json
"events": [
  "daily", [
    "Date",
    "Tavg"
  ],
  "Harvest", [
    "Date",
    "Yield"
  ]
]
```

Each event definition can be: 

- a string, such as `"daily"`
- a JSON array containing a comparison expression
- a JSON object defining one or more trigger conditions

MONICA recognizes the string shortcuts `"daily"`, `"monthly"`, `"yearly"`, `"run"`, and `"crop"`. Other strings are interpreted as an `at` trigger, such as a date pattern or a named workstep event.

A comparison-expression array is normally shorthand for an `at` condition:

```json
["Stage", "=", 2]
```

This is equivalent to:

```json
{"at": ["Stage", "=", 2]}
```

The four-element forms `["at", output, operator, value]` and `["while", output, operator, value]` are also supported.

An event object can contain:

1. `start` and `end`, defining the inclusive period during which the event is active
2. `from` and `to`, defining an inclusive aggregation range
3. `at`, producing non-aggregated output when a date, named event, or condition matches
4. `while`, collecting values while a condition is true and producing aggregated output when the period ends

Date triggers use an ISO-shaped `YYYY-MM-DD` pattern. Components may be replaced by wildcards: `xxxx` for any year and `xx` for any month or day. For example, `xxxx-xx-15` matches the 15th day of every month during the simulation. A specified day that exceeds the number of days in a month is treated as that month's last day. Thus, `xxxx-xx-31` matches the final day of every month.

#### Available events

Event expressions determine when MONICA records or aggregates output values. They may refer to calendar date patterns, model output values, phenological events, or successfully applied worksteps. Event and output names are case-sensitive.

##### Model and phenological events

Common model-generated events include:

- `Stage-1`, `Stage-2`, ...
- `emergence`
- `anthesis`
- `cereal-stem-elongation`
- `maturity`

##### Workstep events

A succesfully applied workstep generally emits an event matching its type, for example:

- `Sowing`
- `AutomaticSowing`
- `Harvest`
- `AutomaticHarvest`
- `Cutting`
- `MineralFertilization`
- `NDemandFertilization`
- `OrganicFertilization`
- `Tillage`
- `Irrigation`
- `AutomaticIrrigation`
- `SetValue`

Automatic worksteps may emit both their base event and their automatic event. For example, automatic sowing emits both `Sowing` and `AutomaticSowing`.

##### Event expressions and shortcuts

The following table shows event expression examples, shortcuts, and their expanded forms:

| Shortcut                     | Expanded form                                                          | Meaning                                                                                                                   |
|------------------------------|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
|                              | `{"start": "xxxx-05-01", "end": "xxxx-07-31", "at": "xxxx-xx-15"}`     | Write results on the 15th of each month from May to July                                                                  |
|                              | `{"from": "Sowing", "to": "anthesis", "while": ["ETa/ETc", "<", 0.4]}` | Write aggregated results from sowing through anthesis, but only while actual to potential evapotranspiration is below 0.4 |
| `"xxxx-03-31"`               | `{"at": "xxxx-03-31"}`                                                 | Write results every March 31st                                                                                            |
| `"daily"`                    | `{"at": "xxxx-xx-xx"}`                                                 | Write daily results                                                                                                       |
| `"monthly"`                  | `{"from": "xxxx-xx-01", "to": "xxxx-xx-31"}`                           | Write monthly aggregated results                                                                                          |
| `"yearly"`                   | `{"from": "xxxx-01-01", "to": "xxxx-12-31"}`                           | Write yearly aggregated results                                                                                           |
| `"run"`                      | `{"from": <start-date>, "to": <end-date>}`                             | Write results aggregated over the entire simulation run                                                                   |
| `"crop"`                     | `{"from": "Sowing", "to": "Harvest"}`                                  | Write results aggregated over the cropping period                                                                         |
| `["while", "Stage", "=", 5]` | `{"while": ["Stage", "=", 5]}`                                         | Write results aggregated only while `Stage` equals 5                                                                      |
| `["at", "Stage", "=", 2]`    | `{"at": ["Stage", "=", 2]}`                                            | Write results daily when `Stage` equals 2                                                                                 |
| `[["Mois", 1], "<", 0.5]`    | `{"at": [["Mois", 1], "<", 0.5]}`                                      | Write results daily when the soil moisture in the first layer is below 0.5                                                |
| `"Sowing"`                   | `{"at": "Sowing"}`                                                     | Write results at sowing time                                                                                              |

---

## List of outputs

After defining an output event (i.e., when MONICA should produce output), you must specify **what** it should output.

For example:

- `"Stage"` returns the crop's current development stage.
- `["Mois", 1]` returns the volumetric soil moisture content of the first soil layer.
- `["OrgBiom", "Root"]` returns the current root biomass.

With the default layer thickness of 0.1 m, layer 1 represents a depth of 0-0.1 m.

### Output types

MONICA output variables can return:

1. Scalar numeric values, such as `"Stage"` or `"Precip"`
2. Scalar text values, such as `"Date"` or `"Crop"`
3. Values defined for individual soil layers, such as `"Mois"`, `"NO3"`, or `"STemp"`
4. Values defined for crop organs, such as `"OrgBiom"`, `"NPP-Organs"`, or `"Ra-Organs"`

Layer-based outputs must normally be accompanied by a layer number or layer range. For example:

```json
["Mois", 1]
```

or:

```json
["Mois", [1, 20]]
```

### Soil layers

MONICA uses 20 soil layers of 0.1 m each by default, resulting in a total simulated soil depth of 2 m. These defaults can be changed using `NumberOfLayers` and `LayerThickness`.

Layer numbers in output definitions are one-based:

- Layer `1` is the first configured layer.
- Layer `15` spans 1.4-1.5 m with the default discretisation.
- `[1, 5]` selects layers 1 through 5, inclusive, corresponding to 0-0.5 m with the default discretisation.

The selected layers must exist in the configured soil profile.

Examples:

```json
["Mois", 1]
```

returns soil moisture for layer 1, whereas:

```json
["Mois", [1, 5]]
```

returns a separate value for each of layers 1 through 5. 

Without a display-name override, these values receive column names such as:

```
Mois_1, Mois_2, Mois_3, Mois_4, Mois_5
```

### Crop organs

Organ-specific outputs use one of the following organ selectors:

```
Root, Leaf, Shoot, Fruit, Struct, Sugar
```

For example:

```json
["OrgBiom", "Leaf"]
```

returns the current leaf biomass.

Not every crop necessarily defines every organ. If the selected organ is not present in the current crop, the output may be zero.

Output names are case-sensitive. Aggregation operation and organ names are case-insensitive. Nevertheless, using the uppercase operation names and the organ spelling shown above is recommended.

### Output definition syntax

An output can be specified either as a string:

```json
"Stage"
```

or as a JSON array whose first element is the output name:

```json
["Mois", 1]
```

The general forms are:

```json
"OutputName"
```

```json
["OutputName", "TIME_OPERATION"]
```

```json
["OutputName", LAYER]
```

```json
["OutputName", LAYER, "TIME_OPERATION"]
```

```json
["OutputName", [FIRST_LAYER, LAST LAYER]]
```

```json
["OutputName", [FIRST_LAYER, LAST LAYER, "LAYER_OPERATION"]]
```

```json
["OutputName", [FIRST_LAYER, LAST LAYER, "LAYER_OPERATION"], "TIME_OPERATION"]
```

and, for organ-specific outputs:

```json
["OutputName", "ORGAN"]
```

```json
["OutputName", "ORGAN", "TIME_OPERATION"]
```

MONICA silently omits unknown output names. Therefore, output names should be copied exactly from the list of supported outputs.

### Display names

A display name can be appended to an output name using `|`:

```json
["DOY|MatDOY", "LAST"]
```

The output column is then named `MatDOY` instead of `DOY`.

Another example is:

```json
["PercolationRate|WDrain", 15, "SUM"]
```

This selects percolation from layer 15, sums it over the event's aggregation period, and writes the result under the name `WDrain`.

When a display name is used for an unaggregated layer range, the same display may be used for every selected layer.

### Aggregation

MONICA supports two independent forms of aggregation:

1. **Layer aggregation** combines values from several soil layers at each sampling step.
2. **Time aggregation** combines values collected over an event period, such as a month, year, crop cycle, or complete simulation run.

The supported operations are:

```
AVG, MEDIAN, SUM, MIN, MAX, FIRST, LAST, NONE
```

### Layer aggregation

A layer aggregation operation is placed inside the layer range array:

```json
["SOC", [1, 3, "AVG"]]
```

At every sampling step, this calculates one value by averaging SOC across layers 1 through 3.

If no layer operation is specified, the default is `NONE`:

```json
["SOC", [1, 3]]
```

This produces three separate values rather than one aggregated value.

For layer aggregation:

- `AVG` returns the arithmetic mean across the selected layers
- `MEDIAN` returns the median across the selected layers
- `SUM` returns the sum across the selected layers
- `MIN` and `MAX` return the smallest and largest values
- `FIRST` returns the value from the shallowest selected layer
- `LAST` returns the value from the deepest selected layer
- `NONE` preserves the individual layer values

Layer aggregation is applied at each sampling step, before any time aggregation.

### Time aggregation

A time aggregation operation is specified as the second element for scalar outputs:

```json
["Precip", "SUM"]
```

or as the third outer element for layer- or organ-specific outputs:

```json
["PercolationRate", 15, "SUM"]
```

```json
["SOC", [1, 3, "AVG"], "LAST"]
```

```json
["OrgBiom", "Leaf", "MAX"]
```

For event periods such as `monthly`, `yearly`, `crop`, and `run`, MONICA temporarily collects values and applies the selected time operation when the event period ends.

For numeric outputs, the default time operation is `AVG`.

For example:

```json
"Act_ET"
```

in a monthly event returns the average of the collected daily values. To obtain monthly cumulative evapotranspiration, use:

```json
["Act_ET", "SUM"]
```

The period boundaries are inclusive. For an unfiltered `from`/`to` period:

- `FIRST` returns the value collected on the `from` day.
- `LAST` returns the value collected on the `to` day.

If the event also contains a `while` condition, `FIRST` and `LAST` refer to the first and last values actually collected while the condition was true.

For text outputs such as `Date` and `Crop`, use `FIRST` or `LAST` explicitly. Other operations are numeric and are not meaningful for text values.

Although `NONE` is accepted as a time operation, it currently behaves like `LAST`. Use `LAST` explicitly when that behavior is required.

For instantaneous events such as `daily`, a date event, or `Harvest`, no time range is accumulated. Consequently, the time operation has no practical effect.

### Aggregation defaults

| Aggregation type         | Default     | Meaning                                              |
|--------------------------|-------------|------------------------------------------------------|
| Layer aggregation        | `NONE`      | Return a separate value for every selected layer     |
| Numeric time aggregation | `AVG`       | Average the values collected during the event period |
| Text time aggregation    | First value | Use `FIRST` or `LAST` explicitly for clarity         |

### Combining layer and time aggregation

Layer and time aggregation can be combined:

```json
["Mois", [1, 15, "AVG"], "LAST"]
```

This definition:

1. Averages soil moisture across layers 1–15 on every sampling day.
2. Returns the resulting average from the last day of the event period.

Similarly:

```json
["SOC", [1, 3, "AVG"], "AVG"]
```

first averages `SOC` across layers 1–3 on each day and then averages those daily values over the event period.

### Examples

| Output definition                        | Meaning                                                                                       |
|------------------------------------------|-----------------------------------------------------------------------------------------------|
| `"Date"`                                 | Return the current date in ISO format, for example `2017-01-17`                               |                                                                                                                                                                         
| `["Year", "LAST"]`                       | Return the year associated with the final collected value                                     |                                                                                                                                                 
| `["PercolationRate\|WDrain", 15, "SUM"]` | Sum percolation rate from layer 15 over the event period and name the result `WDrain`         | 
| `["Mois", [1, 20]]`                      | Return one soil moisture value for every layer                                                |
| `["Mois", [1, 20, "AVG]]`                | Return one value containing the mean soil moisture across all 20 layers at each sampling step |
| `["OrgBiom", "Leaf"]`                    | Return current leaf biomass                                                                   |                                                                                                                                                         
| `["SOC", [1, 3, "AVG"]]`                 | Average soil organic carbon across layers 1-3 at each sampling step                           |
| `["SOC", [1, 3, "AVG], "LAST]`           | Return the layer average SOC value from the final day of the event period                     |
| `["Precip", "SUM"]`                      | Sum precipitation over the event period                                                       |
| `["SnowD", "MAX"]`                       | Return the maximum snow depth during the event period                                         | 

### Output order

Within each event section, output columns follow the order in which their definitions appear in the `events` list.

Event sections are also processed in their configured order. Depending on the runner configuration, the sections may be written consecutively to one file or to separate files.

Unknown output names are omitted, so they do not reserve a position in the output.

### Example output configuration

The example below shows an output section that produces an equivalent file to the `rmout` file from MONICA version 1. It also includes a commented out (`__events`) example of outputs used in the **EU MACSUR Heat Stress** study.

```json
{
"output": {
  "write-file?": false,
  "file-name": "out.csv",

  "csv-options": {
    "include-header-row": true,
    "include-units-row": true,
    "include-aggregation-rows": true,
    "csv-separator": ","
  },

  "__events": [
    "crop", [
      ["Year", "LAST"],
      ["DOY|SowDOY", "FIRST"],
      ["LAI|MaxLAI", "MAX"],
      ["PercolationRate|WDrain", 15, "SUM"],
      ["Act_ET|CumET", "SUM"],
      ["Act_Ev|Evap", "SUM"],
      ["Mois|SoilAvW", [1, 15, "SUM"], "LAST"],
      ["RunOff", "SUM"],
      ["ET0|Eto", "SUM"],
      ["Tmax|TMAXAve", "AVG"],

      ["Yield", "LAST"],
      ["AbBiom|Biom-ma", "LAST"],
      ["AbBiomN|CroN-ma", "LAST"],
      ["GrainN", "LAST"]
    ],

    ["while", "Stage", "=", 5], [
      ["DOY|AntDOY", "FIRST"],
      ["AbBiom|Biom-an", "First"],
      ["AbBiomN|CroN-an", "FIRST"]
    ],

    ["while", "Stage", "=", 7], [
      ["Yield", "FIRST"],
      ["DOY|MatDOY", "FIRST"],
      ["AbBiom|Biom-ma", "First"],
      ["AbBiomN|CroN-ma", "FIRST"],
      ["GrainN", "FIRST"]
    ],

    ["while", "Stage", "=", 2], [
      ["DOY|EmergDOY", "FIRST"]
    ]
  ],

  "events" : [
    "daily", [
      "Date", "Crop", "TraDef", "Tra", "NDef", "HeatRed", "FrostRed", "OxRed",
      "Stage", "TempSum", "VernF", "DaylF",
      "IncRoot", "IncLeaf", "IncShoot", "IncFruit",
      "RelDev", "LT50", "AbBiom",
      ["OrgBiom", "Root"], ["OrgBiom", "Leaf"], ["OrgBiom", "Shoot"],
      ["OrgBiom", "Fruit"], ["OrgBiom", "Struct"], ["OrgBiom", "Sugar"],
      "Yield", "SecondaryYield", "GroPhot", "NetPhot", "MaintR", "GrowthR", "StomRes",
      "Height", "LAI", "RootDep", "EffRootDep", "TotBiomN", "AbBiomN", "SumNUp",
      "ActNup", "PotNup", "NFixed", "Target", "CritN", "AbBiomNc", "YieldNc",
      "Protein",
      "NPP", ["NPP-Organs", "Root"], ["NPP-Organs", "Leaf"], ["NPP-Organs", "Shoot"],
      ["NPP-Organs", "Fruit"], ["NPP-Organs", "Struct"], ["NPP-Organs", "Sugar"],
      "GPP",
      "Ra",
      ["Ra-Organs", "Root"], ["Ra-Organs", "Leaf"], ["Ra-Organs", "Shoot"],
      ["Ra-Organs", "Fruit"], ["Ra-Organs", "Struct"], ["Ra-Organs", "Sugar"],
      ["Mois", [1, 20]], "Precip", "Irrig", "Infilt", "Surface", "RunOff", "SnowD", "FrostD",
      "ThawD", ["PASW", [1, 20]], "SurfTemp", ["STemp", [1, 5]],
      "Act_Ev", "Act_ET", "ET0", "Kc", "AtmCO2", "Groundw", "Recharge", "NLeach",
      ["NO3", [1, 20]], ["Carb", 1], ["NH4", [1, 20]], ["NO2", [1, 4]],
      ["SOC", [1, 6]], ["SOC-X-Y", [1, 3, "SUM"]], ["SOC-X-Y", [1, 20, "SUM"]],
      ["AOMf", 1], ["AOMs", 1], ["SMBf", 1], ["SMBs", 1], ["SOMf", 1],
      ["SOMs", 1], ["CBal", 1], ["Nmin", [1, 3]], "NetNmin", "Denit", "N2O", "SoilpH",
      "NEP", "NEE", "Rh", "Tmin", "Tavg", "Tmax", "Wind", "Globrad", "Relhumid", "Sunhours",
      "NFert"
    ]
  ]
}
}
```

Below you will find a simplified example of a complete `sim.json` file focused on output configuration.

```json
{
  "climate.csv-options": {
		"__given the start and end date, monica will run just this time range, else the full time range given by supplied climate data": "",
		"start-date": "1991-01-01",
		"end-date": "1997-12-31",

		"no-of-climate-file-header-lines": 1,
		"csv-separator": ",",
		"header-to-acd-names": {
			"DE-date": "de-date"
		}
	},

  "output": {
	  "write-file?": false,
		"file-name": "out.csv",

		"__how to write and what to include in monica CSV output": "",
		"csv-options": {
			"include-header-row": true,
			"include-units-row": true,
			"include-aggregation-rows": true,
			"csv-separator": ","
		},

		"__what data to include in the monica output according to the events defined by the keys": "",
		"events" : [
			"monthly", [
				"Year", "Month",
				["SOC", [1, 1, "AVG"]], ["SOC", [1, 3, "AVG"]],
				["WaterContent", [1, 9, "AVG"]], "Recharge", "NLeach",
				["SnowD", "MAX"], ["SnowD", "SUM"], ["FrostD", "SUM"],
				["RunOff", "SUM"], ["NH3", "SUM"], ["Precip", "SUM"], "Act_ET"
			],

			"yearly", [
                "Year",
                ["N", [1, 3]],
                ["RunOff", "SUM"],
                ["NLeach", "SUM"],
                ["Recharge", "SUM"]
            ],

			"run", [["Precip", "SUM"]],

			"Harvest", ["Crop", ["OrgBiom", "Fruit"], "Yield"]
		]
	},

  "NumberOfLayers": 20,
  "LayerThickness": [0.1, "m"],

  "UseSecondaryYields": true,
  "NitrogenResponseOn": true,
  "WaterDeficitResponseOn": true,
  "EmergenceMoistureControlOn": true,
  "EmergenceFloodingControlOn": true,
  "FrostKillOn": true,

  "UseAutomaticIrrigation": false,
  "AutoIrrigationParams": {
    "irrigationParameters": {
      "nitrateConcentration": [0, "mg dm-3"],
      "sulfateConcentration": [0, "mg dm-3"]
    },
    "amount": [17, "mm"],
    "threshold": 0.35
  },

  "UseNMinMineralFertilisingMethod": false,
  "NMinUserParams": { "min": 0, "max": 0, "delayInDays": 0 },
  "NMinFertiliserPartition": {
    "id": "my AN",
    "name": "my very own ammonium nitrate variant",
    "Carbamid": 0,
    "NH4": 0,
    "NO3": 0
  }
}
```

---

## Common output variables

The tables below group commonly used output variables by topic. The most up-to-date list is defined in [**`build-output.cpp`**](https://github.com/zalf-rpm/monica/blob/master/src/io/build-output.cpp).

Output names are case-sensitive. Unknown names are ignored, so check the generated header when adding a new output definition.

```C++
build({id++, "Date", "", "output current date"},
...
build({id++, "TraDef", "0;1", "TranspirationDeficit"},
```

In this example, `Date` and `TraDef` are allowed output names. Additionally, the code lists the expected units of measure (if defined) and provides a description for each output variable.

### General simulation information

| Output name            | (L)ayers/(O)rgans | Settable? | Unit | Description                                                       |
|------------------------|-------------------|-----------|------|-------------------------------------------------------------------|
| **`Count`**            |                   |           |      | Constant value `1` for counting outputs or events                 |
| **`CM-count`**         |                   |           |      | Order number of the currently sown valid crop (`0` before sowing) |
| **`Date`**             |                   |           |      | Simulation date (`YYYY-MM-DD`)                                    |
| **`days-since-start`** |                   |           | d    | Number of days since simulation start                             |
| **`DOY`**              |                   |           |      | Current day of year (`1-365`, or `1-366` in leap years)           |
| **`Month`**            |                   |           |      | Current month number (`1-12`)                                     |
| **`Year`**             |                   |           |      | Current year                                                      |
| **`Crop`**             |                   |           |      | Crop name (`species/cultivar`, empty when no crop is growing)     |

### Weather and atmospheric conditions

| Output name    | (L)ayers/(O)rgans | Settable? | Unit   | Description                              |
|----------------|-------------------|-----------|--------|------------------------------------------|
| **`AtmCO2`**   |                   |           | ppm    | Atmospheric CO<sub>2</sub> concentration |
| **`Tmin`**     |                   |           | °C     | Daily minimum air temperature            |
| **`Tavg`**     |                   |           | °C     | Daily mean air temperature               |
| **`Tmax`**     |                   |           | °C     | Daily maximum air temperature            |
| **`Precip`**   |                   |           | mm     | Daily precipitation                      |
| **`Wind`**     |                   |           | m s-1  | Daily wind speed                         |
| **`Globrad`**  |                   |           | MJ m-2 | Daily global radiation                   |
| **`Relhumid`** |                   |           | %      | Daily relative humidity                  |
| **`Sunhours`** |                   |           | h      | Daily sunshine duration                  |

### Crop development and stress responses

| Output name           | (L)ayers/(O)rgans | Settable? | Unit   | Description                                                                                                                |
|-----------------------|-------------------|-----------|--------|----------------------------------------------------------------------------------------------------------------------------|
| **`TraDef`**          |                   |           | 0-1    | Ratio of actual to potential transpiration (`1` = no stress, `0` = maximum stress)                                         |
| **`NDef`**            |                   |           | 0-1    | Crop N reduction factor (`1` = no stress, `0` = maximum stress)                                                            | 
| **`HeatRed`**         |                   |           | 0-1    | Heat stress reduction factor (`1` = no damage, `0` = maximum damage)                                                       |
| **`FrostRed`**        |                   |           | 0-1    | Frost damage reduction factor (`1` = no damage, `0` = maximum damage)                                                      |
| **`OxRed`**           |                   |           | 0-1    | Oxygen stress reduction factor (`1` = no damage, `0` = maximum damage)                                                     |
| **`Stage`**           |                   | x         | 1-N    | One-based phenological stage number (`0` = no crop)                                                                        |
| **`TempSum`**         |                   |           | °C d   | Accumulated effective temperature sum                                                                                      |
| **`VernF`**           |                   |           | 0-1    | Vernalisation factor (`1` = unrestricted, `0` = fully restricted)                                                          |
| **`DaylF`**           |                   |           | 0-1    | Daylength response factor (`1` = unrestricted, `0` = fully restricted)                                                     |
| **`RelDev`**          |                   |           | 0-1    | Ratio of the current accumulated temperature sum to the crop's total temperature requirement (`0` = start, `1` = maturity) |
| **`LT50`**            |                   |           | °C     | Crop crown temperature causing 50% lethal damage                                                                           |
| **`RootNDef`**        |                   |           | 0-1    | Root N reduction factor (`1` = no stress, `0` = maximum stress)                                                            |
| **`TimeUnderAnoxia`** |                   |           | d      | Internal anoxia duration counter (`0` = no current anoxia)                                                                 |
| **`Tmax>=40`**        |                   |           | 0 or 1 | `1` when the current daily maximum air temperature is at least 40 °C, otherwise `0`                                        |

### Crop biomass and productivity

| Output name                 | (L)ayers/(O)rgans | Settable? | Unit               | Description                                                                                                         |
|-----------------------------|-------------------|-----------|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **`IncRoot`**               |                   |           | kg CH2O ha-1 d-1   | Daily root biomass increment                                                                                        |
| **`IncLeaf`**               |                   |           | kg CH2O ha-1 d-1   | Daily leaf biomass increment                                                                                        |
| **`IncShoot`**              |                   |           | kg CH2O ha-1 d-1   | Daily shoot biomass increment                                                                                       |
| **`IncFruit`**              |                   |           | kg CH2O ha-1 d-1   | Daily fruit or storage organ biomass increment                                                                      |
| **`AbBiom`**                |                   |           | kg DM ha-1         | Aboveground crop biomass                                                                                            |
| **`OrgBiom`**               | O                 |           | kg DM ha-1         | Total biomass of the selected crop organ, including green and dead biomass (e.g., `Root`, `Leaf`, `Shoot`, `Fruit`) |
| **`OrgGreenBiom`**          | O                 |           | kg DM ha-1         | Green biomass of the selected crop organ (e.g., `Root`, `Leaf`, `Shoot`, `Fruit`)                                   |
| **`Yield`**                 |                   |           | kg DM ha-1         | Primary crop yield                                                                                                  |
| **`SecondaryYield`**        |                   |           | kg DM ha-1         | Secondary crop yield                                                                                                |
| **`sumExportedCutBiomass`** |                   |           | kg DM ha-1         | Cumulative exported cut biomass                                                                                     |
| **`exportedCutBiomass`**    |                   |           | kg DM ha-1         | Exported biomass from the latest cut                                                                                |
| **`sumResidueCutBiomass`**  |                   |           | kg DM ha-1         | Cumulative cut residue biomass                                                                                      |
| **`residueCutBiomass`**     |                   |           | kg DM ha-1         | Residue biomass from the latest cut                                                                                 |
| **`GroPhot`**               |                   |           | kg CH2O ha-1 d-1   | Daily gross photosynthesis                                                                                          |
| **`NetPhot`**               |                   |           | kg CH2O ha-1 d-1   | Net daily assimilates                                                                                               |
| **`MaintR`**                |                   |           | kg CH2O ha-1 d-1   | Daily maintenance respiration (AGROSIM-based)                                                                       |
| **`GrowthR`**               |                   |           | kg CH2O ha-1 d-1   | Daily growth respiration (AGROSIM-based)                                                                            |
| **`StomRes`**               |                   |           | s m-1              | Stomatal resistance                                                                                                 |
| **`Height`**                |                   |           | m                  | Crop height                                                                                                         |
| **`LAI`**                   |                   |           | m2 leaf m-2 ground | Leaf area index                                                                                                     |
| **`Ass`**                   |                   |           | kg CH2O ha-1 d-1   | Net daily assimilates                                                                                               |
| **`LightInterception1`**    |                   |           |                    | Radiation intercepted by the sole, upper, or shorter canopy                                                         |
| **`LightInterception2`**    |                   |           |                    | Radiation intercepted by the taller crop's lower canopy                                                             |
| **`BedGrad`**               |                   |           | 0-1                | Crop canopy cover fraction                                                                                          |

### Crop nitrogen

| Output name              | (L)ayers/(O)rgans | Settable? | Unit               | Description                                                               |
|--------------------------|-------------------|-----------|--------------------|---------------------------------------------------------------------------|
| **`TotBiomN`**           |                   |           | kg N ha-1          | N content of total crop biomass                                           |
| **`AbBiomN`**            |                   |           | kg N ha-1          | N content of aboveground biomass                                          |
| **`SumNUp`**             |                   |           | kg N ha-1          | Cumulative crop N uptake                                                  |
| **`ActNup`**             |                   |           | kg N ha-1 d-1      | Daily actual crop N uptake                                                |
| **`PotNup`**             |                   |           | kg N ha-1 d-1      | Daily potential crop N uptake                                             |
| **`NFixed`**             |                   |           | kg N ha-1 d-1      | Daily biological atmospheric N fixation                                   |
| **`Target`**             |                   |           | kg N kg-1 DM       | Target aboveground biomass N concentration                                |
| **`CritN`**              |                   |           | kg N kg-1 DM       | Critical aboveground biomass N concentration                              |
| **`AbBiomNc`**           |                   |           | kg N kg-1 DM       | Current aboveground biomass N concentration                               |
| **`Nstress`**            |                   |           | 0-1                | Crop N sufficiency (`1` = sufficient N)                                   |
| **`YieldNc`**            |                   |           | kg N kg-1 DM       | Primary yield N concentration                                             |
| **`Protein`**            |                   |           | kg protein kg-1 DM | Primary yield crude protein concentration, calculated as `YieldNc * 6.25` |
| **`GrainN`**             |                   |           | kg N ha-1          | Fruit organ N content                                                     |
| **`Nresid`**             |                   |           | kg N ha-1          | Crop residue N content                                                    |
| **`ActNupLayer`**        | L                 |           | kg N ha-1 d-1      | Daily N uptake from the selected soil layer                               |
| **`YieldN`**             |                   |           | kg N ha-1          | Primary yield N content                                                   |
| **`rootNConcentration`** |                   |           | kg N kg-1 DM       | Root biomass N concentration                                              |

### Crop roots and water use

| Output name                      | (L)ayers/(O)rgans | Settable? | Unit            | Description                                                                           |
|----------------------------------|-------------------|-----------|-----------------|---------------------------------------------------------------------------------------|
| **`Tra`** or **`Transpiration`** |                   |           | mm              | Daily actual crop transpiration                                                       |
| **`Act_Trans`**                  |                   |           | mm              | Daily actual crop transpiration, rounded to one decimal place                         |
| **`RootDep`**                    |                   |           | layer count     | Rooting depth in soil layers                                                          |
| **`EffRootDep`**                 |                   |           | m               | Maximum effective rooting depth                                                       |
| **`rootDensity`**                | L                 |           | m root m-3 soil | Root density in the selected soil layer                                               |
| **`rootingZone`**                |                   |           | layer count     | Number of soil layers in the rooting zone                                             |
| **`PotTraDef`**                  |                   |           | mm              | Water supply deficit in the last processed root layer                                 |
| **`ActTraDef`**                  |                   |           | mm              | Actual deficit in the last processed root layer                                       |
| **`TraRed`**                     |                   |           | mm              | Transpiration reduction in the last processed root layer                              |
| **`RootWaUptak`**                | L                 |           | mm              | Water extracted from the selected layer                                               |
| **`Kc`**                         |                   |           |                 | Current crop coefficient used to calculate potential evapotranspiration as `ET0 * Kc` |
| **`Kcb`**                        |                   |           |                 | FAO-56 basal crop coefficient                                                         |

### Ecosystem carbon fluxes

| Output name      | (L)ayers/(O)rgans | Settable? | Unit          | Description                                           |
|------------------|-------------------|-----------|---------------|-------------------------------------------------------|
| **`NPP`**        |                   |           | kg C ha-1 d-1 | Net primary production: `GPP - Ra`                    |
| **`NPP-Organs`** | O                 |           | kg C ha-1 d-1 | NPP allocated to the selected organ by biomass share  |
| **`GPP`**        |                   |           | kg C ha-1 d-1 | Gross primary production                              |
| **`Ra`**         |                   |           | kg C ha-1 d-1 | Autotrophic crop respiration                          |
| **`Ra-Organs`**  | O                 |           | kg C ha-1 d-1 | `Ra` allocated to the selected organ by biomass share |
| **`NEP`**        |                   |           | kg C ha-1 d-1 | Net ecosystem production: `NPP - Rh`                  |
| **`NEE`**        |                   |           | kg C ha-1 d-1 | Net ecosystem exchange: `Rh - NPP`                    |
| **`Rh`**         |                   |           | kg C ha-1 d-1 | Heterotrophic decomposer respiration                  |

### Soil water balance

| Output name                                           | (L)ayers/(O)rgans | Settable? | Unit   | Description                                                         |
|-------------------------------------------------------|-------------------|-----------|--------|---------------------------------------------------------------------|
| **`Mois`**                                            | L                 | x         | m3 m-3 | Volumetric soil water content                                       |
| **`Infilt`**                                          |                   |           | mm     | Daily infiltration into the topsoil                                 |
| **`Surface`**                                         |                   |           | mm     | Surface water storage                                               |
| **`RunOff`**                                          |                   |           | mm     | Daily surface water runoff                                          |
| **`SnowD`**                                           |                   |           | mm     | Snow depth                                                          |
| **`PASW`**                                            | L                 |           | m3 m-3 | Plant available soil water (`Mois - Pwp`)                           |
| **`Act_Ev`**                                          |                   |           | mm     | Actual evaporation                                                  |
| **`Pot_ET`**                                          |                   |           | mm     | Potential evapotranspiration (`ET0 * Kc`)                           |
| **`Act_ET`**                                          |                   |           | mm     | Total daily actual evapotranspiration                               |
| **`ET0`**                                             |                   |           | mm     | Daily reference evapotranspiration                                  |
| **`Groundw`**                                         |                   |           | m      | Groundwater table depth                                             |
| **`Recharge`**                                        |                   |           | mm     | Water flux at the model's lower boundary                            |
| **`WaterContent`**                                    | L                 |           | 0-1    | Relative available water: `(Mois - Pwp)/(Fc - Pwp)`                 |
| **`CapillaryRise`**                                   | L                 |           |        | Capillary water value                                               |
| **`PercolationRate`**                                 | L                 |           | mm d-1 | Percolation through the selected soil layer                         |
| **`Evapotranspiration`**                              |                   |           | mm     | Remaining evapotranspiration after evaporation of intercepted water |
| **`Evaporation`** or **`Evaporation_from_intercept`** |                   |           | mm     | Potential ET remaining after interception evaporation               |
| **`ETa/ETc`**                                         |                   |           |        | Ratio of actual to potential evapotranspiration `Act_ET/Pot_ET`     |
| **`Fc`**                                              | L                 |           | m3 m-3 | Field capacity                                                      |
| **`Pwp`**                                             | L                 |           | m3 m-3 | Permanent wilting point                                             |
| **`Ke`**                                              |                   |           |        | FAO-56 soil evaporation coefficient                                 |
| **`AWC`**                                             | L                 |           | m3 m-3 | Available water capacity: `Fc - Pwp`                                |
| **`Sat`**                                             | L                 |           | m3 m-3 | Saturated water content                                             |
| **`WaterFlux`**                                       | L                 |           | mm d-1 | Vertical water flux (positive = downward, negative = upward)        |
| **`Evaporated_from_surface`**                         |                   |           | mm     | Daily evaporation from surface water storage                        |

### Soil physical and thermal properties

| Output name    | (L)ayers/(O)rgans | Settable? | Unit    | Description                            |
|----------------|-------------------|-----------|---------|----------------------------------------|
| **`FrostD`**   |                   |           | m       | Frost front depth in soil              |
| **`ThawD`**    |                   |           | m       | Thaw front depth in soil               |
| **`SurfTemp`** |                   |           | °C      | Soil surface temperature               |
| **`STemp`**    | L                 |           | °C      | Soil temperature in the selected layer |
| **`Sand`**     | L                 |           | kg kg-1 | Soil sand content                      |
| **`Clay`**     | L                 |           | kg kg-1 | Soil clay content                      |
| **`Silt`**     | L                 |           | kg kg-1 | Soil silt content                      |
| **`Stone`**    | L                 |           | kg kg-1 | Soil stone content                     |
| **`pH`**       | L                 |           |         | Soil pH in the selected layer          |
| **`SoilpH`**   |                   |           |         | Soil pH in the first layer             |

### Soil carbon pools and dynamics

| Output name             | (L)ayers/(O)rgans | Settable? | Unit         | Description                                          |
|-------------------------|-------------------|-----------|--------------|------------------------------------------------------|
| **`SOC`**               | L                 |           | kg C kg-1    | Total soil organic carbon mass fraction              |
| **`SOC-X-Y`**           | L                 |           | g C m-2      | SOC stock in each selected layer                     |
| **`AOMf`**              | L                 |           | kg C m-3     | Total fast pool AOM carbon                           |
| **`AOMs`**              | L                 |           | kg C m-3     | Total slow pool AOM carbon                           |
| **`SMBf`**              | L                 |           | kg C m-3     | Fast pool soil microbial biomass carbon              |
| **`SMBs`**              | L                 |           | kg C m-3     | Slow pool soil microbial biomass carbon              |
| **`SOMf`**              | L                 |           | kg C m-3     | Fast pool soil organic matter carbon                 |
| **`SOMs`**              | L                 |           | kg C m-3     | Slow pool soil organic matter carbon                 |
| **`CBal`**              | L                 |           | kg C m-3 d-1 | Daily net carbon change                              |
| **`Co`**                | L                 |           | kg C kg-1    | Non-inert soil organic carbon mass fraction          |
| **`SMB-CO2-ER`**        | L                 |           | kg C m-3 d-1 | Soil microbial biomass CO<sub>2</sub> evolution rate |
| **`noOfAOMPools`**      |                   |           | count        | Number of AOM pools in the first soil layer          |
| **`CN_Ratio_AOM_Fast`** | L                 |           |              | Fast pool C:N ratio of the first AOM pool            |
| **`AOM_Fast`**          | L                 |           | kg C m-3     | Fast carbon pool of the first AOM pool               |
| **`AOM_Slow`**          | L                 |           | kg C m-3     | Slow carbon pool of the first AOM pool               |

### Soil nitrogen dynamics

| Output name        | (L)ayers/(O)rgans | Settable? | Unit           | Description                                                         |
|--------------------|-------------------|-----------|----------------|---------------------------------------------------------------------|
| **`NLeach`**       |                   |           | kg N ha-1 d-1  | Nitrogen leaching at `LeachingDepth`                                |
| **`NO3`**          | L                 | x         | kg N m-3       | Soil nitrate-N concentration (NO<sub>3</sub>-N)                     |
| **`Carb`**         | L                 | x         | kg N m-3       | Soil carbamide-N concentration (urea-N)                             |
| **`NH4`**          | L                 | x         | kg N m-3       | Soil ammonium-N concentration (NH<sub>4</sub>-N)                    |
| **`NO2`**          | L                 | x         | kg N m-3       | Soil nitrite-N concentration (NO<sub>2</sub>-N)                     |
| **`Nmin`**         | L                 |           | kg N ha-1 d-1  | N mineralisation or immobilisation magnitude                        |
| **`NetNmin`**      |                   |           | kg N ha-1 d-1  | Total `Nmin` across organic soil layers                             |
| **`Denit`**        |                   |           | kg N ha-1 d-1  | N loss through denitrification                                      |
| **`actnitrate`**   | L                 |           | kg N m-3 d-1   | Actual nitrification rate (N<sub>2</sub>O STICS module)             |
| **`N2O`**          |                   |           | kg N ha-1 d-1  | Total N<sub>2</sub>O-N production                                   |
| **`N2Onit`**       |                   |           | kg N ha-1 d-1  | N<sub>2</sub>O-N from nitrification (N<sub>2</sub>O STICS module)   |
| **`N2Odenit`**     |                   |           | kg N ha-1 d-1  | N<sub>2</sub>O-N from denitrification (N<sub>2</sub>O STICS module) |
| **`N`**            | L                 |           | kg N m-3       | Total mineral N concentration (`NO3 + NO2 + NH4`)                   |
| **`NH3`**          |                   |           | kg N ha-1 d-1  | Daily ammonia (NH<sub>3</sub>) volatilisation                       |
| **`OrgN`**         | L                 |           | kg N m-3       | Organic N concentration in the SMB, SOM, and AOM pools              |
| **`NO3conv`**      | L                 |           | kg N m-3       | Convective nitrate balance term                                     |
| **`NO3disp`**      | L                 |           | kg N m-3       | Dispersive nitrate balance term                                     |
| **`actammoxrate`** | L                 |           | kg N m-3 d-1   | Actual ammonium oxidation rate                                      |
| **`actdenitrate`** | L                 |           | kg N m-3 d-1   | Actual denitrification rate                                         |

### Crop management inputs and residue balances

| Output name                     | (L)ayers/(O)rgans | Settable? | Unit         | Description                                                                 |
|---------------------------------|-------------------|-----------|--------------|-----------------------------------------------------------------------------|
| **`Irrig`**                     |                   |           | mm           | Irrigation amount applied on the current day                                |
| **`NFert`**                     |                   |           | kg N ha-1    | Mineral fertiliser N applied on the current day                             |
| **`NOrgFert`**                  |                   |           | kg N ha-1    | Organic fertiliser N applied on the current day                             |
| **`SumNFert`**                  |                   |           | kg N ha-1    | Cumulative mineral fertiliser N for the current cultivation method          |
| **`SumNOrgFert`**               |                   |           | kg N ha-1    | Cumulative organic fertiliser N for the current cultivation method          |
| **`optCarbonExportedResidues`** |                   |           | kg DM ha-1   | Residues exported by the current optimal carbon-balance calculation         |
| **`optCarbonReturnedResidues`** |                   |           | kg DM ha-1   | Residues returned by the current optimal carbon-balance calculation         |
| **`humusBalanceCarryOver`**     |                   |           | Heq-NRW ha-1 | Humus balance carry-over from the latest optimal carbon-balance calculation |

### Ozone impacts and biogenic emissions

| Output name                         | (L)ayers/(O)rgans | Settable? | Unit                          | Description                                                                                                |
|-------------------------------------|-------------------|-----------|-------------------------------|------------------------------------------------------------------------------------------------------------|
| **`AtmO3`**                         |                   |           | ppb                           | Atmospheric ozone concentration used by the model                                                          |
| **`O3-short-damage`**               |                   |           |                               | Short-term carboxylation factor (`1` = no reduction, `0` = complete reduction)                             |
| **`O3-long-damage`**                |                   |           |                               | Long-term ozone factor controlling senescence onset (`1` = no ozone effect)                                |
| **`O3-WS-gs-reduction`**            |                   |           |                               | Water stress factor for stomatal conductance and ozone uptake (`1` = no reduction, `0` = complete closure) |
| **`O3-total-uptake`**               |                   |           | µmol O<sub>3</sub> m-2 ground | Cumulative ozone uptake by the current crop                                                                |
| **`guenther-isoprene-emission`**    |                   |           | µmol m-2 ground d-1           | Daily canopy isoprene emission from the Guenther model                                                     |
| **`guenther-monoterpene-emission`** |                   |           | µmol m-2 ground d-1           | Daily canopy monoterpene emission from the Guenther model                                                  |
| **`jjv-isoprene-emission`**         |                   |           | µmol m-2 ground d-1           | Daily canopy isoprene emission from the JJV model                                                          |
| **`jjv-monoterpene-emission`**      |                   |           | µmol m-2 ground d-1           | Daily canopy monoterpene emission from the JJV model                                                       |

---

## Simulation toggles

The top-level keys in `sim.json` control optional MONICA model processes. Toggle names are case-sensitive and must use JSON booleans (`true` or `false`).

```json
{
  "UseSecondaryYields": true,
  "NitrogenResponseOn": true,
  "WaterDeficitResponseOn": true,
  "EmergenceMoistureControlOn": true,
  "EmergenceFloodingControlOn": true,
  "FrostKillOn": true,

  "UseAutomaticIrrigation": false,
  "UseNMinMineralFertilisingMethod": false
}
```

### Overview

| Key                                   | Type    | Description                                                 |
|---------------------------------------|---------|-------------------------------------------------------------|
| **`UseSecondaryYields`**              | boolean | Enables exclusion of secondary yields from harvest residues |
| **`NitrogenResponseOn`**              | boolean | Enables nitrogen stress effects                             |
| **`WaterDeficitResponseOn`**          | boolean | Enables water stress effects                                |
| **`EmergenceMoistureControlOn`**      | boolean | Enables topsoil moisture control of emergence               |
| **`EmergenceFloodingControlOn`**      | boolean | Enables surface water control of emergence                  |
| **`FrostKillOn`**                     | boolean | Enables frost damage effects                                |
| **`UseAutomaticIrrigation`**          | boolean | Enables automatic irrigation                                |
| **`UseNMinMineralFertilisingMethod`** | boolean | Enables automatic NMin fertilisation                        |

### Crop response toggles

#### `NitrogenResponseOn`

`NitrogenResponseOn` controls whether nitrogen availability affects crop growth. When enabled, nitrogen shortage can reduce crop development and biomass growth. When disabled, crop growth is not reduced by nitrogen stress, but nitrogen uptake and soil nitrogen processes remain active. 

```json
{
  "NitrogenResponseOn": true
}
```

#### `WaterDeficitResponseOn`

`WaterDeficitResponseOn` controls whether water availability affects crop growth. When enabled, water deficit can reduce crop growth. When disabled, crop growth is not reduced by water stress, but soil water calculations, transpiration, and water uptake remain active. 

```json
{
  "WaterDeficitResponseOn": true
}
```

#### `EmergenceMoistureControlOn`

`EmergenceMoistureControlOn` controls whether topsoil moisture restricts crop emergence. When enabled, emergence progresses only if the topsoil is neither too dry nor above field capacity. When disabled, emergence progresses regardless of topsoil moisture.

```json
{
  "EmergenceMoistureControlOn": true
}
```

#### `EmergenceFloodingControlOn`

`EmergenceFloodingControlOn` controls whether standing surface water restricts crop emergence. When enabled, emergence progresses only if surface water storage is below `0.001` mm. When disabled, emergence progresses regardless of standing surface water.

```json
{
  "EmergenceFloodingControlOn": true
}
```

#### `FrostKillOn`

`FrostKillOn` controls whether frost damage affects crop growth. When enabled, exposure below the crop's lethal temperature threshold progressively reduces assimilation. When disabled, the frost damage response is skipped.

```json
{
  "FrostKillOn": true
}
```

### Secondary yield handling toggle

#### `UseSecondaryYields`

`UseSecondaryYields` controls whether secondary yields are removed from harvest residues. When enabled, both primary and secondary yields are excluded from residue biomass. When disabled, only the primary yield is excluded, leaving secondary yield biomass in the residues.

```json
{
  "UseSecondaryYields": true
}
```

### Automatic management toggles

Automatic management methods require both an enabling toggle and the corresponding parameter objects.

| Toggle                                | Parameter object                                    |
|---------------------------------------|-----------------------------------------------------|
| **`UseAutomaticIrrigation`**          | **`AutoIrrigationParams`**                          |
| **`UseNMinMineralFertilisingMethod`** | **`NMinUserParams`**, **`NMinFertiliserPartition`** |

#### Automatic irrigation

Automatic irrigation is enabled with `UseAutomaticIrrigation`. Its default is `false`. The parameters themselves are defined in `AutoIrrigationParams`.

When `amount` is greater than zero, MONICA applies that fixed amount and ignores `set_to_%nFC`. Otherwise, `set_to_%nFC` can refill the affected layers to the requested percentage of plant-available water capacity. The same depth, `calc_nFC_until_depth_m`, is used both to calculate the trigger and, in refill mode, to select the layers to refill.

Irrigation is possible only while a crop is growing and its current temperature sum lies within the inclusive range `HeatSumIrrigationStart` to `HeatSumIrrigationEnd`. These two parameters are defined in the active crop's cultivar configuration. If supplied, `startDate` and `stopDate` additionally restrict irrigation to an inclusive calendar-date range.

| Key                          | Default  | Unit         | Description                                                                                                          |
|------------------------------|----------|--------------|----------------------------------------------------------------------------------------------------------------------|
| **`UseAutomaticIrrigation`** | `false`  |              | Enables or disables automatic irrigation                                                                             |
| **`startDate`**              | unset    | ISO date     | First irrigation date. Absolute dates only.                                                                          |
| **`stopDate`**               | unset    | ISO date     | Last irrigation date. Absolute dates only.                                                                           |
| **`amount`**                 | unset    | mm           | Fixed irrigation amount. When greater than zero, `set_to_%nFC` is ignored.                                           |
| **`set_to_%nFC`**            | unset    | % nFC        | Refill target used when no positive fixed `amount` is configured.                                                    |
| **`threshold`**              | unset    | fraction nFC | Legacy trigger. Prefer `trigger_if_nFC_below_%`.                                                                     |
| **`trigger_if_nFC_below_%`** | unset    | % nFC        | Trigger irrigation when plant-available water is at or below this percentage.                                        |
| **`calc_nFC_until_depth_m`** | `0.3`    | m            | Depth over which plant-available water is evaluated. In refill mode, the same depth determines the layers to refill. |
| **`nitrateConcentration`**   | `0`      | mg dm-3      | Nitrate concentration in irrigation water. Place in `irrigationParameters`.                                          |
| **`sulfateConcentration`**   | `0`      | mg dm-3      | Sulfate concentration in irrigation water. Currently ignored.                                                        |

##### Fixed amount irrigation

When `amount` is greater than zero, MONICA applies that fixed amount whenever the trigger conditions are met. In this mode, `set_to_%nFC` is ignored.

```json
{
  "UseAutomaticIrrigation": true,
  "AutoIrrigationParams": {
    "startDate": "2024-05-01",
    "stopDate": "2024-08-31",
    "amount": [17, "mm"],
    "trigger_if_nFC_below_%": [50, "%"],
    "calc_nFC_until_depth_m": [0.3, "m"]
  }
}
```

This example applies 17 mm whenever plant-available water in the first 0.3 m is at or below 50%, from May 1 through August 31, inclusive.

##### Refill irrigation

If no positive fixed `amount` is configured, `set_to_%nFC` can be used to refill the affected soil layers to a target percentage of plant-available water capacity.

```json
{
  "UseAutomaticIrrigation": true,
  "AutoIrrigationParams": {
    "irrigationParameters": {
      "nitrateConcentration": [10, "mg dm-3"]
    },
    "trigger_if_nFC_below_%": [50, "%"],
    "set_to_%nFC": [100, "%"],
    "calc_nFC_until_depth_m": [0.5, "m"]
  }
}
```

This example refills the first 0.5 m to field capacity whenever plant-available water over that depth is at or below 50%. Irrigation water contains 10 mg dm-3 nitrate.

#### NMin mineral fertilisation

The automatic NMin method is enabled with `UseNMinMineralFertilisingMethod`.

```json
{
  "UseNMinMineralFertilisingMethod": true
}
```

The method requires application limits (`NMinUserParams`), fertiliser composition (`NMinFertiliserPartition`), and a calculation day for winter crops (`JulianDayAutomaticFertilising`).

```json
{
  "UseNMinMineralFertilisingMethod": true,
  "NMinUserParams": {"min": 40, "max": 120, "delayInDays": 10},
  "NMinFertiliserPartition": ["include-from-file", "mineral-fertilisers/AN.json"],
  "JulianDayAutomaticFertilising": 89
}
```

MONICA calculates a fertiliser recommendation from crop-specific nitrogen targets and the mineral nitrogen already present in the soil.

##### Application timing

The timing depends on the crop type:

- For non-winter crops, MONICA performs the NMin calculation when the crop is sown.
- For winter crops, MONICA performs it on `JulianDayAutomaticFertilising`, provided the crop is present on that day.

##### `NMinUserParams`

| Key               | Unit       | Description                                  |
|-------------------|------------|----------------------------------------------|
| **`min`**         | kg N ha-1  | Minimum fertiliser requirement               |
| **`max`**         | kg N ha-1  | Maximum fertiliser requirement               |
| **`delayInDays`** | days       | Delay before applying the amount above `max` |

In the example above, calculated requirements below 40 kg N ha-1 are skipped, up to 120 kg N ha-1 is applied immediately, and any surplus is applied after 10 days. If the topsoil is wetter than field capacity, the calculation is postponed.

##### `NMinFertiliserPartition`

`NMinFertiliserPartition` defines how the applied mineral nitrogen is divided among carbamide, ammonium, and nitrate. It can be supplied either by including an external file or by defining the fertiliser directly in `sim.json`.

###### External fertiliser definition

```json
{
  "NMinFertiliserPartition": ["include-from-file", "mineral-fertilisers/AN.json"]
}
```

###### Inline fertiliser definition

```json
{
  "NMinFertiliserPartition": {
    "id": "my AN",
    "name": "my very own ammonium nitrate variant",
    "Carbamid": 0,
    "NH4": 0.5,
    "NO3": 0.5
  }
}
```