# **CROP JSON**
**crop.json** defines all crop specific input data for MONICA. There have to be just two keys in the top-level JSON object, **cropRotation** and **CropParameters**. **cropRotation** is a **JSON array** of JSON objects representing a cultivation method (class **CultivationMethod** in the MONICA C++ code). **CropParameters** keeps global crop related MONICA parameters, which are taken either from the **user-parameter** database table or a referenced file.

## Cultivation Methods

A cultivation method is defined as a set of worksteps (class hierarchy **Workstep** in MONICA code). The **order** of the worksteps is **important**. Putting static worksteps (with fixed relative/absolute dates) in the wrong order may prevent them from being executed! Worksteps can be the sowing of a crop, the harvesting thereof, fertilizer, irrigation water or tillage applications. A sowing application tells MONICA at which point in time which crop to grow. Within the sowing workstep a parameter **crop** will be set to a JSON object with **ALL** the crop's parameters MONICA needs. This can be done in-line, via a include function (**include-from-db** or **include-from-file**) or via a **ref** function. Because crops might be used in more than one workstep or cultivation method, the example below (at the end of the **CROP JSON** section) defines all used crops in a separate JSON object named **crops**, which maps shortcut names to the actual crop parameters. The mapping object is available under the top-level key **crops** which is the first parameter to the **ref** function, the second being the referenced shortcut name. The names chosen (**crops** and e.g. **WR** or **SM**) are abitrary.

A cultivation method can also have parameters, which are described in the following table.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**can-be-skipped** | **true** or **false** | **false** | | if true, this cultivation method can be skipped if it's ealiest starting date is past the current date (e.g. when climate data start later than the first workstep in the cultivation method); if false, MONICA will wait until the **date** (condition) of the first workstep is reached, which might never happen if the **date** is in the past
**is-cover-crop** | **true** or **false** | **false** | | if true, the cultivation method describes a catch or cover crop and similarly to **can-be-skipped** will be skipped if the previous primary crop was havested later then the (latest) sowing **date** (in contrast to the earliest workstep **date** using **can-be-skipped**); if false MONICA might not continue the crop rotation because it is waiting for the the cover/catch crop cultivation method to start (sowing workstep), whose (latest) sowing **date** is already in the past


## Worksteps

The following table shows the worksteps available in MONICA. All worksteps expect a **date** parameter to be set. If the **date** is missing the workstep is being interpreted as a dynamic workstep which in some way needs conditional properties to be set, which are being checked every day. In case the conditions hold true, the workstep is being executed once per **CultivationMethod**.

Every non-dynamic workstep (e.g. Sowing, Harvest, Tillage, SetValue etc) can have a **days** (default 0)[d] and an **after** [event-name] property. If both are set, the workstep will be executed **days** after the event **after** happenend.

A workstep date can be a relative date (e.g. **"0000-12-01"** (every december 1st) or **"0001-12-01"** (every next year december 1st)), which means its year part is a four digit number between 0 and 100, padded with 0s; or a normal date (**"2017-12-01"** december 1st 2017). A **crop-rotation** with relative dates will simply be applied to all available (selected) climate data, wraps around when its end is reached and starts anew. An absolute **crop-rotation** will only be applied if the worksteps' dates are reached, thus also care has to be taken that a workstep can be reached. For instance a workstep will never be executed if its **date** is set before the actual climate data start.

All worksteps have the parameters of the workstep **Workstep** in common. If no **date** is given it's a dynamic workstep and at least a parameter specifiying a condition should be defined, else the workstep will never be carried out.

### **Workstep**

Is the base workstep, does nothing but can be used in conjuction with the fired events **Workstep**. If no **date** is given **days** and **after** can be specified, or alternatively **at** which equals **after** with **days** set to 0. Then **days** after the event **after** happend, the workstep will be carried out. Please not that due to the internal workings of MONICA actually means that **after: 1** actually means basically the same as **after: 0**, because dynamic worksteps will be checked (and potentially executed) at the beginning of each day, but events like **anthesis** etc. will be issued later during the daily calculation. So there is no clear distinction between before or after a particular day as one has to know the implementation to decide what means before or after.

Fires the **Workstep** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**date** | iso-date <br> "YYYY-MM-DD" | | "1991-05-21" <br> "0001-05-21" | absolute or <br> relative date
**days** | [d] | 0 | |
**after** | string | | "Harvest" | "event/workstep name"
**at** | string | | "Harvest" | "event/workstep name"

