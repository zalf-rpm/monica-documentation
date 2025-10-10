# The `sim.json` configuration file

The `sim.json` file contains simulation-specific information such as the start and end date (MONICA will select the appropriate climate data from `climate.csv`) and whether certain global toggles, such as nitrogen response or the use of secondary yields, are turned on or off.

`NumberOfLayers` and `LayerThickness` should not be changed at this time.

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

Additionally, the value in this mapping can be a **JSON array** containing two elements &ndash; an arithmetic operation (`+`, `-`, `*`, `/`) and a number. In that case, the operation is applied to the values in that column. In the example below, if the `.csv` file contains a column `global_radiation` in `J cm-2`, it can be mapped to `globrad` (MONICA's internal ACD name) and multiplied (`*`) by **100.0** to convert to **MJ m-2**.

```json
"climate.csv-options": {
  "__given the start and end date, monica will run just this time range, else the full time range given by supplied climate data": "",
  "start-date": "1991-01-01",
  "end-date": "1997-12-31",

  "no-of-climate-file-header-lines": 1,
  "csv-separator": ",",
  "header-to-acd-names": {
    "DE-date": "de-date",
    "global_radiation": "globrad",
    "global_radiation": ["*", 100.0]
}
```

---

## Outputting results from MONICA

All results from a MONICA run can be written to a **CSV** file. Which results are written, and how they are aggregated, is configured in `sim.json` under the key `output`.

The following table describes several available options which can be set:

| Key                            | Description                                                                                                                   | Example/Default Value        |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| **`path-to-output`**           | Path to directory for output files                                                                                            | **`./`** (current directory) |
| **`write-file?`**              | Whether to write an output file to "path-to-output"                                                                           | **`true`** or **`false`**    |
| **`include-header-row`**       | Include header row in the output                                                                                              | **`true`** or **`false`**    |
| **`include-units-row`**        | Include a row with units                                                                                                      | **`true`** or **`false`**    |
| **`include aggregation-rows`** | Include two rows showing a) the expression which requested an output and b) what MONICA interpreted (for debugging purposes)  | **`true`** or **`false`**    |
| **`csv-separator`**            | Separator character                                                                                                           | **`,`**                      |
| **`events`**                   | A list of pairs, which describe the events on which MONICA is requested to output a list of results                           |                              |

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
| `[["Mois", 1], "<", 0.5]`    | `{"at": ["Mois", 1], "<", 0.5]}`                                       | Write results daily when the soil moisture in the first layer is below 0.5                                                                                       |
| `"Sowing"`                   | `{"at": "Sowing"}`                                                     | Write results at sowing time                                                                                                                                     |

As shown in the table above, simple comparison expressions can be used in events. The available operators are `<`, `<=`, `=`, `>=`, `>`. Output expressions such as `"Stage"` or `["Mois", 1]`, as well as numeric values (e.g., **1**), can be used on both sides of these operators.

---

## List of outputs

After defining events (i.e., when to output), you must specify **what** to output. 
Some examples of **outputs** already appeared in the previous table, such as `["at", "Stage", "=", 2]` or `[["Mois", 1], "<", 0.5]`. `"Stage"` outputs the current development stage of the plant, while `["Mois", 1]` outputs the soil moisture in the first 10 cm soil layer. 

MONICA supports three categories of outputs:

1. **Scalar values** (e.g., `"Stage"`)
2. **Array values** for soil layers (e.g., `"Mois"`, which consists of 20 values representing soil moisture in all layers)
3. **Array values** for crop organs (e.g., `["OrgBiom", "Root"]`)

MONICA currently uses 20 soil layers of 10 cm each (total depth 2 m). When outputs refer to layers, they can specify:

