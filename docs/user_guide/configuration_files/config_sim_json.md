# The `sim.json` configuration file

The **`sim.json`** file contains simulation-specific information like start and end date (MONICA will pick the right climate data from **`climate.csv`**) or whether certain global toggles like nitrogen response/use of secondary yields will be turned on or off.

**`NumberOfLayers`** and **`LayerThickness`** shouldn't be changed currently.

---

## Reading climate data (**`climate.csv-options`**)

Climate data are stored in **`.csv`** formatted files, `climate.csv-options` in **`sim.json`** is used for their configuration. 
The time range for the MONICA run is defined by the keys **`start-date`** and **`end-date`** of the **JSON object** **`climate.csv-options`**. If these keys are not present, the time range is set that of the **`climate.csv`**.

The key **`no-of-climate-file-header-lines`** defines how many lines will be skipped initially, as there could be no header in the file (0), just the column names (1), or maybe more (e.g. the units). **`csv-separator`** defines what character is used to separate the columns and values. In a **Comma Separated Values** file this is usually a **`,`**, but could as well be the tabulator character **`\t`** or spaces etc.

MONICA accepts **Available Climate Data (ACD)** names for the climate input. These names are case-sensitive!

| ACD element     | Description                              | Example        | Unit       |
|-----------------|------------------------------------------|----------------|------------|
| **`day`**       | day of year                              | **5**          |            |
| **`month`**     | month of year                            | **11**         |            |
| **`year`**      | year                                     | **2017**       |            |                                      
| **`iso-date`**  | ISO date format                          | **2017-11-05** |            |          
| **`de-date`**   | German date format                       | **05.11.2017** |            |               
| **`tmin`**      | minimum daily temperature                | **-2**         | **°C**     |     
| **`tavg`**      | average daily temperature                | **15.3**       | **°C**     |    
| **`tmax`**      | maximum daily temperature                | **34.7**       | **°C**     |  
| **`precip`**    | daily precipitation                      | **2.3**        | **mm**     |
| **`globrad`**   | global radiation                         | **27.431**     | **MJ m-2** |   
| **`sunhours`**  | hours of sunshine (better use `globrad`) | **8.5**        | **h**      | 
| **`wind`**      | wind speed                               | **6.7**        | **m s-1**  |
| **`relhumid`**  | relative humidity                        | **90.0**       | **%**      |
| **`skip`**      | skip an existing element                 |                |            |

Unknown column headers will be skipped automatically, therefore the **`skip`** **ACD** is to explicitly skip known elements, e.g. if there are multiple columns for the same type of climate elements. 

In order to use any climate dataset without further adjustments, any column header names can be mapped to their according **ACD** names. The value of the optional key **`header-to-acd-names`** is a **JSON object** with its key/value pairs defining those mappings.

Additionally, it is possible that the value side of the mapping is actually a **JSON array**, which is allowed to contain two elements - first a simple arithmetic operation (`+`, `-`, `*`, `/`) and second a number value. Then instead of replacing the key (a name) by the value (a valid **ACD** name) the operation is applied to the key (a column name). In the example below the `.csv` file contains a column **global_radiation** in **J cm-2** **global_radiation** is mapped to **`globrad`** the valid MONICA **ACD** name for the global radiation, and secondly it will be multiplied (`*`) by **100.0** to convert the values to **MJ m-2**.

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

All results of a MONICA run can be stored in a **CSV** formatted file. Which results are output and how these results are aggregated can be defined in **`sim.json`** under the key **`output`**.

The following table describes a couple of options which can be set:

| Key                            | Description                                                                                                                                      | Example/Default Value        |
|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| **`path-to-output`**           | Path to directory to write output to                                                                                                             | **`./`** = current directory |
| **`write-file?`**              | Write file to "path-to-output"?                                                                                                                  | **`true`** or **`false`**    |
| **`include-header-row`**       | Include the header row in the output                                                                                                             | **`true`** or **`false`**    |
| **`include-units-row`**        | Include a line with the units                                                                                                                    | **`true`** or **`false`**    |
| **`include aggregation-rows`** | Show two rows telling about a) the expression which requested an output and b) what MONICA interpreted - both can be used for debugging purposes | **`true`** or **`false`**    |
| **`csv-separator`**            | The separating character to be used                                                                                                              | **`,`**                      |
| **`events`**                   | A list of pairs, which describe the events on which MONICA is requested to output a list of results                                              |                              |