### **Sowing**

Define the sowing time with the **date** parameter and the crop specified with the **crop** parameter will be planted.

Fires the **Sowing** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**crop** | **JSON object** | | | a complex **JSON object** specifying the crop to be planted
**PlantDensity** | [plants m-2] | | **10** for maize | plant density

#### **crop** JSON object

The key **crop** references a complex **JSON** structure describing the crop to be planted. It might look like:

```Json
{
  "is-winter-crop": false,
  "is-perennial-crop": true,
  "cropParams": {
    "species": ["include-from-file", "crops/clover-grass-ley.json"],
    "cultivar": ["include-from-file", "crops/clover-grass-ley/.json"]
  },
  "residueParams": ["include-from-file", "crop-residues/clover-grass-ley.json"]
}
```

If the to be planted crop is a perennial crop **is-perennial-crop** should be set to true, even though the cultivar parameters have a key **Perennial** which can define at the cultivar level that a crop is by default a perennial crop. If **is-perennial-crop** is given it will overwrite the key **Perennial**. If **is-perennial-crop** is set to **true**, but no **perennialCropParams** key is specified, then the **perennialCropParams** will be the same as **cropParams**. 

Both **cropParams** and **residueParams** are compulsory keys. 

If **is-winter-crop** is not given, then a crop will be defined as a winter crop if its sowing day of year is after the havesting day of year. **is-winter-crop** will currently be used to decide when to apply the N-Min method (if activated). If **true** the N-min method will be used at the defined DOY (eg. DOY = 89). If the crop is a summer crop, the N-min method will be carried out at sowing time.


Key name | unit/type | default | optional? | description
-------- | --------- | ------- | ------- | -----------
**cropParams** | JSON object | | | contains the MONICA species and cultivar parameters
**residueParams** | JSON object |  |  | the MONICA residue parameters 
**perennialCropParams** | JSON object | = **cropParams** if **is-perennial-crop** = **true** | x | **cropParams** for the years after the planting year
**is-winter-crop** | **true** or **false** | if DOY(seed-date) > DOY(harvest-date) <br> **true** else **false**  | x | 
**is-perennial-crop** | **true** or **false** | **false** | x | 
**cuttingDates** | JSON array of iso-dates | [] | x | define a list of cutting dates for the crop, is a shortcut for the same amount of **Cutting** worksteps

#### **cropParams**/**perennialCropParams** JSON object

Key name | unit/type | description
-------- | --------- | -----------
**species** | JSON object | the MONICA species parameters 
**cultivar** | JSON object | the MONICA cultivar specific parameters



### **Automatic Sowing**

An automatic sowing workstep, whose algorithm will start to check all conditions at **earliest-data** and will nevertheless sow if no conditions have been met at **latest-date**.

**conditions**

- **min-temp** && **days-in-temp-window**:
  - **spring crops**: the daily minimum air temperature (**tmin**) >= **min-temp** as well as the average temperature (**tavg**) during the time window **days-in-temp-window** has to be >= **min-temp**
  - **winter crops**: sowing is possible when the running average air temperature (**tavg**) within the time window **days-in-temp-window** <= **min-temp**
- **min-%-asw** && **max-%-asw**: the bounds (in % of available soil water) wherein the soil moisture (0-10cm) **["Mois", 1]** has to lie for possible sowing
- **max-3d-precip**: the maximum precipitation sum [mm] for the last 3 days (incl. actual day) for possible sowing
- **max-curr-day-precip**: the maximum precipitation sum [mm] for the actual day for possible sowing
- **temp-sum-above-base-temp** && **base-temp**: **temp-sum-above-base-temp** is the temperature sum above **base-temp** needed for sowing

Fires the **AutomaticSowing** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**crop** | JSON object
**earliest-date** | iso-date | | | earliest date when sowing might take place
**latest-date** | iso-date | | | last date when sowing will in any case happen
**min-temp** | [°C] | | | minimal temperature to be reached
**days-in-temp-window** | [d] | | | no of days the minimal temperature has to be reached
**min-%-asw** | [%] | | | percentage of minimal available soil water
**max-%-asw** | [%] | | | percentage of maximal available soil water
**max-3d-precip** | [mm] | | | maximal 3 day precipitation sum
**max-curr-day-precip** | [mm] | | | maximal current day precipitation
**temp-sum-above-base-temp** | [K] | | | temperature sum above base temperature to be reached
**base-temp** | [°C] | | | base temperature for calculation of temperature sum

