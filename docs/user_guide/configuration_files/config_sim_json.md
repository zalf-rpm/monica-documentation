# The `sim.json` configuration file

The `sim.json` file contains simulation-specific information such as the start and end date (MONICA will select the appropriate climate data from `climate.csv`) and whether certain global toggles, such as nitrogen response or the use of secondary yields, are turned on or off.

The default soil discretisation consists of 20 layers, each 0.1 m thick. Both `NumberOfLayers` and `LayerThickness` are configurable, but changing them can affect parameterisation and output definitions that assume the defaults.

---

## Reading climate data (`climate.csv-options`)

Climate data are stored in `.csv` files. The `climate.csv-options` object in `sim.json` configures how they are read. 
The time range for a MONICA run is defined by the keys `start-date` and `end-date` inside the `climate.csv-options` JSON object. If these keys are not present, the time range is taken directly from `climate.csv`.

The key `no-of-climate-file-header-lines` defines how many lines will be skipped at the beginning of the file, as there may be no header (0), only column names (1), or multiple header lines (e.g., including units). The `csv-separator` key defines which character separates columns and values. In a **Comma Separated Values (CSV)** file, this is usually a `,`, but it could also be a tab `\t`, space, or another character.

MONICA accepts **Available Climate Data (ACD)** names for the climate input. These names are **case-sensitive**.

| ACD element     | Description                              | Example        | Unit       |
|-----------------|------------------------------------------|----------------|------------|
| **`day`**       | Day of year                              | **5**          |            |
| **`month`**     | Month of year                            | **11**         |            |
| **`year`**      | Year                                     | **2017**       |            |                                      
| **`iso-date`**  | ISO date format                          | **2017-11-05** |            |          
| **`de-date`**   | German date format                       | **05.11.2017** |            |               
| **`tmin`**      | Minimum daily temperature                | **-2**         | **°C**     |     
| **`tavg`**      | Average daily temperature                | **15.3**       | **°C**     |    
| **`tmax`**      | Maximum daily temperature                | **34.7**       | **°C**     |  
| **`precip`**    | Daily precipitation                      | **2.3**        | **mm**     |
| **`globrad`**   | Global radiation                         | **27.431**     | **MJ m-2** |   
| **`sunhours`**  | Hours of sunshine (better use `globrad`) | **8.5**        | **h**      | 
| **`wind`**      | Wind speed                               | **6.7**        | **m s-1**  |
| **`relhumid`**  | Relative humidity                        | **90.0**       | **%**      |
| **`skip`**      | Skip an existing element                 |                |            |

Unknown column headers are skipped automatically. The `skip` ACD can be used explicitly to ignore known elements, for example, if there are multiple columns for the same type of climate variable. 

To use any climate dataset without modifying the original file, column names can be **mapped** to the corresponding ACD names using the `header-to-acd-names` object. Its key/value pairs define these mappings.

Additionally, a mapping value can be a **JSON array** containing three elements: the ACD name, an arithmetic operation (`+`, `-`, `*`, `/`), and a number. The operation is applied to every value in that column. For example, values in `J cm-2` are converted to `MJ m-2` by multiplying by `0.01`.