* a single layer (e.g., `15` for the 15th layer, corresponding to 1.5 m depth),
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
      ["Runoff", "SUM"],
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
      "Yield", "SumYield", "GroPhot", "NetPhot", "MaintR", "GrowthR",	"StomRes",
      "Height", "LAI", "RootDep", "EffRootDep", "TotBiomN", "AbBiomN", "SumNUp",
      "ActNup", "PotNup", "NFixed", "Target", "CritN", "AbBiomNc", "YieldNc",
      "Protein",
      "NPP", ["NPP", "Root"], ["NPP", "Leaf"], ["NPP", "Shoot"],
      ["NPP", "Fruit"], ["NPP", "Struct"], ["NPP", "Sugar"],
      "GPP",
      "Ra",
      ["Ra", "Root"], ["Ra", "Leaf"], ["Ra", "Shoot"], ["Ra", "Fruit"],
      ["Ra", "Struct"], ["Ra", "Sugar"],
      ["Mois", [1, 20]], "Precip", "Irrig", "Infilt", "Surface", "RunOff", "SnowD", "FrostD",
      "ThawD", ["PASW", [1, 20]], "SurfTemp", ["STemp", [1, 5]],
      "Act_Ev", "Act_ET", "ET0", "Kc", "AtmCO2", "Groundw", "Recharge", "NLeach",
      ["NO3", [1, 20]], ["Carb", 0], ["NH4", [1, 20]], ["NO2", [1, 4]],
      ["SOC", [1, 6]], ["SOC-X-Y", [1, 3, "SUM"]], ["SOC-X-Y", [1, 20, "SUM"]],
      ["AOMf", 1], ["AOMs", 1], ["SMBf", 1], ["SMBs", 1], ["SOMf", 1],
      ["SOMs", 1], ["CBal", 1], ["Nmin", [1, 3]], "NetNmin", "Denit", "N2O", "SoilpH",
      "NEP", "NEE", "Rh", "Tmin", "Tavg", "Tmax", "Wind", "Globrad", "Relhumid", "Sunhours",
      "NFert"
    ]
  ]
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

			"harvesting", ["Crop", ["OrgBiom", "Fruit"], "Yield"],
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

## Allowed output variables