### **Harvest**

Define the harvesting time of the previously sown crop. There whether to **export** (= add only roots and residues of plant to AOMs).

Fires **Harvest** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**exported** | **true** or **false** | true | | if exported = false, then the whole crop will be incorporated into the soil, else just roots and residues. If no particular export rules for the organs are defined (see below), then the **cultivar parameters** **OrganIdsForPrimaryYield** and **OrganIdsForSecondaryYield** are used. If any parameter like **leaf** or **fruit** etc is defined the **export** parameter is ignored and the more specific ones are used.
**leaf** and/or **fruit** and/or **shoot** and/or **struct** and/or **sugar** | JSON object | | {"export": [85, "%"], "incorporate": true} | Tells **MONICA** to **export** a certain **percentage** (if missing, **defaults to 100%**) of the organ used as parameter (e.g. **leaf**) and **incorporate** the residues into the soil (~~or leave the them as layer on the ground~~ -> right now always true and not changeable!) (if missing, **defaults to true**). Missing organs are being treated as residues and will by default be **incorporated** (as if specified **{"export": 0, "incorporate": true}**).


### **AutomaticHarvest**

An automatic harvesting workstep, which will be carried out if conditions are met after **maturity** of the crop or if no conditions have been met at **latest-date**.

**conditions**

- **min-%-asw** && **max-%-asw**: the bounds (in % of available soil water) wherein the soil moisture (0-10cm) **["Mois", 1]** has to lie for possible harvesting
- **max-3d-precip**: the maximum precipitation sum [mm] for the last 3 days (incl. actual day) for possible harvesting
- **max-curr-day-precip**: the maximum precipitation sum [mm] for the actual day for possible harvesting

Fires **AutomaticHarvest** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**latest-date** | iso-date | | | last date when harvesting will in any case happen
**min-%-asw** | [%] | | | percentage of minimal available soil water
**max-%-asw** | [%] | | | percentage of maximal available soil water
**max-3d-precip** | [mm] | | | maximal 3 day precipitation sum
**max-curr-day-precip** | [mm] | | | maximal current day precipitation


### **Cutting**

Cut a crop. 

The parameters **organs**, **export** and **cut-max-assimilation-rate** help to parameterize the cutting operation. **organs** tells which organs are to be cut (or left on the plot), while **exports** optionally tells how much of each cut organ part to export (the rest will be left on the plot and added to the organic matter pools). If no **organs** are being defined, then the cultivar parameter **OrganIdsForCutting** will be used (**yieldPercentage** and **organId**). To specify which organ to cut or how much to export, the same organ names can be used as in the specification of the outputs section (**"Root", "Leaf", "Shoot", "Fruit", "Struct", "Sugar"**). To further specify how much of an organ to cut, a second parameter to the organ can be a **unit**, which specifies the **biomass (kg ha-1)**, the **LAI (m2 m-2)** or (the **default**) a **percentage (%)**. A third parameter to the JSON array for the value in the **organs** array can be **left** or **cut** (**default**). Specifiying either how much to cut or how much to leave on the plot.

Fires **Cutting** event.


Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**organs** | JSON-object | | ```{"Leaf": 85, "Fruit": [100, "%"]}``` or ```{"Leaf": [1.5, "m2 m-2", "left"], "Fruit": [50, "%", "cut"]}``` | a mapping of organs to percentage to cut
**export** | JSON-object or (**true** or **false**) | **true** | ```{"Leaf": 100, "Fruit": 0}``` | a mapping of organs to percentage to export after cut or just **true** or **false** if everything or nothing should be exported
**cut-max-assimilation-rate** | [%] | **100** | | a percentage by which the maximal assimilation rate will be cut


### **MineralFertilization**

Apply **amount** kg N fertilizer, whose composition is described by **partition**. The partition object can also be referenced by include from a **JSON file**.

Fires **MineralFertilization** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**partition** | JSON object | | ```{"Carbamid": 0, "NH4": 0.5, "NO3": 0.5}``` | Json object describing the composition of the N fertilizer
**amount** | [kg N] | | | amount of N to put into the soil


### **NDemandFertilization**

Apply a calculated amount of N fertilizer, whose composition is described by **partition** (and can also be referenced by include from a **JSON file**) at either the given workstep's **date** or if there's no date, when stage **stage** is being entered. The measurement of existing N in the soil will happen until depth **depth**.