```json
{
"climate.csv-options": {
  "__given the start and end date, monica will run just this time range, else the full time range given by supplied climate data": "",
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

---

## Outputting results from MONICA

All results from a MONICA run can be written to a **CSV** file. Which results are written, and how they are aggregated, is configured in `sim.json` under the key `output`.

The following table describes several available options which can be set:

The following keys are direct members of `output`:

| Key                  | Description                                                                                                             |
|----------------------|-------------------------------------------------------------------------------------------------------------------------|
| **`path-to-output`** | Directory in which output files are created                                                                             |
| **`file-name`**      | Name of the output file                                                                                                 |
| **`write-file?`**    | Write to a file when `true`, otherwise the commandline runner writes output to standard output. The default is `false`. |
| **`events`**         | Alternating event definitions and lists of requested outputs                                                            |

CSV formatting is configured in the nested `output.csv-options` object:

| Key                            | Description                                                                                            |
|--------------------------------|--------------------------------------------------------------------------------------------------------|
| **`include-header-row`**       | Include a row containing output names                                                                  |
| **`include-units-row`**        | Include a row containing units                                                                         |
| **`include-aggregation-rows`** | Include two diagnostic rows: MONICA's interpreted output expressions and the original JSON expressions |
| **`csv-separator`**            | Column separator, for example `","`                                                                    |

Because omitted JSON booleans evaluate to `false` in the commandline runner, set the three `include-*` options explicitly when those rows are required.

---

### Defining events

The `events` key defines the list of MONICA results to be written to the output. Each event is specified as a **pair**, consisting of an **event definition** followed by the corresponding **list of outputs**. 

Events are defined inside a JSON array and can take one of the following forms: 

* A simple string such as `"daily"`, 
* A JSON array, or 
* A full JSON object that explicitly describes the conditions under which outputs should be generated. 

Strings and JSON arrays serve as **shortcuts** for more complex JSON objects that describe the triggering conditions for generating output.

The event structure allows specification of:

1. When to **start** and **end** outputting results (`start` and `end` keys)
2. Time ranges to aggregate results (`from` and `to` keys)
3. Points **at** which time or condition to write results (non-aggregated)
4. Periods **while** conditions are true (for conditional aggregation)

ISO dates can include placeholders (`x`). Every possible value for the year, month, or day of the placeholder is used. For example, `xxxx-xx-15` means every 15th day of each month.

#### Available events

In addition to date patterns, each workstep generates an event with the same name (e.g., `Sowing`, `Harvest`, `AutomaticSowing`, `SetValue`, etc.).

Common events include:

* **run-started**
* **Stage-1**, **Stage-2**, ... 
* **emergence**
* **anthesis**
* **cereal-stem-elongation**
* **maturity**
* **Sowing**, **AutomaticSowing**, **Cutting**, etc.

The following table shows examples of some event expressions, their shortcut forms, and their expanded equivalents:

| Shortcut                     | Expanded form                                                          | Meaning                                                                                                                                                          |
|------------------------------|------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                              | `{"start": "xxxx-05-01", "end": "xxxx-07-31", "at": "xxxx-xx-15"}`     | Output results on the 15th of each month from May to July &ndash; daily results will be written                                                                  |
|                              | `{"from": "Sowing", "to": "anthesis", "while": ["ETa/ETc", "<", 0.4]}` | Output aggregated results from the `Sowing` event until (and including) the `anthesis` event, but only while actual to potential evapotranspiration is below 0.4 |
| `"xxxx-03-31"`               | `{"at": "xxxx-03-31"}`                                                 | Write results every March 31st                                                                                                                                   |
| `"daily"`                    | `{"at": "xxxx-xx-xx"}`                                                 | Write daily results                                                                                                                                              |
| `"monthly"`                  | `{"from": "xxxx-xx-01", "to": "xxxx-xx-31"}`                           | Write monthly aggregated results                                                                                                                                 |
| `"yearly"`                   | `{"from": "xxxx-01-01", "to": "xxxx-12-31"}`                           | Write yearly aggregated results                                                                                                                                  |
| `"run"`                      | `{"from": <start-date>, "to": <end-date>}`                             | Write results aggregated over the entire simulation run                                                                                                          |
| `"crop"`                     | `{"from": "Sowing", "to": "Harvest"}`                                  | Write results aggregated over the cropping period                                                                                                                |
| `["while", "Stage", "=", 5]` | `{"while": ["Stage", "=", 5]}`                                         | Write results aggregated only while `Stage` equals 5                                                                                                             |
| `["at", "Stage", "=", 2]`    | `{"at": ["Stage", "=", 2]}`                                            | Write results daily when `Stage` equals 2                                                                                                                        |
| `[["Mois", 1], "<", 0.5]`    | `{"at": [["Mois", 1], "<", 0.5]}`                                      | Write results daily when the soil moisture in the first layer is below 0.5                                                                                       |
| `"Sowing"`                   | `{"at": "Sowing"}`                                                     | Write results at sowing time                                                                                                                                     |

As shown in the table above, simple comparison expressions can be used in events. The available operators are `<`, `<=`, `=`, `>=`, `>`. Output expressions such as `"Stage"` or `["Mois", 1]`, as well as numeric values (e.g., **1**), can be used on both sides of these operators. Event and output names are case-sensitive.

---

## List of outputs

After defining events (i.e., when to output), you must specify **what** to output. Some output expressions already appeared in the previous table, such as `"Stage"` and `["Mois", 1]`. `"Stage"` outputs the current development stage of the plant. With the default layer thickness of 0.1 m, `["Mois", 1]` outputs soil moisture in the first 10 cm of soil.

MONICA supports three categories of outputs:

1. **Scalar values** (e.g., `"Stage"`)
2. **Array values** for soil layers (e.g., `"Mois"`, with one value per configured layer)
3. **Array values** for crop organs (e.g., `["OrgBiom", "Root"]`)

By default, MONICA uses 20 soil layers of 0.1 m each (total depth 2 m). Output layer numbers are one-based. When outputs refer to layers, they can specify:

* a single layer (e.g., `15` for the 15th layer, spanning 1.4-1.5 m with the default discretisation),
* a range of layers (e.g., `[1, 5]` for layers 1 to 5, corresponding to 0 to 0.5 m depth),
* or a crop organ (e.g., `"Root"`, `"Leaf"`, `"Shoot"`, `"Fruit"`, `"Struct"`, `"Sugar"`).

Additionally, the user must tell MONICA whether ranges of values (in arrays) should be output as multiple scalars or aggregated into a single value. If aggregation is applied, the aggregation method must also be specified. 

The available aggregation operations are: `AVG`, `MEDIAN`, `SUM`, `MIN`, `MAX`, `FIRST`, `LAST`, `NONE`. 

Aggregation can occur on a daily basis to aggregate soil layers into a single scalar value (the **default** operation for layer aggregation is `NONE`) or over a time range, e.g. to aggregate monthly values, where the **default** operation in this case is `AVG`.

| Aggregation reason                      | Default operation  | Meaning                                                                                                                                                                                |
|-----------------------------------------|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Aggregate soil layers into scalar value | **`NONE`**         | If no aggregation operation is specified, the selected range of layer values will be output as individual values.                                                                      |
| Aggregate time range                    | **`AVG`**          | If an event defines an aggregation time range and no aggregation operation is specified (value 2 or 3 in the **JSON array**), the temporarily collected daily values will be averaged. |

To understand aggregation operations, imagine that all values to be aggregated (e.g., `Runoff`) are stored temporarily in a list during the `from`/`to` period . After the `to` time range ends, the specified aggregation operation is applied to this list. For example, `AVG` will calculate the mean of all values, while `FIRST` will return the first value in the list &ndash; the value from the start (`from` day) of the aggregation period.

The user specifies an output either by simply defining the output name (e.g., `"Stage"`, `"Crop"`, or `"Date"`) or by using a **JSON array**, where the first element of the array is the aforementioned output name. It is also possible to append a display name to the output name, separated by the character `|`. This display name will be used in the output instead of the result name. For example, `"DOY|MatDOY"` will output `MatDOY` instead of `DOY`, which may be more descriptive. 

The second element in the array can specify one of the following:

* the selected soil layer, 
* a range of layers, 
* a crop organ (for array values), or 
* an aggregation operation (for scalar values). 

If omitted, the aggregation operation defaults to `NONE`. 

For array outputs, a third element can define the aggregation operation. The second element can be:

* a single number (e.g., the soil layer number), 
* a string describing the crop organ, or 
* an array that describes a layer range (for soil layers only). 

In the latter case, the first value of the array is the starting layer, the second the ending layer (inclusive), and an optional third value specifies the aggregation operation. If the range includes an aggregation operation, MONICA will apply it and store a single aggregated value each time output is requested. Otherwise, individual layer values will be listed in the output, each labeled with the result name and the corresponding layer number (e.g., `Mois_1`, `Mois_2`, etc.). 

The following examples show the possible variations:

| Output definition                        | Meaning                                                                                                                                                                                               |
|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `"Date"`                                 | Output the ISO date (e.g., `2017-01-17`)                                                                                                                                                              |                                                                                                                                                                         
| `["Year", "LAST"]`                       | Output the year at the `to` time &ndash; the year at the end of the aggregation period                                                                                                                |                                                                                                                                                 
| `["PercolationRate\|WDrain", 15, "SUM"]` | Output the sum of percolation rates in soil layer 15 (1.5 m) during the aggregation period, and rename it in the output as `WDrain` (`SUM` requires an event to specify the aggregation time range.)  | 
| `["Mois", [1, 20]]`                      | Output the daily soil moisture of all 20 soil layers. The outputs will be labeled "Mois_1", "Mois_2", etc.                                                                                            |                                                                                                          
| `["OrgBiom", "Leaf"]`                    | Output the daily organic biomass of leaves                                                                                                                                                            |                                                                                                                                                         
| `["SOC", [1, 3, "AVG"]]`                 | Output the daily soil organic carbon as a single value averaged across the first three soil layers                                                                                                    |                                                                                                             
| `["Precip", "SUM"]`                      | Output the sum of precipitation values within a given aggregation time range                                                                                                                          |                                                                                                                                       

In the generated output file, the order of the results follows the sequence defined in the `events` list and can be relied upon. 

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

The table below documents commonly used output variables. The most up-to-date list is defined in [**`build-output.cpp`**](https://github.com/zalf-lsa/monica/blob/master/src/io/build-output.cpp).

Output names are case-sensitive. Unknown names are ignored, so check the generated header when adding a new output definition.

```C++
build({id++, "Date", "", "output current date"},
...
build({id++, "TraDef", "0;1", "TranspirationDeficit"},
```

In this example, `Date` and `TraDef` are allowed output names. Additionally, the code lists the expected units of measure (if defined) and provides a description for each output variable.

| Output name                         | (L)ayers/(O)rgans | Settable?                     | Unit                                | Description                                                                                                                                                                   |
|-------------------------------------|-------------------|-------------------------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`Count`**                         |                   |                               |                                     | Output constant value 1 (can be used for counting records or events)                                                                                                          |
| **`CM-count`**                      |                   |                               |                                     | Order number of the current cultivation method                                                                                                                                |
| **`Date`**                          |                   |                               |                                     | Current date                                                                                                                                                                  |
| **`days-since-start`**              |                   |                               |                                     | Number of days since simulation start                                                                                                                                         |
| **`DOY`**                           |                   |                               |                                     | Current day of year                                                                                                                                                           |
| **`Month`**                         |                   |                               |                                     | Current month number (1-12)                                                                                                                                                   |
| **`Year`**                          |                   |                               |                                     | Current year                                                                                                                                                                  |
| **`Crop`**                          |                   |                               |                                     | Crop name                                                                                                                                                                     |
| **`TraDef`**                        |                   |                               | 0;1                                 | Transpiration deficit index (`1`= no stress, `0` = maximum stress)                                                                                                            |
| **`Tra`**                           |                   |                               | mm                                  | Actual transpiration                                                                                                                                                          |
| **`NDef`**                          |                   |                               | 0;1                                 | Nitrogen deficit index, indicates N availability (`1` = no stress, `0` = no N available)                                                                                      | 
| **`HeatRed`**                       |                   |                               | 0;1                                 | Heat stress reductor                                                                                                                                                          |
| **`FrostRed`**                      |                   |                               | 0;1                                 | Frost stress reductor                                                                                                                                                         |
| **`OxRed`**                         |                   |                               | 0;1                                 | Oxygen deficit reductor                                                                                                                                                       |
| **`Stage`**                         |                   | x                             | 1-based stage number                | Phenological development stage                                                                                                                                                |
| **`TempSum`**                       |                   |                               | °Cd                                 | Accumulated temperature sum                                                                                                                                                   |
| **`VernF`**                         |                   |                               | 0;1                                 | Vernalisation factor                                                                                                                                                          |
| **`DaylF`**                         |                   |                               | 0;1                                 | Daylength factor                                                                                                                                                              |
| **`IncRoot`**                       |                   |                               | kg CH2O ha-1 d-1                    | Growth increment of root                                                                                                                                                      |
| **`IncLeaf`**                       |                   |                               | kg CH2O ha-1 d-1                    | Growth increment of leaf                                                                                                                                                      |
| **`IncShoot`**                      |                   |                               | kg CH2O ha-1 d-1                    | Growth increment of shoot                                                                                                                                                     |
| **`IncFruit`**                      |                   |                               | kg CH2O ha-1 d-1                    | Growth increment of fruit                                                                                                                                                     |
| **`RelDev`**                        |                   |                               | 0;1                                 | Relative total development                                                                                                                                                    |
| **`LT50`**                          |                   |                               | °C                                  | Crop's lethal temperature at which 50% of plants die                                                                                                                          |
| **`AbBiom`**                        |                   |                               | kg DM ha-1                          | Aboveground biomass                                                                                                                                                           |
| **`OrgBiom`**                       | O                 |                               | kg DM ha-1                          | Biomass of individual crop organs (e.g, Root, Leaf, Shoot, Fruit)                                                                                                             |
| **`Yield`**                         |                   |                               | kg DM ha-1                          | Current primary crop yield                                                                                                                                                    |
| **`sumExportedCutBiomass`**         |                   |                               | kg DM ha-1                          | Total exported biomass across all cuts for the current crop                                                                                                                   |
| **`exportedCutBiomass`**            |                   |                               | kg DM ha-1                          | Exported biomass from the current crop at the current cut                                                                                                                     |
| **`sumResidueCutBiomass`**          |                   |                               | kg DM ha-1                          | Total residue biomass across all cuts for the current crop                                                                                                                    |
| **`residueCutBiomass`**             |                   |                               | kg DM ha-1                          | Residue biomass from the current crop at the current cut                                                                                                                      |
| **`GroPhot`**                       |                   |                               | kg CH2O ha-1 d-1                    | Gross photosynthesis rate per hectare                                                                                                                                         |
| **`NetPhot`**                       |                   |                               | kg CH2O ha-1 d-1                    | Net photosynthesis per hectare                                                                                                                                                |
| **`MaintR`**                        |                   |                               | kg CH2O ha-1 d-1                    | Maintenance respiration rate per hectare (from AGROSIM)                                                                                                                       |
| **`GrowthR`**                       |                   |                               | kg CH2O ha-1 d-1                    | Growth respiration rate per hectare (from AGROSIM)                                                                                                                            |
| **`StomRes`**                       |                   |                               | s m-1                               | Stomata resistance                                                                                                                                                            |
| **`Height`**                        |                   |                               | m                                   | Crop height                                                                                                                                                                   |
| **`LAI`**                           |                   |                               | m2 m-2                              | Leaf Area Index                                                                                                                                                               |
| **`RootDep`**                       |                   |                               | layer#                              | Rooting depth                                                                                                                                                                 |
| **`EffRootDep`**                    |                   |                               | m                                   | Depth of the maximum active and effective root                                                                                                                                |
| **`TotBiomN`**                      |                   |                               | kg N ha-1                           | Total nitrogen content in biomass                                                                                                                                             |
| **`AbBiomN`**                       |                   |                               | kg N ha-1                           | Aboveground nitrogen content in biomass                                                                                                                                       |
| **`SumNUp`**                        |                   |                               | kg N ha-1                           | Cumulative actual nitrogen uptake by the crop                                                                                                                                 |
| **`ActNup`**                        |                   |                               | kg N ha-1                           | Daily actual crop N uptake                                                                                                                                                    |
| **`PotNup`**                        |                   |                               | kg N ha-1                           | Daily potential crop N uptake/demand                                                                                                                                          |
| **`NFixed`**                        |                   |                               | kg N ha-1                           | Nitrogen input by the crop via atmospheric fixation                                                                                                                           |
| **`Target`**                        |                   |                               | kg N kgDM-1                         | Target nitrogen concentration                                                                                                                                                 |
| **`CritN`**                         |                   |                               | kg N ha-1                           | Critical nitrogen concentration                                                                                                                                               |
| **`AbBiomNc`**                      |                   |                               | kg N ha-1 (kg ha-1)-1               | Nitrogen concentration in aboveground biomass                                                                                                                                 |
| **`Nstress`**                       |                   |                               | -                                   | Nitrogen stress index, indicates N availability (`1` = no stress, `0` = no N available)                                                                                       |
| **`YieldNc`**                       |                   |                               | kg N kgDM-1                         | Nitrogen concentration in primary yield                                                                                                                                       |
| **`Protein`**                       |                   |                               | kg kg-1                             | Raw protein concentration                                                                                                                                                     |
| **`NPP`**                           |                   |                               | kg C ha-1 d-1                       | Net Primary Production (NPP)                                                                                                                                                  |
| **`NPP-Organs`**                    | O                 |                               | kg C ha-1 d-1                       | Organ-specific Net Primary Production (NPP)                                                                                                                                   |
| **`GPP`**                           |                   |                               | kg C ha-1 d-1                       | Gross Primary Production (GPP)                                                                                                                                                |
| **`Ra`**                            |                   |                               | kg C ha-1 d-1                       | Autotrophic respiration                                                                                                                                                       |
| **`Ra-Organs`**                     | O                 |                               | kg C ha-1 d-1                       | Organ-specific autotrophic respiration                                                                                                                                        |
| **`Mois`**                          | L                 | x                             | m3 m-3                              | Soil moisture content per layer                                                                                                                                               |
| **`Irrig`**                         |                   |                               | mm                                  | Irrigation amount applied                                                                                                                                                     |
| **`Infilt`**                        |                   |                               | mm                                  | Amount of water that infiltrates into top soil layer                                                                                                                          |
| **`Surface`**                       |                   |                               | mm                                  | Surface water storage                                                                                                                                                         |
| **`RunOff`**                        |                   |                               | mm                                  | Surface water runoff for the current day                                                                                                                                      |
| **`SnowD`**                         |                   |                               | mm                                  | Snow depth                                                                                                                                                                    |
| **`FrostD`**                        |                   |                               | m                                   | Frost front depth in soil                                                                                                                                                     |
| **`ThawD`**                         |                   |                               | m                                   | Thaw front depth in soil                                                                                                                                                      |
| **`PASW`**                          | L                 |                               | m3 m-3                              | Plant available soil water per layer                                                                                                                                          |
| **`SurfTemp`**                      |                   |                               | °C                                  | Soil surface temperature                                                                                                                                                      |
| **`STemp`**                         | L                 |                               | °C                                  | Soil temperature by layer                                                                                                                                                     |
| **`Act_Ev`**                        |                   |                               | mm                                  | Actual evaporation                                                                                                                                                            |
| **`Pot_ET`**                        |                   |                               | mm                                  | Potential evapotranspiration = ET0 * Kc = the plants water use                                                                                                                |
| **`Act_ET`**                        |                   |                               | mm                                  | Actual evapotranspiration = actual transpiration + actual evaporation + evaporation from interception + evaporation from surface                                              |
| **`ET0`**                           |                   |                               | mm                                  | Reference evapotranspiration                                                                                                                                                  |
| **`Kc`**                            |                   |                               |                                     | Plant coefficient to calculate with ET0 the plants water use (ET0 * Kc)                                                                                                       |
| **`AtmCO2`**                        |                   |                               | ppm                                 | Atmospheric CO<sub>2</sub> concentration in the atmosphere                                                                                                                    |
| **`Groundw`**                       |                   |                               | m                                   | Groundwater table depth                                                                                                                                                       |
| **`Recharge`**                      |                   |                               | mm                                  | Groundwater recharge                                                                                                                                                          |
| **`NLeach`**                        |                   |                               | kg N ha-1                           | Nitrogen leaching at the configured `LeachingDepth`                                                                                                                           |
| **`NO3`**                           | L                 | x                             | kg N m-3                            | Soil nitrate (NO<sub>3</sub>) per layer                                                                                                                                       |
| **`Carb`**                          | L                 | x                             | kg N m-3                            | Soil carbamide per layer                                                                                                                                                      |
| **`NH4`**                           | L                 | x                             | kg N m-3                            | Soil ammonium (NH<sub>4</sub>) per layer                                                                                                                                      |
| **`NO2`**                           | L                 | x                             | kg N m-3                            | Soil nitrite (NO<sub>2</sub>) per layer                                                                                                                                       |
| **`SOC`**                           | L                 |                               | kg C kg-1                           | Soil organic carbon content per layer                                                                                                                                         |
| **`SOC-X-Y`**                       | L                 |                               | g C m-2                             | Produces a stock per selected layer. It represent the stock between X and Y only when the layer range is aggregated with `SUM`. (SOC * bulk density * layer thickness * 1000) |
| **`AOMf`**                          | L                 |                               | kg C m-3                            | Sum of added organic matter (AOM) fast pool carbon content                                                                                                                    |
| **`AOMs`**                          | L                 |                               | kg C m-3                            | Sum of added organic matter (AOM) slow pool carbon content                                                                                                                    |
| **`SMBf`**                          | L                 |                               | kg C m-3                            | Soil microbial biomass (SMB) fast pool carbon content                                                                                                                         |
| **`SMBs`**                          | L                 |                               | kg C m-3                            | Soil microbial biomass (SMB) slow pool carbon content                                                                                                                         |
| **`SOMf`**                          | L                 |                               | kg C m-3                            | Soil organic matter (SOM) fast pool carbon content                                                                                                                            |
| **`SOMs`**                          | L                 |                               | kg C m-3                            | Soil organic matter (SOM) slow pool carbon content                                                                                                                            |
| **`CBal`**                          | L                 |                               | kg C m-3                            | Carbon balance per layer                                                                                                                                                      |
| **`Nmin`**                          | L                 |                               | kg N ha-1 d-1                       | Net nitrogen mineralisation rate per layer                                                                                                                                    |
| **`NetNmin`**                       |                   |                               | kg N ha-1                           | Daily net mineralisation summed over the organic soil layers                                                                                                                  |
| **`Denit`**                         |                   |                               | kg N ha-1                           | Daily nitrogen loss through denitrification                                                                                                                                   |
| **`actnitrate`**                    | L                 |                               | kg N m-3 d-1                        | N production rate resulting from nitrification (N<sub>2</sub>O STICS module)                                                                                                  |
| **`N2O`**                           |                   |                               | kg N ha-1                           | Daily total N<sub>2</sub>O-N production                                                                                                                                       |
| **`N2Onit`**                        |                   |                               | kg N ha-1                           | N<sub>2</sub>O produced through nitrification (N<sub>2</sub>O STICS module)                                                                                                   |
| **`N2Odenit`**                      |                   |                               | kg N ha-1                           | N<sub>2</sub>O produced through denitrification (N<sub>2</sub>O STICS module)                                                                                                 |
| **`SoilpH`**                        |                   |                               |                                     | pH of the first soil layer only. Use `pH` for layer-selectable values.                                                                                                        |
| **`NEP`**                           |                   |                               | kg C ha-1 d-1                       | Net Ecosystem Production (NEP)                                                                                                                                                |
| **`NEE`**                           |                   |                               | kg C ha-1 d-1                       | Net Ecosystem Exchange (NEE)                                                                                                                                                  |
| **`Rh`**                            |                   |                               | kg C ha-1 d-1                       | Daily decomposer respiration                                                                                                                                                  |
| **`Tmin`**                          |                   |                               | °C                                  | Minimum daily air temperature                                                                                                                                                 |
| **`Tavg`**                          |                   |                               | °C                                  | Mean daily air temperature                                                                                                                                                    |
| **`Tmax`**                          |                   |                               | °C                                  | Maximum daily air temperature                                                                                                                                                 |
| **`Precip`**                        |                   |                               | mm                                  | Precipitation                                                                                                                                                                 |
| **`Wind`**                          |                   |                               | m s-1                               | Wind speed                                                                                                                                                                    |
| **`Globrad`**                       |                   |                               | MJ m-2                              | Global radiation                                                                                                                                                              |
| **`Relhumid`**                      |                   |                               | %                                   | Relative humidity                                                                                                                                                             |
| **`Sunhours`**                      |                   |                               | h                                   | Sunshine duration                                                                                                                                                             |
| **`BedGrad`**                       |                   |                               | 0;1                                 | Fractional soil coverage                                                                                                                                                      |
| **`N`**                             | L                 |                               | kg N m-3                            | Mineral nitrogen content in soil                                                                                                                                              |
| **`Co`**                            | L                 |                               | kg C m-3                            | Soil organic carbon concentration per soil layer                                                                                                                              |
| **`NH3`**                           |                   |                               | kg N ha-1                           | Ammonia (NH<sub>3</sub>) volatilisation                                                                                                                                       |
| **`NFert`**                         |                   |                               | kg N ha-1                           | Daily nitrogen fertiliser input                                                                                                                                               |
| **`WaterContent`**                  | L                 |                               | 0;1                                 | Fraction of usable field capacity: PASW/(FC-PWP)                                                                                                                              |
| **`CapillaryRise`**                 | L                 |                               | mm                                  | Capillary rise                                                                                                                                                                |
| **`PercolationRate`**               | L                 |                               | mm                                  | Percolation rate                                                                                                                                                              |
| **`SMB-CO2-ER`**                    | L                 |                               | kg C m-3 d-1                        | Soil microbial biomass (SMB) CO<sub>2</sub> evolution rate                                                                                                                    |
| **`Evapotranspiration`**            |                   |                               | mm                                  | Remaining evapotranspiration after evaporation of intercepted water                                                                                                           |
| **`Evaporation`**                   |                   |                               | mm                                  | Evaporation from intercepted water                                                                                                                                            |
| **`ETa/ETc`**                       |                   |                               |                                     | Ratio of actual to potential evapotranspiration `Act_ET/Pot_ET` (actual evapotranspiration/potential evapotranspiration)                                                      |
| **`Transpiration`**                 |                   |                               | mm                                  | Actual transpiration                                                                                                                                                          |
| **`GrainN`**                        |                   |                               | kg N ha-1                           | Current fruit organ biomass nitrogen content                                                                                                                                  |
| **`Fc`**                            | L                 |                               | m3 m-3                              | Field capacity                                                                                                                                                                |
| **`Pwp`**                           | L                 |                               | m3 m-3                              | Permanent wilting point                                                                                                                                                       |
| **`Nresid`**                        |                   |                               | kg N ha-1                           | Nitrogen content in crop residues                                                                                                                                             |
| **`Sand`**                          | L                 |                               | kg kg-1                             | Soil sand content                                                                                                                                                             |
| **`Clay`**                          | L                 |                               | kg kg-1                             | Soil clay content                                                                                                                                                             |
| **`Silt`**                          | L                 |                               | kg kg-1                             | Soil silt content                                                                                                                                                             |                                                                                     
| **`Stone`**                         | L                 |                               | kg kg-1                             | Soil stone content                                                                                                                                                            |                                                                                 
| **`pH`**                            | L                 |                               |                                     | Soil pH per layer                                                                                                                                                             |
| **`rootDensity`**                   | L                 |                               | m root m-3 soil (equivalent to m-2) | Root density in soil layer                                                                                                                                                    |                                                                
| **`rootingZone`**                   |                   |                               | layer number                        | Current number of layers in the rooting zone                                                                                                                                  |
| **`PotTraDef`**                     |                   |                               | mm                                  | Potential transpiration deficit before redistribution of water uptake among rooted layers                                                                                     |
| **`ActTraDef`**                     |                   |                               | mm                                  | Actual transpiration deficit remaining after redistribution of water uptake among rooted layers                                                                               |
| **`TraRed`**                        |                   |                               | mm                                  | Transpiration reduction calculated during the layer water-uptake routine                                                                                                      |
| **`Ass`**                           |                   |                               | kg CH2O ha-1 d-1                    | Daily assimilated remaining after environmental stress factors and respiration losses                                                                                         |
| **`RootNDef`**                      |                   |                               | 0;1                                 | Root nitrogen uptake reduction factor. `1` means no limitation and `0` means maximum limitation.                                                                              |
| **`TimeUnderAnoxia`**               |                   |                               | d                                   | Number of consecutive days the crop has experienced anoxic soil conditions                                                                                                    |
| **`OrgGreenBiom`**                  | O                 |                               | kg DM ha-1                          | Current green biomass of the selected crop organ                                                                                                                              |
| **`SecondaryYield`**                |                   |                               | kg DM ha-1                          | Current secondary crop yield                                                                                                                                                  |
| **`ActNupLayer`**                   | L                 |                               | kg N ha-1                           | Daily actual crop N uptake from the selected soil layer                                                                                                                       |
| **`RootWaUptak`**                   | L                 |                               | mm                                  | Actual transpiration water extracted from the selected soil layer. Despite its name, the getter returns layer transpiration.                                                  |
| **`YieldN`**                        |                   |                               | kg N ha-1                           | Nitrogen content of the current primary crop yield                                                                                                                            |
| **`rootNConcentration`**            |                   |                               | kg N kg DM-1                        | Nitrogen concentration of crop root biomass                                                                                                                                   |
| **`LightInterception1`**            |                   |                               | fraction                            | Fraction of radiation intercepted by a sole crop or by the upper canopy layer of the taller crop in an intercrop                                                              |
| **`LightInterception2`**            |                   |                               | fraction                            | Fraction of radiation intercepted by the lower canopy layer of the taller crop in an intercrop                                                                                |
| **`Kcb`**                           |                   |                               |                                     | Daily basal crop coefficient used by the FAO-56 dual-crop-coefficient calculation                                                                                             |
| **`Ke`**                            |                   |                               |                                     | Daily soil evaporation coefficient used by the FAO-56 dual-crop-coefficient calculation                                                                                       |
| **`optCarbonExportedResidues`**     |                   |                               | kg DM ha-1                          | Portion of crop residues exported according to the optimal carbon-balance calculation                                                                                         |
| **`optCarbonReturnedResidues`**     |                   |                               | kg DM ha-1                          | Portion of crop residues returned to the soil according to the optimal carbon-balance calculation                                                                             |
| **`humusBalanceCarryOver`**         |                   |                               | Heq-NRW ha-1                        | Humus equivalent balance carried over by the optimat carbon-balance calculation                                                                                               |
| **`AWC`**                           | L                 |                               | m3 m-3                              | Available water capacity of the selected layer: field capacity - permanent wilting point                                                                                      |
| **`Sat`**                           | L                 |                               | m3 m-3                              | Volumetric water content at saturation for the selected soil layer                                                                                                            |
| **`WaterFlux`**                     | L                 |                               | mm d-1                              | Vertical water flux associated with the selected soil layer. The sign indicates flow direction according to MONICA's internal convention.                                     |
| **`OrgN`**                          | L                 |                               | kg N m-3                            | Total organic N concentration in the selected layer, calculated from the SMB, SOM, and AOM carbon pools and their C:N ratios                                                  |
| **`NO3conv`**                       | L                 |                               | kg N m-3 d-1                        | Convective contribution to the daily change in nitrate concentration in the selected layer                                                                                    |
| **`NO3disp`**                       | L                 |                               | kg N m-3 d-1                        | Dispersive contribution to the daily change in nitrate concentration in the selected layer                                                                                    |
| **`noOfAOMPools`**                  |                   |                               | count                               | Number of AOM pools currently present in the first soil layer                                                                                                                 |
| **`CN_Ratio_AOM_Fast`**             | L                 |                               |                                     | C:N ratio of the fast pool of the first AOM pool in the selected layer. Returns `0` when no AOM pool exists.                                                                  |
| **`AOM_Fast`**                      | L                 |                               | kg C m-3                            | Carbon concentration in the fast pool of the first AOM pool in the selected layer. Unlike `AOMf`, this does not sum all AOM pools.                                            |
| **`AOM_Slow`**                      | L                 |                               | kg C m-3                            | Carbon concentration in the slow pool of the first AOM pool in the selected layer. Unlike `AOMs`, this does not sum all AOM pools.                                            |
| **`actammoxrate`**                  | L                 |                               | kg N m-3 d-1                        | Actual ammonia oxidation rate in the selected organic soil layer                                                                                                              |
| **`actdenitrate`**                  | L                 |                               | kg N m-3 d-1                        | Actual denitrification rate in the selected organic soil layer                                                                                                                |
| **`NOrgFert`**                      |                   |                               | kg N ha-1                           | Organic fertiliser N applied on the current day                                                                                                                               |
| **`SumNFert`**                      |                   |                               | kg N ha-1                           | Cumulative mineral fertiliser N applied during the current cropping period                                                                                                    |
| **`SumNOrgFert`**                   |                   |                               | kg N ha-1                           | Cumulative organic fertiliser N applied during the current cropping period                                                                                                    |
| **`Act_Trans`**                     |                   |                               | mm                                  | Daily actual crop transpiration. Alias of `Tra` and `Transpiration`, with different rounding precision.                                                                       |
| **`Evaporated_from_surface`**       |                   |                               | mm                                  | Water evaporated from surface water storage during the current day                                                                                                            |
| **`Evaporated_from_intercept`**     |                   |                               | mm                                  | Water evaporated from intercepted canopy water during the current day. Alias of `Evaporation`.                                                                                |
| **`AtmO3`**                         |                   |                               | ppb                                 | Atmospheric ozone concentration used during the current simulation step                                                                                                       |
| **`Tmax>=40`**                      |                   |                               | 0 or 1                              | Returns `1` when the current daily maximum air temperature is at least 40 °C, otherwise returns `0`                                                                           |
| **`O3-short-damage`**               |                   |                               |                                     | Short-term ozone damage factor representing reduction of photosynthetic carboxylation                                                                                         |
| **`O3-long-damage`**                |                   |                               |                                     | Long-term ozone damage factor associated with ozone-induced senescence                                                                                                        |
| **`O3-WS-gs-reduction`**            |                   |                               |                                     | Reduction factor describing the effect of water stress on stomatal conductance and ozone uptake                                                                               |
| **`O3-total-uptake`**               |                   | µmol O<sub>3</sub> m-2 ground |                                     | Cumulative ozone uptake per unit ground area                                                                                                                                  |
| **`guenther-isoprene-emission`**    |                   | µmol m-2 ground d-1           |                                     | Daily total canopy isoprene emission calculated with the Guenther model                                                                                                       |
| **`guenther-monoterpene-emission`** |                   | µmol m-2 ground d-1           |                                     | Daily total canopy mototerpene emission calculated with the Guenther model                                                                                                    |
| **`jjv-isoprene-emission`**         |                   | µmol m-2 ground d-1           |                                     | Daily total canopy isoprene emission calculated with the JJV model                                                                                                            |
| **`jjv-monoterpene-emission`**      |                   | µmol m-2 ground d-1           |                                     | Daily total canopy monoterpene emission calculated with the JJV model                                                                                                         |

---

## Automatic irrigation trigger

Automatic irrigation is enabled with `UseAutomaticIrrigation`. Its default is `false`. The parameters themselves are defined in `AutoIrrigationParams`.

When `amount` is greater than zero, MONICA applies that fixed amount and ignores `set_to_%nFC`. Otherwise, `set_to_%nFC` can refill the affected layers to the requested percentage of plant-available water capacity. The same depth, `calc_nFC_until_depth_m`, is used both to calculate the trigger and, in refill mode, to select the layers to refill.

Irrigation is possible only while a crop is growing and its current temperature sum lies within the inclusive range `HeatSumIrrigationStart` to `HeatSumIrrigationEnd`. These two parameters are defined in the active crop's cultivar configuration. If supplied, `startDate` and `stopDate` additionally restrict irrigation to an inclusive calendar-date range.

| Key                          | Default  | Unit         | Description                                                                                                    |
|------------------------------|----------|--------------|----------------------------------------------------------------------------------------------------------------|
| **`UseAutomaticIrrigation`** | `false`  |              | Enables or disables automatic irrigation. This key is a direct member of `sim.json`.                           |
| **`startDate`**              | unset    | ISO date     | First date on which automatic irrigation may occur. Absolute dates only.                                       |
| **`stopDate`**               | unset    | ISO date     | Last date on which automatic irrigation may occur. Absolute dates only.                                        |
| **`amount`**                 | unset    | mm           | Fixed irrigation amount. When greater than zero, `set_to_%nFC` is ignored.                                     |
| **`set_to_%nFC`**            | unset    | % nFC        | Refill target used when no positive fixed `amount` is configured.                                              |
| **`threshold`**              | unset    | fraction nFC | Legacy fractional trigger. Prefer `trigger_if_nFC_below_%`.                                                    |
| **`trigger_if_nFC_below_%`** | unset    | % nFC        | Trigger irrigation when plant-available water is at or below this percentage.                                  |
| **`calc_nFC_until_depth_m`** | `0.3`    | m            | Depth over which plant-available water is evaluated.                                                           |
| **`nitrateConcentration`**   | `0`      | mg dm-3      | Nitrate concentration in irrigation water. Must be inside `irrigationParameters`.                              |
| **`sulfateConcentration`**   | `0`      | mg dm-3      | Sulfate concentration in irrigation water. Must be inside `irrigationParameters`; currently ignored by MONICA. |

### Examples

**Example 1: Fixed Amount**

Apply 17 mm whenever plant-available water in the first 0.3 m is at or below 50%, from May 1 through August 31, inclusive.

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

**Example 2: Refill to Field Capacity**

Refill the first 0.5 m to field capacity (100% nFC) whenever plant-available water over that depth is at or below 50%. Irrigation water contains 10 mg dm-3 nitrate.

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