---

### Events

The key **`events`** defines the list of MONICA results to be output. The events are defined in a **JSON array**, each as a pair of an **`event`** followed by the according  **[list of outputs]**. An event can either be a simple string like **`"daily"`** or a **JSON object** or a **JSON array**. A string and a **JSON array** are just shortcuts for a more complex **JSON object** which describes the event upon which output should be generated.

In order to define requested outputs a few kinds of information have to be distinguished:

1. when to **start**/**end** outputting results (**`start`**/**`end`** keys)
2. in the start/end period **from**/**to** when to aggregate data (**`from`**/**`to`** keys)
3. **at** which time/condition to write a result, which doesn't aggregate results
4. **while** some condition is true, aggregate results

ISO-Dates can contain placeholders (**`x`**). Every possible value for the year, month, or day of the placeholder is used.

#### Available events

Additional to the date patterns, each workstep generates an event with the same name, e.g. there are events like Sowing, Harvest, AutomaticSowing, SetValue etc.

* **run-started**
* **Stage-1**, **Stage-2**, ... 
* **emergence**
* **anthesis**
* **cereal-stem-elongation**
* **maturity**
* **Sowing**, **AutomaticSowing**, **Cutting**, ....

The following table shows some event expressions, shortcuts (and their expansion):

| Shortcut                     | Expanded form                                                          | Meaning                                                                                                                                                              |
|------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                              | `{"start": "xxxx-05-01", "end": "xxxx-07-31", "at": "xxxx-xx-15"}`     | output results at every 15th from Mai to July - daily results will be output                                                                                         |
|                              | `{"from": "Sowing", "to": "anthesis", "while": ["ETa/ETc", "<", 0.4]}` | output aggregated results from "Sowing" event until (incl) the "anthesis" event, but include only results while actual to potential evapotranspiration was below 0.4 |
| `"xxxx-03-31"`               | `{"at": "xxxx-03-31"}`                                                 | write results for every March 31st                                                                                                                                   |
| `"daily"`                    | `{"at": "xxxx-xx-xx"}`                                                 | write daily results                                                                                                                                                  |
| `"monthly"`                  | `{"from": "xxxx-xx-01", "to": "xxxx-xx-31"}`                           | write monthly aggregated results                                                                                                                                     |
| `"yearly"`                   | `{"from": "xxxx-01-01", "to": "xxxx-12-31"}`                           | write yearly aggregated results                                                                                                                                      |
| `"run"`                      | `{"from": <start-date>, "to": <end-date>}`                             | write results aggregated over the entire run                                                                                                                         |
| `"crop"`                     | `{"from": "Sowing", "to": "Harvest"}`                                  | write results aggregated during the cropping period                                                                                                                  |
| `["while", "Stage", "=", 5]` | `{"while": ["Stage", "=", 5]}`                                         | write results aggregated only while the result **`Stage`** equals 5                                                                                                  |
| `["at", "Stage", "=", 2]`    | `{"at": ["Stage", "=", 2]}`                                            | write results daily if the result **`Stage`** equals 2                                                                                                               |
| `[["Mois", 1], "<", 0.5]`    | `{"at": ["Mois", 1], "<", 0.5]}`                                       | write results daily if the soil moisture in the first layer is below 0.5                                                                                             |
| `"Sowing"`                   | `{"at": "Sowing"}`                                                     | at sowing time write a result                                                                                                                                        |

As shown in the table above, simple comparison expressions can be used in the events. The available operators are **`<`, `<=`, `=`, `>=`, `>`**. Output expressions, such as **`"Stage"`** or **`["Mois", 1]`**, as well as numeric values (e.g. **1**) can be used on both sides of these operators.