Fires **NDemandFertilization** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**partition** | JSON object | | ```{"id": "AN",	"name": "ammonium nitrate", "Carbamid": 0, "NH4": 0.5, "NO3": 0.5}``` | Json object describing the composition of the N fertilizer
**N-demand** | [kg N] | | | the amount of N which is demanded by the crop (= the difference to the actually available N will be given)
**depth** | [m]
**stage** | dev stage [1-6/7] | | 1 (sowing) or<br> 6 (maturity of maize) | MONICA development stage number


### **OrganicFertilization**

Apply **amount** kg organic fertilizer described by the **parameters** **JSON object** and incorporate if **incorporation** is set to **true**.

Fires **OrganicFertilization** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**parameters** | JSON object | | ```{"id": "CAM", "name": "cattle manure", "AOM_DryMatterContent": [0.196, "kg DM kg FM-1"], "AOM_FastDecCoeffStandard": [0.002, "d-1"], "AOM_NH4Content": [0.007, "kg N kg DM-1"], "AOM_NO3Content": [0, "kg N kg DM-1"], "AOM_SlowDecCoeffStandard": [0.0002, "d-1"], "CN_Ratio_AOM_Fast": 6.5, "CN_Ratio_AOM_Slow": 100, "NConcentration": 0, "PartAOM_Slow_to_SMB_Fast": [1, "kg kg-1"], "PartAOM_Slow_to_SMB_Slow": [0, "kg kg-1"], "PartAOM_to_AOM_Fast": [0.18, "kg kg-1"], "PartAOM_to_AOM_Slow": [0.72, "kg kg-1"]}``` | Json object describing the composition of the organic fertilizer
**amount** | [kg] | | [30000, "kg"]
**incorporation** | **true** or <br> **false**

### **Tillage**

Till up to **depth** m.

Fires **Tillage** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**depth** | [m]


### **Irrigation**

Irrigate **amount** of water with optionally given nitrate/sulfate concentrations.

Fires **Irrigation** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**parameters** | JSON object | | ```{"nitrateConcentration": [mg dm-3], "sulfateConcentration": [mg dm-3]}```
**amount** | [mm]


### **SetValue**