The most up-to-date list of available and allowed output names can be found directly in the MONICA source code, in the file [**`build-output.cpp`**](https://github.com/zalf-lsa/monica/blob/master/src/io/build-output.cpp#L359). There, you will find entries such as:

```C++
build({id++, "Date", "", "output current date"},
...
build({id++, "TraDef", "0;1", "TranspirationDeficit"},
```

In this example, `Date` and `TraDef` are allowed output names. Additionally, the code lists the expected units of measure (if defined) and provides a description for each output variable.

| Output name                 | (L)ayers/(O)rgans | Setttable? | Unit                 | Description                                                                                                                          |
|-----------------------------|-------------------|------------|----------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **`Count`**                 |                   |            |                      | Output constant value 1 (can be used for counting records or events)                                                                 |
| **`CM-count`**              |                   |            |                      | Order number of the current cultivation method                                                                                       |
| **`Date`**                  |                   |            |                      | Current date                                                                                                                         |
| **`days-since-start`**      |                   |            |                      | Number of days since simulation start                                                                                                |
| **`DOY`**                   |                   |            |                      | Current day of year                                                                                                                  |
| **`Month`**                 |                   |            |                      | Current month number (1-12)                                                                                                          |
| **`Year`**                  |                   |            |                      | Current year                                                                                                                         |
| **`Crop`**                  |                   |            |                      | Crop name                                                                                                                            |
| **`TraDef`**                |                   |            | 0;1                  | Transpiration deficit index (`1`= no stress, `0` = maximum stress)                                                                   |
| **`Tra`**                   |                   |            | mm                   | Actual transpiration                                                                                                                 |
| **`NDef`**                  |                   |            | 0;1                  | Nitrogen deficit index, indicates N availability (`1` = no stress, `0` = no N available)                                             | 
| **`HeatRed`**               |                   |            | 0;1                  | Heat stress reductor                                                                                                                 |
| **`FrostRed`**              |                   |            | 0;1                  | Frost stress reductor                                                                                                                |
| **`OxRed`**                 |                   |            | 0;1                  | Oxygen deficit reductor                                                                                                              |
| **`Stage`**                 |                   |            |                      | Phenological development stage                                                                                                       |
| **`TempSum`**               |                   |            | °Cd                  | Accumulated temperature sum                                                                                                          |
| **`VernF`**                 |                   |            | 0;1                  | Vernalisation factor                                                                                                                 |
| **`DaylF`**                 |                   |            | 0;1                  | Daylength factor                                                                                                                     |
| **`IncRoot`**               |                   |            | kg ha-1              | Growth increment of root                                                                                                             |
| **`IncLeaf`**               |                   |            | kg ha-1              | Growth increment of leaf                                                                                                             |
| **`IncShoot`**              |                   |            | kg ha-1              | Growth increment of shoot                                                                                                            |
| **`IncFruit`**              |                   |            | kg ha-1              | Growth increment of fruit                                                                                                            |
| **`RelDev`**                |                   |            | 0;1                  | Relative total development                                                                                                           |
| **`LT50`**                  |                   |            | °C                   | Crop's lethal temperature at which 50% of plants die                                                                                 |
| **`AbBiom`**                |                   |            | kg ha-1              | Aboveground biomass                                                                                                                  |
| **`OrgBiom`**               | O                 |            | kgDM ha-1            | Biomass of individual crop organs (e.g, Root, Leaf, Shoot, Fruit)                                                                    |
| **`Yield`**                 |                   |            | kgDM ha-1            | Primary harvested yield                                                                                                              |
| **`sumExportedCutBiomass`** |                   |            | kgDM ha-1            | Total exported biomass across all cuts for the current crop                                                                          |
| **`exportedCutBiomass`**    |                   |            | kgDM ha-1            | Exported biomass from the current crop at the current cut                                                                            |
| **`sumResidueCutBiomass`**  |                   |            | kgDM ha-1            | Total residue biomass across all cuts for the current crop                                                                           |
| **`residueCutBiomass`**     |                   |            | kgDM ha-1            | Residue biomass from the current crop at the current cut                                                                             |
| **`GroPho`**                |                   |            | kgCH2O ha-1          | Gross photosynthesis rate per hectare                                                                                                |
| **`NetPhot`**               |                   |            | kgCH2O ha-1          | Net photosynthesis per hectare                                                                                                       |
| **`MaintR`**                |                   |            | kgCH2O ha-1          | Maintenance respiration rate per hectare (from AGROSIM)                                                                              |
| **`GrowthR`**               |                   |            | kgCH2O ha-1          | Growth respiration rate per hectare (from AGROSIM)                                                                                   |
| **`StomRes`**               |                   |            | s m-1                | Stomata resistance                                                                                                                   |
| **`Height`**                |                   |            | m                    | Crop height                                                                                                                          |
| **`LAI`**                   |                   |            | m2 m-2               | Leaf Area Index                                                                                                                      |
| **`RootDep`**               |                   |            | layer#               | Rooting depth                                                                                                                        |
| **`EffRootDep`**            |                   |            | m                    | Depth of the maximum active and effective root                                                                                       |
| **`TotBiomN`**              |                   |            | kgN ha-1             | Total nitrogen content in biomass                                                                                                    |
| **`AbBiomN`**               |                   |            | kgN ha-1             | Aboveground nitrogen content in biomass                                                                                              |
| **`SumNUp`**                |                   |            | kgN ha-1             | Cumulative actual nitrogen uptake by the crop                                                                                        |
| **`ActNup`**                |                   |            | kgN ha-1             | Actual nitrogen uptake                                                                                                               |
| **`PotNup`**                |                   |            | kgN ha-1             | Potential nitrogen uptake                                                                                                            |
| **`NFixed`**                |                   |            | kgN ha-1             | Nitrogen input by the crop via atmospheric fixation                                                                                  |
| **`Target`**                |                   |            | kgN ha-1             | Target nitrogen concentration                                                                                                        |
| **`CritN`**                 |                   |            | kgN ha-1             | Critical nitrogen concentration                                                                                                      |
| **`AbBiomNc`**              |                   |            | kgN ha-1 (kg ha-1)-1 | Nitrogen concentration in aboveground biomass                                                                                        |
| **`Nstress`**               |                   |            | -                    | Nitrogen stress index, indicates N availability (`1` = no stress, `0` = no N available)                                              |
| **`YieldNc`**               |                   |            | kgN ha-1             | Nitrogen concentration in primary yield                                                                                              |
| **`Protein`**               |                   |            | kg kg-1              | Raw protein concentration                                                                                                            |
| **`NPP`**                   |                   |            | kgC ha-1             | Net Primary Production (NPP)                                                                                                         |
| **`NPP-Organs`**            | O                 |            | kgC ha-1             | Organ-specific Net Primary Production (NPP)                                                                                          |
| **`GPP`**                   |                   |            | kgC ha-1             | Gross Primary Production (GPP)                                                                                                       |
| **`Ra`**                    |                   |            | kgC ha-1             | Autotrophic respiration                                                                                                              |
| **`Ra-Organs`**             | O                 |            | kgC ha-1             | Organ-specific autotrophic respiration                                                                                               |
| **`Mois`**                  | L                 | x          | m3 m-3               | Soil moisture content per layer                                                                                                      |
| **`Irrig`**                 |                   |            | mm                   | Irrigation amount applied                                                                                                            |
| **`Infilt`**                |                   |            | mm                   | Amount of water that infiltrates into top soil layer                                                                                 |
| **`Surface`**               |                   |            | mm                   | Surface water storage                                                                                                                |
| **`RunOff`**                |                   |            | mm                   | Surface water runoff for the current day                                                                                             |
| **`SnowD`**                 |                   |            | mm                   | Snow depth                                                                                                                           |
| **`FrostD`**                |                   |            | m                    | Frost front depth in soil                                                                                                            |
| **`ThawD`**                 |                   |            | m                    | Thaw front depth in soil                                                                                                             |
| **`PASW`**                  | L                 |            | m3 m-3               | Plant available soil water per layer                                                                                                 |
| **`SurfTemp`**              |                   |            | °C                   | Soil surface temperature                                                                                                             |
| **`STemp`**                 | L                 |            | °C                   | Soil temperature by layer                                                                                                            |
| **`Act_Ev`**                |                   |            | mm                   | Actual evaporation                                                                                                                   |
| **`Pot_ET`**                |                   |            | mm                   | Potential evapotranspiration = ET0 * Kc = the plants water use                                                                       |
| **`Act_ET`**                |                   |            | mm                   | Actual evapotranspiration = actual transpiration + actual evaporation + evaporation from interception + evaporation from surface     |
| **`ET0`**                   |                   |            | mm                   | Reference evapotranspiration                                                                                                         |
| **`Kc`**                    |                   |            |                      | Plant coefficient to calculate with ET0 the plants water use (ET0 * Kc)                                                              |
| **`AtmCO2`**                |                   |            | ppm                  | Atmospheric CO<sub>2</sub> concentration in the atmosphere                                                                           |
| **`Groundw`**               |                   |            | m                    | Groundwater table depth                                                                                                              |
| **`Recharge`**              |                   |            | mm                   | Groundwater recharge                                                                                                                 |
| **`NLeach`**                |                   |            | kgN ha-1             | Nitrogen leaching below the root zone                                                                                                |
| **`NO3`**                   | L                 | x          | kgN m-3              | Soil nitrate (NO<sub>3</sub>) per layer                                                                                              |
| **`Carb`**                  | L                 | x          | kgN m-3              | Soil carbamide per layer                                                                                                             |
| **`NH4`**                   | L                 | x          | kgN m-3              | Soil ammonium (NH<sub>4</sub>) per layer                                                                                             |
| **`NO2`**                   | L                 | x          | kgN m-3              | Soil nitrite (NO<sub>2</sub>) per layer                                                                                              |
| **`SOC`**                   | L                 |            | kgC kg-1             | Soil organic carbon content per layer                                                                                                |
| **`SOC-X-Y`**               | L                 |            | gC m-2               | Soil organic carbon stock between soil layers *X* and *Y*. (SOC * bulk density * layer thickness * 1000)                             |
| **`AOMf`**                  | L                 |            | kgC m-3              | Sum of added organic matter (AOM) fast pool carbon content                                                                           |
| **`AOMs`**                  | L                 |            | kgC m-3              | Sum of added organic matter (AOM) slow pool carbon content                                                                           |
| **`SMBf`**                  | L                 |            | kgC m-3              | Soil microbial biomass (SMB) fast pool carbon content                                                                                |
| **`SMBs`**                  | L                 |            | kgC m-3              | Soil microbial biomass (SMB) slow pool carbon content                                                                                |
| **`SOMf`**                  | L                 |            | kgC m-3              | Soil organic matter (SOM) fast pool carbon content                                                                                   |
| **`SOMs`**                  | L                 |            | kgC m-3              | Soil organic matter (SOM) slow pool carbon content                                                                                   |
| **`CBal`**                  | L                 |            | kgC m-3              | Carbon balance per layer                                                                                                             |
| **`Nmin`**                  | L                 |            | kgN ha-1             | Net nitrogen mineralisation rate per layer                                                                                           |
| **`NetNmin`**               |                   |            | kgN ha-1             | Net nitrogen mineralisation rate for the layers defined in the parameter `MaxMineralizationDepth` (see `general/soil-organic.json`)  |
| **`Denit`**                 |                   |            | kgN ha-1             | Amount of N resulting from denitrification                                                                                           |
| **`actnitrate`**            |                   |            | kgN m-3              | N production rate resulting from nitrification (N<sub>2</sub>O STICS module)                                                         |
| **`N2O`**                   |                   |            | kgN ha-1             | Total N<sub>2</sub>O produced (MONICA's original approach)                                                                           |
| **`N2Onit`**                |                   |            | kgN ha-1             | N<sub>2</sub>O produced through nitrification (N<sub>2</sub>O STICS module)                                                          |
| **`N2Odenit`**              |                   |            | kgN ha-1             | N<sub>2</sub>O produced through denitrification (N<sub>2</sub>O STICS module)                                                        |
| **`SoilpH`**                |                   |            |                      | Soil pH                                                                                                                              |
| **`NEP`**                   |                   |            | kgC ha-1             | Net Ecosystem Production (NEP)                                                                                                       |
| **`NEE`**                   |                   |            | kgC ha-              | Net Ecosystem Exchange (NEE)                                                                                                         |
| **`Rh`**                    |                   |            | kgC ha-              | Daily decomposer respiration                                                                                                         |
| **`Tmin`**                  |                   |            | °C                   | Minimum daily air temperature                                                                                                        |
| **`Tavg`**                  |                   |            | °C                   | Mean daily air temperature                                                                                                           |
| **`Tmax`**                  |                   |            | °C                   | Maximum daily air temperature                                                                                                        |
| **`Precip`**                |                   |            | mm                   | Precipitation                                                                                                                        |
| **`Wind`**                  |                   |            | m s-1                | Wind speed                                                                                                                           |
| **`Globrad`**               |                   |            | MJ m-2               | Global radiation                                                                                                                     |
| **`Relhumid`**              |                   |            | %                    | Relative humidity                                                                                                                    |
| **`Sunhours`**              |                   |            | h                    | Sunshine duration                                                                                                                    |
| **`BedGrad`**               |                   |            | 0;1                  | Fractional soil coverage                                                                                                             |
| **`N`**                     | L                 |            | kgN m-3              | Mineral nitrogen content in soil                                                                                                     |
| **`Co`**                    | L                 |            | kgC m-3              | Soil organic carbon concentration per soil layer                                                                                     |
| **`NH3`**                   |                   |            | kgN ha-1             | Ammonia (NH<sub>3</sub>) volatilisation                                                                                              |
| **`NFert`**                 |                   |            | kgN ha-1             | Daily nitrogen fertiliser input                                                                                                      |
| **`WaterContent`**          | L                 |            | fraction nFC         | Fraction of usable field capacity: PASW/(FC-PWP)                                                                                     |
| **`CapillaryRise`**         | L                 |            | mm                   | Capillary rise                                                                                                                       |
| **`PercolationRate`**       | L                 |            | mm                   | Percolation rate                                                                                                                     |
| **`SMB-CO2-ER`**            | L                 |            |                      | Soil microbial biomass (SMB) CO<sub>2</sub> evolution rate                                                                           |
| **`Evapotranspiration`**    |                   |            | mm                   | Remaining evapotranspiration after evaporation of intercepted water                                                                  |
| **`Evaporation`**           |                   |            | mm                   | Evaporation from intercepted water                                                                                                   |
| **`ETa/ETc`**               |                   |            |                      | Ratio of actual to potential evapotranspiration `Act_ET/Pot_ET` (actual evapotranspiration/potential evapotranspiration)             |
| **`Transpiration`**         |                   |            | mm                   | Actual transpiration                                                                                                                 |
| **`GrainN`**                |                   |            | kg ha-1              | Nitrogen content of harvested grains                                                                                                 |
| **`Fc`**                    | L                 |            | m3 m-3               | Field capacity                                                                                                                       |
| **`Pwp`**                   | L                 |            | m3 m-3               | Permanent wilting point                                                                                                              |
| **`Nresid`**                |                   |            | kgN ha-1             | Nitrogen content in crop residues                                                                                                    |
| **`Sand`**                  | L                 |            | kg kg-1              | Soil sand content                                                                                                                    |
| **`Clay`**                  | L                 |            | kg kg-1              | Soil clay content                                                                                                                    |
| **`Silt`**                  | L                 |            | kg kg-1              | Soil silt content                                                                                                                    |                                                                                     
| **`Stone`**                 | L                 |            | kg kg-1              | Soil stone content                                                                                                                   |                                                                                 
| **`pH`**                    | L                 |            | kg kg-1              | Soil pH per layer                                                                                                                    |                                                                                    
| **`rootDensity`**           | L                 |            |                      | Root density in soil layer                                                                                                           |                                                                
| **`rootingZone`**           |                   |            |                      | Current rooting zone, the soil layer into which roots are growing                                                                    |                               

---

## Automatic irrigation trigger

Automatic irrigation can be enabled or disabled using the `UseAutomaticIrrigation` key (set to `true` or `false`). If the key `amount` is defined, a fixed amount of irrigation water will be applied. Alternatively, if `set_to_%nFC` is specified, the soil moisture of the affected layers (up to the depth defined by `calc_nFC_until_depth_m`) will be set to the given percentage of available soil water. The trigger becomes active **only** when a crop is currently growing **and** MONICA's internal current temperature sum is between `HeatSumIrrigationStart` and `HeatSumIrrigationEnd`. Both parameters are defined in the **cultivar** JSON file of the active crop.

| Key                            | Default value     | Unit         | Description                                                                                                                                                                                                         |
|--------------------------------|-------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`"UseAutomaticIrrigation"`** | `true` or `false` |              | Enables or disables automatic irrigation                                                                                                                                                                            |
| **`"nitrateConcentration"`**   | 0                 | mg dm-3      | Nitrate concentration in irrigation water                                                                                                                                                                           |
| **`"sulfateConcentration"`**   | 0                 | mg dm-3      | Sulfate concentration in irrigation water (*currently ignored by MONICA*)                                                                                                                                           |
| **`"amount"`**                 |                   | mm           | Amount of irrigation water to apply when plant available water drops below the trigger threshold                                                                                                                    |
| **`"set_to_%nFC"`**            |                   | % nFC        | Sets soil moisture of the involved layers to the specified percentage of plant available water (nFC)                                                                                                                |
| **`"threshold"`**              |                   | fraction nFC | Fraction of net field capacity (plant available water = field capacity - permanent wilting point to) that must be reached before irrigation is triggered <br>⚠️ *Deprecated, use `trigger_if_nFC_below_%` instead.* |
| **`"trigger_if_nFC_below_%"`** |                   | % nFC        | Triggers irrigation if the percentage of net field capacity (plant available water = field capacity - permanent wilting point to) drops below this value                                                            |
| **`"calc_nFC_until_depth_m"`** | 0.3               | m            | The soil depth used by MONICA to check whether the irrigation trigger condition is met                                                                                                                              |


#### Example 1:

Apply 17 mm irrigation each time the plant available water in the first 30 cm of the soil profile drops below 50%.

```json
"UseAutomaticIrrigation": false,
"AutoIrrigationParams": {
  "amount": [17, "mm"],
  "trigger_if_nFC_below_%": [50, "%"],
  "calc_nFC_until_depth_m": [0.3, "m"]
}
```

#### Example 2:

Set soil moisture in the first 50 cm of the soil profile to field capacity = 100% nFC, if the plant available water drops below 50%. This time add 10 mg dm-3 of nitrate with the irrigation water.

```json
"UseAutomaticIrrigation": false,
"AutoIrrigationParams": {
  "irrigationParameters": {
    "nitrateConcentration": [10, "mg dm-3"],
  },
  "trigger_if_nFC_below_%": [50, "%"],
  "set_to_%nFC": [100, "%"],
  "calc_nFC_until_depth_m": [0.5, "m"]
}
```