---

## List of outputs

The previous section defined the events, so the points in time for which MONICA results should be output. 
What remains is to define what should be output. In the previous table appeared already some **outputs** in expressions like `["at", "Stage", "=", 2]` or `[["Mois", 1], "<", 0.5]`. **`"Stage"`** outputs the current development stage the plant is in and **`["Mois", 1]`** outputs the soil moisture in the first 10 cm soil-layer. MONICA internally defines a lot of names which refer to results which can be output. There are three categories:

1. Scalar values like **`Stage`**
2. Array values like **`Mois`**, which actually consist of 20 values for the soil moisture in all the layers
3. Array values like **`["OrgBiom", "Root"]`**, crop-organ specific results

MONICA currently uses a fixed set of 20 10 cm soil layers. If an output requires to choose a layer or a range of layers, a number between 1 and 20 has to be supplied. If the output is about a crop-organ, one of the following keywords is to be used: **`"Root"`, `"Leaf"`, `"Shoot"`, `"Fruit"`, `"Struct"`, `"Sugar"`**.

Additionally, the user has to tell MONICA whether ranges of values (in the arrays) are to be output as a bunch of scalars or instead be aggregated to a single value and if they should be aggregated, how to aggregate them. The following aggregation operations are available: **`AVG`, `MEDIAN`, `SUM`, `MIN`, `MAX`, `FIRST`, `LAST`, `NONE`**. Aggregation might happen on a daily basis to aggregate soil layers to a single scalar value (the **default** operation for layer aggregation is **`NONE`**) or in aggregation time ranges, e.g. to aggregate monthly values, where the **default** aggregation operation is **`AVG`**.

| Aggregation reason                      | Default operation  | Meaning                                                                                                                                                                  |
|-----------------------------------------|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aggregate soil layers into scalar value | **`NONE`**         | if no aggregation operation is given, selected range of layer values will be output                                                                                      |
| aggregate time range                    | **`AVG`**          | if an event defines an aggregation time range, a missing aggregation operation (value 2 or 3 in **JSON array**), the temporarily collected daily values will be averaged |