Set at the given **date** the **setable output expression** to either just a **value** (can be a scalar (5.0) or a **JSON array** if setable output expression is itself a **JSON array** (e.g. output expression is **["Mois, [1,3]]**, then value could be **[20, 30, 20]**)) or a simple arithmetic expression where **OP** can be [**+, -, \*, /**] and either side either a value or an output expression. **ONLY FEW SETABLE OUTPUT EXPRESSIONS ARE AVAILABLE CURRENTLY.**

Fires **SetValue** event.

Parameter name | unit/type | default | example | description
-------------- | --------- | ------- | ------- | -----------
**var** | setable output expression <br> | | ```["NO2", 1]``` | the setable variable to assign a value to
**value** | value or <br> JSON array <br> ```["=", output-name or value, OP, output-name or value]```] | | ```["=", ["NO2", 1], "*", 0.5]``` | an expression or number like the ones to be used to define outputs

Some examples for the **SetValue** workstep.

Set on every september 22nd the NO2 value in layer 0 (first layer) to the value which will be calculated by layer 1 * 0.5:

```json
{ "date": "0000-09-22", "type": "SetValue", "var": ["NO2", 1], "value": ["=", ["NO2", 1], "*", 0.5] }
```

Double on every september 23rd the NO3 values in the layers 1-10:

```json
{ "date": "0000-09-23", "type": "SetValue", "var": ["NO3", [1, 10]], "value": ["=", ["NO3", [1, 10]], "*", 2] }
```

Set on every september 24th the NH4 value in layer 1 to the value it already has (actually senseless):

```json
{ "date": "0000-09-24", "type": "SetValue", "var": ["NH4", 1], "value": ["NH4", 1] }
```

Set on every september 25th the carbamid value in allen layers to 0.01

```json
{ "date": "0000-09-25", "type": "SetValue", "var": ["Carb", [1,20]], "value": 0.01 }
```



## Example **crop.json** file

```json
{
  "crops": {
    "WR": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/rye.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/rye/winter rye.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/rye.json"]
    },
    "SM": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/maize.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/maize/silage maize.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/maize.json"]
    },
    "MEP": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/potato.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/potato/moderately early potato.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/potato.json"]
    },
    "WW": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/wheat.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/wheat/winter wheat.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/wheat.json"]
    },
    "WG": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/barley.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/barley/winter barley.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/barley.json"]
    },
    "SG": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/barley.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/barley/spring barley.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/barley.json"]
    },
    "SC": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/rape.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/rape/winter rape.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/rape.json"]
    }
  },

  "cropRotation": [
    {
      "worksteps": [
        { "date": "1991-09-23", "type": "Sowing", "crop": ["ref", "crops", "WR"] },
        {
          "date": "1992-05-05",
          "type": "Irrigation",
          "amount": [1.0, "mm"],
          "parameters": {
            "nitrateConcentration": [0.0, "mg dm-3"],
            "sulfateConcentration": [334, "mg dm-3"]
          }
        },
        {
          "date": "1992-04-03",
          "type": "MineralFertilization",
          "amount": [40.0, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1992-05-07",
          "type": "MineralFertilization",
          "amount": [40.0, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        { "date": "1992-07-27", "type": "Harvest", "crop": ["ref", "crops", "WR"] }
      ]
    },
    {
      "worksteps": [
        {
          "date": "1993-04-23",
          "type": "MineralFertilization",
          "amount": [125, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        { "date": "1993-04-27", "type": "Tillage", "depth": [0.15, "m"] },
        { "date": "1993-05-04", "type": "Sowing", "crop": ["ref", "crops", "SM"] },
        {
          "date": "1993-05-10",
          "type": "MineralFertilization",
          "amount": [60, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        { "date": "1993-09-23", "type": "Harvest", "crop": ["ref", "crops", "SM"] },
        {
          "date": "1993-12-16",
          "type": "OrganicFertilization",
          "amount": [30000, "kg N"],
          "parameters": ["include-from-file", "../monica-parameters/organic-fertilisers/CADLM.json"],
          "incorporation": true
        },
        { "date": "1993-12-22", "type": "Tillage", "depth": [0.1, "m"] }
      ]
    },
    {
      "worksteps": [
        { "date": "1994-04-25", "type": "Sowing", "crop": ["ref", "crops", "MEP"] },
        {
          "date": "1994-05-04",
          "type": "MineralFertilization",
          "amount": [90, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        { "date": "1994-09-06", "type": "Harvest", "crop": ["ref", "crops", "MEP"] },
        { "date": "1994-09-29", "type": "Tillage", "depth": [0.15, "m"] }
      ]
    },
    {
      "worksteps": [
        { "date": "1994-10-11", "type": "Sowing", "crop": ["ref", "crops", "WW"] },
        {
          "date": "1995-03-24",
          "type": "MineralFertilization",
          "amount": [60, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1995-04-27",
          "type": "MineralFertilization",
          "amount": [40, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1995-05-12",
          "type": "MineralFertilization",
          "amount": [60, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        { "date": "1995-08-02", "type": "Harvest", "crop": ["ref", "crops", "WW"] },
        { "date": "1995-08-03", "type": "Tillage", "depth": [0.15, "m"] }
      ]
    },
    {
      "worksteps": [
        { "date": "1995-09-07", "type": "Sowing", "crop": ["ref", "crops", "WG"] },
        {
          "date": "1996-03-21",
          "type": "MineralFertilization",
          "amount": [60, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        { "date": "1996-04-13", "type": "Harvest", "crop": ["ref", "crops", "WG"] },
        { "date": "1996-04-14", "type": "Tillage", "depth": [0.10, "m"] }
      ]
    },
    {
      "worksteps": [
        { "date": "1996-04-17", "type": "Sowing", "crop": ["ref", "crops", "SG"] },
        { "date": "1996-09-10", "type": "Harvest", "crop": ["ref", "crops", "SG"] },
        { "date": "1996-09-17", "type": "Tillage", "depth": [0.10, "m"] }
      ]
    },
    {
      "worksteps": [
        { "date": "1997-04-04", "type": "Sowing", "crop": ["ref", "crops", "SC"] },
        {
          "date": "1997-04-10",
          "type": "MineralFertilization",
          "amount": [80, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        { "date": "1997-07-08", "type": "Harvest", "crop": ["ref", "crops", "SC"] },
        { "date": "1997-07-09", "type": "Tillage", "depth": [0.10, "m"] }
      ]
    }
  ],

  "CropParameters": {
    "DEFAULT": ["include-from-file", "../monica-parameters/user-parameters/hermes-crop.json"]
  }
}
```






***

***

***

***

***

***