To understand aggregation operations one can think of that all the values to be aggregated (e.g. the **Runoff**) will be stored during the **`from`**/**`to`** period temporarily in a list. After the **`to`** time range is over the aggregation operation will be applied to this list. That means that **`AVG`** will average all values or that **`FIRST`** will return simply the first value in the list aka the value when the time range began (**`from`** day).

The user specifies an output by either defining in the most simple case the name of the output (e.g. **`"Stage"`**, **`"Crop"`** or **`"Date"`**) or a **JSON array** where the first element of the array is the aforementioned output name. It is allowed to append to the name, separated by the character **`|`** a display name which will in the output used instead of the result name, e.g. **`"DOY|MatDOY"`** would not output **`DOY`** but **`MatDOY`** which could be more descriptive. The second element in the array is, either the chosen soil layer, layer range, crop-organ (for array values) or the aggregation operation for scalar values. The latter is by default set to **`NONE`** if not used. If the output is an array value a third value, the aggregation operation can be supplied. For array values the second value can be a single number (the soil layer number), a string describing the crop-organ or again an array which describes (for soil layers only) a range of layers. In the latter case the array's first value is the starting layer, the second the ending layer (inclusive) and a possible third value an aggregation operation. If the range description specifies an aggregation operation every time an output is requested the defined range will be aggregated via the operation and a single value will be stored, else a list of values will be returned and show up in the outputs with the name of the result appended by underscore and layer number. The following examples with show the possible variations.

| Output definition                        | Meaning                                                                                                                                                                                                               |
|------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `"Date"`                                 | return the ISO date, e.g. 2017-01-17                                                                                                                                                                                  |                                                                                                                                                                         
| `["Year", "LAST"]`                       | return the year at **`to`** time, when the aggregation time ends                                                                                                                                                      |                                                                                                                                                 
| `["PercolationRate\|WDrain", 15, "SUM"]` | return the sum of the **`PercolationRate`**s in soil layer 15 (1.5 m) in an aggregation period, but rename it in the outputs as **`WDrain`** ... to **`SUM`** requires an event to specify the aggregation time range | 
| `["Mois", [1, 20]]`                      | return the daily soil-moistures of all 20 soil layers, output will be labeled "Mois_1", "Mois_2", etc.                                                                                                                |                                                                                                          
| `["OrgBiom", "Leaf"]`                    | return the daily organic biomass of the **`Leaf`** organ                                                                                                                                                              |                                                                                                                                                         
| `["SOC", [1, 3, "AVG"]]`                 | return the daily soil organic carbon as a single value averaged over the first three soil layers                                                                                                                      |                                                                                                             
| `["Precip", "SUM"]`                      | return the sum of the precipitations in a given aggregation time range                                                                                                                                                |                                                                                                                                       

In the created output file the order of the output results in the **`events`** list is preserved and can be relied upon. Below you'll find an example output section, which creates an equivalent file to the **`rmout`** file of the old version 1 of MONICA and also includes commented out (**`__events`**) an example set of outputs used in the EU MACSUR Heat Stress study.

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

Finally below you'll find an output-wise simplified full **`sim.json`** file.

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

## Allowed outputs

The most up-to-date list of available and allowed output names can be found directly from the MONICA source in the file [**`build-output.cpp`**](https://github.com/zalf-lsa/monica/blob/master/src/io/build-output.cpp#L359). There you'll find entries like:

```C++
build({id++, "Date", "", "output current date"},
.
.
.
build({id++, "TraDef", "0;1", "TranspirationDeficit"},
```

Here **`Date`** or **`TraDef`** are the allowed outputs. Additionally one can see after the output name (if available) the expected units of measure and a description.

| Output name                 | (L)ayers/(O)rgans? | Setable? | Unit                 | Description                                                                                                                         |
|-----------------------------|--------------------|----------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| **`Count`**                 |                    |          |                      | Output 1 for counting things                                                                                                        |
| **`CM-count`**              |                    |          |                      | Order number of the current cultivation method                                                                                      |
| **`Date`**                  |                    |          |                      | Current date                                                                                                                        |
| **`days-since-start`**      |                    |          |                      | Number of days since simulation start                                                                                               |
| **`DOY`**                   |                    |          |                      | Current day of year                                                                                                                 |
| **`Month`**                 |                    |          |                      | Current month number (1-12)                                                                                                         |
| **`Year`**                  |                    |          |                      | Current year                                                                                                                        |
| **`Crop`**                  |                    |          |                      | Crop name                                                                                                                           |
| **`TraDef`**                |                    |          | 0;1                  | Transpiration deficit index (`1`= no stress, `0` = maximum stress)                                                                  |
| **`Tra`**                   |                    |          | mm                   | Actual transpiration                                                                                                                |
| **`NDef`**                  |                    |          | 0;1                  | Nitrogen deficit index, indicates N availability (`1` = no stress, `0` = no N available)                                            | 
| **`HeatRed`**               |                    |          | 0;1                  | Heat stress reductor                                                                                                                |
| **`FrostRed`**              |                    |          | 0;1                  | Frost stress reductor                                                                                                               |
| **`OxRed`**                 |                    |          | 0;1                  | Oxygen deficit reductor                                                                                                             |
| **`Stage`**                 |                    |          |                      | Phenological development stage                                                                                                      |
| **`TempSum`**               |                    |          | °Cd                  | Accumulated temperature sum                                                                                                         |
| **`VernF`**                 |                    |          | 0;1                  | Vernalisation factor                                                                                                                |
| **`DaylF`**                 |                    |          | 0;1                  | Daylength factor                                                                                                                    |
| **`IncRoot`**               |                    |          | kg ha-1              | Growth increment of root                                                                                                            |
| **`IncLeaf`**               |                    |          | kg ha-1              | Growth increment of leaf                                                                                                            |
| **`IncShoot`**              |                    |          | kg ha-1              | Growth increment of shoot                                                                                                           |
| **`IncFruit`**              |                    |          | kg ha-1              | Growth increment of fruit                                                                                                           |
| **`RelDev`**                |                    |          | 0;1                  | Relative total development                                                                                                          |
| **`LT50`**                  |                    |          | °C                   | LT50, crop's lethal temperature                                                                                                     |
| **`AbBiom`**                |                    |          | kg ha-1              | Aboveground biomass                                                                                                                 |
| **`OrgBiom`**               | O                  |          | kgDM ha-1            | Biomass of individual crop organs (Root, Leaf, Shoot, Fruit, etc.)                                                                  |
| **`Yield`**                 |                    |          | kgDM ha-1            | Primary harvested yield                                                                                                             |
| **`sumExportedCutBiomass`** |                    |          | kgDM ha-1            | Total exported biomass across all cuts for the current crop                                                                         |
| **`exportedCutBiomass`**    |                    |          | kgDM ha-1            | Exported biomass from the current crop at the current cut                                                                           |
| **`sumResidueCutBiomass`**  |                    |          | kgDM ha-1            | Total residue biomass across all cuts for the current crop                                                                          |
| **`residueCutBiomass`**     |                    |          | kgDM ha-1            | Residue biomass from the current crop at the current cut                                                                            |
| **`GroPho`**                |                    |          | kgCH2O ha-1          | Gross photosynthesis rate per hectare                                                                                               |
| **`NetPhot`**               |                    |          | kgCH2O ha-1          | Net photosynthesis per hectare                                                                                                      |
| **`MaintR`**                |                    |          | kgCH2O ha-1          | Maintenance respiration rate from AGROSIM per hectare                                                                               |
| **`GrowthR`**               |                    |          | kgCH2O ha-1          | Growth respiration rate from AGROSIM per hectare                                                                                    |
| **`StomRes`**               |                    |          | s m-1                | Stomata resistance                                                                                                                  |
| **`Height`**                |                    |          | m                    | Crop height                                                                                                                         |
| **`LAI`**                   |                    |          | m2 m-2               | Leaf Area Index                                                                                                                     |
| **`RootDep`**               |                    |          | layer#               | Rooting depth                                                                                                                       |
| **`EffRootDep`**            |                    |          | m                    | Depth of the maximum active and effective root                                                                                      |
| **`TotBiomN`**              |                    |          | kgN ha-1             | Total nitrogen content in biomass                                                                                                   |
| **`AbBiomN`**               |                    |          | kgN ha-1             | Aboveground nitrogen content in biomass                                                                                             |
| **`SumNUp`**                |                    |          | kgN ha-1             | Cumulative actual nitrogen uptake by the crop                                                                                       |
| **`ActNup`**                |                    |          | kgN ha-1             | Actual nitrogen uptake                                                                                                              |
| **`PotNup`**                |                    |          | kgN ha-1             | Potential nitrogen uptake                                                                                                           |
| **`NFixed`**                |                    |          | kgN ha-1             | Crop's nitrogen input via atmospheric fixation                                                                                      |
| **`Target`**                |                    |          | kgN ha-1             | Target nitrogen concentration                                                                                                       |
| **`CritN`**                 |                    |          | kgN ha-1             | Critical nitrogen concentration                                                                                                     |
| **`AbBiomNc`**              |                    |          | kgN ha-1 (kg ha-1)-1 | Aboveground biomass nitrogen concentration                                                                                          |
| **`Nstress`**               |                    |          | -                    | Nitrogen stress index, indicates N availability (`1` = no stress, `0` = no N available)                                             |
| **`YieldNc`**               |                    |          | kgN ha-1             | Nitrogen concentration in primary yield                                                                                             |
| **`Protein`**               |                    |          | kg kg-1              | Raw protein concentration                                                                                                           |
| **`NPP`**                   |                    |          | kgC ha-1             | Net Primary Production (NPP)                                                                                                        |
| **`NPP-Organs`**            | O                  |          | kgC ha-1             | Organ-specific Net Primary Production (NPP)                                                                                         |
| **`GPP`**                   |                    |          | kgC ha-1             | Gross Primary Production (GPP)                                                                                                      |
| **`Ra`**                    |                    |          | kgC ha-1             | Autotrophic respiration                                                                                                             |
| **`Ra-Organs`**             | O                  |          | kgC ha-1             | Organ-specific autotrophic respiration                                                                                              |
| **`Mois`**                  | L                  | x        | m3 m-3               | Soil moisture content per layer                                                                                                     |
| **`Irrig`**                 |                    |          | mm                   | Irrigation amount applied                                                                                                           |
| **`Infilt`**                |                    |          | mm                   | Amount of water that infiltrates into top soil layer                                                                                |
| **`Surface`**               |                    |          | mm                   | Surface water storage                                                                                                               |
| **`RunOff`**                |                    |          | mm                   | Surface water runoff of current day                                                                                                 |
| **`SnowD`**                 |                    |          | mm                   | Snow depth                                                                                                                          |
| **`FrostD`**                |                    |          | m                    | Frost front depth in soil                                                                                                           |
| **`ThawD`**                 |                    |          | m                    | Thaw front depth in soil                                                                                                            |
| **`PASW`**                  | L                  |          | m3 m-3               | Plant available soil water per layer                                                                                                |
| **`SurfTemp`**              |                    |          | °C                   | Soil surface temperature                                                                                                            |
| **`STemp`**                 | L                  |          | °C                   | Soil temperature by layer                                                                                                           |
| **`Act_Ev`**                |                    |          | mm                   | Actual evaporation                                                                                                                  |
| **`Pot_ET`**                |                    |          | mm                   | Potential evapotranspiration = ET0 * Kc = the plants water use                                                                      |
| **`Act_ET`**                |                    |          | mm                   | Actual evapotranspiration = actual transpiration + actual evaporation + evaporation from interception + evaporation from surface    |
| **`ET0`**                   |                    |          | mm                   | Reference evapotranspiration                                                                                                        |
| **`Kc`**                    |                    |          |                      | Plant coefficient to calculate with ET0 the plants water use (ET0 * Kc)                                                             |
| **`AtmCO2`**                |                    |          | ppm                  | Atmospheric CO<sub>2</sub> concentration in the atmosphere                                                                          |
| **`Groundw`**               |                    |          | m                    | Groundwater table depth                                                                                                             |
| **`Recharge`**              |                    |          | mm                   | Groundwater recharge                                                                                                                |
| **`NLeach`**                |                    |          | kgN ha-1             | Nitrogen leach                                                                                                                      |
| **`NO3`**                   | L                  | x        | kgN m-3              | Soil nitrate (NO<sub>3</sub>) per layer                                                                                             |
| **`Carb`**                  | L                  | x        | kgN m-3              | Soil carbamide per layer                                                                                                            |
| **`NH4`**                   | L                  | x        | kgN m-3              | Soil ammonium (NH<sub>4</sub>) per layer                                                                                            |
| **`NO2`**                   | L                  | x        | kgN m-3              | Soil nitrite (NO<sub>2</sub>) per layer                                                                                             |
| **`SOC`**                   | L                  |          | kgC kg-1             | Soil organic carbon content per layer                                                                                               |
| **`SOC-X-Y`**               | L                  |          | gC m-2               | Soil organic carbon stock between soil layers `X` and `Y`. (SOC * BulkDensity * LayerThickness * 1000)                              |
| **`AOMf`**                  | L                  |          | kgC m-3              | Sum of added organic matter (AOM) fast pool carbon content                                                                          |
| **`AOMs`**                  | L                  |          | kgC m-3              | Sum of added organic matter (AOM) slow pool carbon content                                                                          |
| **`SMBf`**                  | L                  |          | kgC m-3              | Soil microbial biomass (SMB) fast pool carbon content                                                                               |
| **`SMBs`**                  | L                  |          | kgC m-3              | Soil microbial biomass (SMB) slow pool carbon content                                                                               |
| **`SOMf`**                  | L                  |          | kgC m-3              | Soil organic matter (SOM) fast pool carbon content                                                                                  |
| **`SOMs`**                  | L                  |          | kgC m-3              | Soil organic matter (SOM) slow pool carbon content                                                                                  |
| **`CBal`**                  | L                  |          | kgC m-3              | Carbon balance per layer                                                                                                            |
| **`Nmin`**                  | L                  |          | kgN ha-1             | Net nitrogen mineralisation rate per layer                                                                                          |
| **`NetNmin`**               |                    |          | kgN ha-1             | Net nitrogen mineralisation rate for the layers defined in the parameter `MaxMineralizationDepth` (see `general/soil-organic.json`) |
| **`Denit`**                 |                    |          | kgN ha-1             | Amount of N resulting from denitrification                                                                                          |
| **`actnitrate`**            |                    |          | kgN m-3              | N production rate resulting from nitrification (N<sub>2</sub>O STICS module)                                                        |
| **`N2O`**                   |                    |          | kgN ha-1             | Total N<sub>2</sub>O produced (MONICA's original approach)                                                                          |
| **`N2Onit`**                |                    |          | kgN ha-1             | N<sub>2</sub>O produced through nitrification (N<sub>2</sub>O STICS module)                                                         |
| **`N2Odenit`**              |                    |          | kgN ha-1             | N<sub>2</sub>O produced through denitrification (N<sub>2</sub>O STICS module)                                                       |
| **`SoilpH`**                |                    |          |                      | Soil pH                                                                                                                             |
| **`NEP`**                   |                    |          | kgC ha-1             | Net Ecosystem Production (NEP)                                                                                                      |
| **`NEE`**                   |                    |          | kgC ha-              | Net Ecosystem Exchange (NEE)                                                                                                        |
| **`Rh`**                    |                    |          | kgC ha-              | Daily decomposer respiration                                                                                                        |
| **`Tmin`**                  |                    |          | °C                   | Minimum daily air temperature                                                                                                       |
| **`Tavg`**                  |                    |          | °C                   | Mean daily air temperature                                                                                                          |
| **`Tmax`**                  |                    |          | °C                   | Maximum daily air temperature                                                                                                       |
| **`Precip`**                |                    |          | mm                   | Precipitation                                                                                                                       |
| **`Wind`**                  |                    |          | m s-1                | Wind speed                                                                                                                          |
| **`Globrad`**               |                    |          | MJ m-2               | Global radiation                                                                                                                    |
| **`Relhumid`**              |                    |          | %                    | Relative humidity                                                                                                                   |
| **`Sunhours`**              |                    |          | h                    | Sunshine duration                                                                                                                   |
| **`BedGrad`**               |                    |          | 0;1                  | Fractional soil coverage                                                                                                            |
| **`N`**                     | L                  |          | kgN m-3              | Mineral nitrogen content in soil                                                                                                    |
| **`Co`**                    | L                  |          | kgC m-3              | Soil organic carbon concentration per soil layer                                                                                    |
| **`NH3`**                   |                    |          | kgN ha-1             | Ammonia (NH<sub>3</sub>) volatilisation                                                                                             |
| **`NFert`**                 |                    |          | kgN ha-1             | Daily nitrogen fertiliser input                                                                                                     |
| **`WaterContent`**          | L                  |          | fraction nFC         | Fraction of usable field capacity: PASW/(FC-PWP)                                                                                    |
| **`CapillaryRise`**         | L                  |          | mm                   | Capillary rise                                                                                                                      |
| **`PercolationRate`**       | L                  |          | mm                   | Percolation rate                                                                                                                    |
| **`SMB-CO2-ER`**            | L                  |          |                      | Soil microbial biomass (SMB) CO<sub>2</sub> evolution rate                                                                          |
| **`Evapotranspiration`**    |                    |          | mm                   | Remaining evapotranspiration after evaporation of intercepted water                                                                 |
| **`Evaporation`**           |                    |          | mm                   | Evaporation from intercepted water                                                                                                  |
| **`ETa/ETc`**               |                    |          |                      | Ratio of actual to potential evapotranspiration Act_ET/Pot_ET (actual evapotranspiration/potential evapotranspiration)              |
| **`Transpiration`**         |                    |          | mm                   | Actual transpiration                                                                                                                |
| **`GrainN`**                |                    |          | kg ha-1              | Nitrogen content of harvested grains                                                                                                |
| **`Fc`**                    | L                  |          | m3 m-3               | Field capacity                                                                                                                      |
| **`Pwp`**                   | L                  |          | m3 m-3               | Permanent wilting point                                                                                                             |
| **`Nresid`**                |                    |          | kgN ha-1             | Nitrogen content in crop residues                                                                                                   |
| **`Sand`**                  | L                  |          | kg kg-1              | Soil sand content                                                                                                                   |
| **`Clay`**                  | L                  |          | kg kg-1              | Soil clay content                                                                                                                   |
| **`Silt`**                  | L                  |          | kg kg-1              | Soil silt content                                                                                                                   |                                                                                     
| **`Stone`**                 | L                  |          | kg kg-1              | Soil stone content                                                                                                                  |                                                                                 
| **`pH`**                    | L                  |          | kg kg-1              | Soil pH per layer                                                                                                                   |                                                                                    
| **`rootDensity`**           | L                  |          |                      | Root density in soil layer                                                                                                          |                                                                
| **`rootingZone`**           |                    |          |                      | Current rooting zone, the soil layer into which roots are growing                                                                   |                               

---

## Automatic Irrigation trigger

Turn automatic irrigation trigger on or off via the **`UseAutomaticIrrigation`** key set to **`true`**. If the key **`amount`** is set a fixed amount of irrigation water will be applied, else if **`set_to_%nFC`** is set, the soil moisture of the involved soil layers (**`calc_nFC_until_depth_m`**) will be set to the given percentage of available soil water. The trigger is only activated if there's a crop currently growing and if MONICAs internal current temperature sum is between **`HeatSumIrrigationStart`** and **`HeatSumIrrigationEnd`**. Both parameters can be found in the **cultivar** JSON file of the currently active crop.

| Key                            | Default value     | Unit         | Description                                                                                                                                                                   |
|--------------------------------|-------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`"UseAutomaticIrrigation"`** | `true` or `false` |              | Turn automatic irrigation on/off                                                                                                                                              |
| **`"nitrateConcentration"`**   | 0                 | mg dm-3      | Nitrate concentration in the irrigation water                                                                                                                                 |
| **`"sulfateConcentration"`**   | 0                 | mg dm-3      | Sulfate concentration in the irrigation water <br>⚠️ *Currently ignored by MONICA.*                                                                                           |
| **`"amount"`**                 |                   | mm           | Amount of irrigation water to apply if the plant available water drops below the threshold                                                                                    |
| **`"set_to_%nFC"`**            |                   | % nFC        | Set the soil moisture of all the involved layers to that percentage of plant available water (nFC)                                                                            |
| **`"threshold"`**              |                   | fraction nFC | Fraction of net field capacity (plant available water = field capacity - permanent wilting point to) to drop below <br>⚠️ *Deprecated, use `trigger_if_nFC_below_%` instead.* |
| **`"trigger_if_nFC_below_%"`** |                   | % nFC        | Trigger irrigation if percentage of net field capacity (plant available water = field capacity - permanent wilting point to) drops below that value                           |
| **`"calc_nFC_until_depth_m"`** | 0.3               | m            | The soil depth MONICA uses to check if irrigation should be triggered                                                                                                         |

Minimal example to trigger irrigation of 17 mm each time the plant available water in the first 30 cm of the soil profile drops below 50%.

```json
"UseAutomaticIrrigation": false,
"AutoIrrigationParams": {
  "amount": [17, "mm"],
  "trigger_if_nFC_below_%": [50, "%"],
  "calc_nFC_until_depth_m": [0.3, "m"]
}
```

Similar example, but this time set the soil moisture in the first 50 cm of the soil profile to field capacity = 100%nFC if the plant available water drops below 50%. This time add 10 mg/dm of nitrate with the irrigation water.

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