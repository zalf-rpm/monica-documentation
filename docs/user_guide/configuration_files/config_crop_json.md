# The `crop.json` configuration file

`crop.json` defines all crop-specific input data for MONICA. The top-level JSON object must contain exactly two keys: `cropRotation` and `CropParameters`. 

* `cropRotation` is a **JSON array** of objects, each representing a cultivation method (class `CultivationMethod` in the MONICA C++ code). 
* `CropParameters` contains global crop-related MONICA parameters, which are taken either from the **user-parameter** database table or a referenced file.

---

## Cultivation Methods

A **cultivation method** is defined as a set of **worksteps** (class hierarchy `Workstep` in the MONICA code). The **order** of the worksteps is **important**. Putting static worksteps (with fixed relative or absolute dates) in the wrong order may prevent them from being executed. 

Worksteps can represent sowing, harvesting, fertilization, irrigation, or tillage applications. A sowing workstep tells MONICA when and which crop to grow. Within the sowing workstep, the `crop` parameter must contain a JSON object with **all** crop parameters MONICA requires. This can be defined inline, included via `include-from-db` or `include-from-file`, or referenced using `ref`. 

Because crops might be used in more than one workstep or cultivation method, the example below (at the end of this section) defines all crops in a separate JSON object named `crops`, which maps shortcut names to crop parameter objects. The mapping object is stored under the top-level key `crops`, which becomes the first parameter to the `ref` function, with the second being the crop's shortcut name. The names (`crops`, `WR`, `SM`, etc.) are arbitrary.

A cultivation method can also have parameters, as described in the following table:

| Parameter name        | Unit/Type                 | Default     | Example | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------|---------------------------|-------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`can-be-skipped`**  | **`true`** or **`false`** | **`false`** |         | If **true**, this cultivation method can be skipped if its earliest starting date is before the current date (e.g., when the climate data start later than the first workstep in the cultivation method). If **false**, MONICA will wait until the **date** (condition) of the first workstep is reached, which might never happen if that date is in the past.                                                                                               |
| **`is-cover-crop`**   | **`true`** or **`false`** | **`false`** |         | If **true**, the cultivation method describes a catch or cover crop. Similarly to `can-be-skipped`, it will be skipped if the previous primary crop was harvested later than the latest sowing **date** (in contrast to the earliest workstep **date** using `can-be-skipped`). If **false**, MONICA might not continue the crop rotation because it waits for the the cover/catch crop sowing workstep, whose **latest** sowing date is already in the past. |

---

## Worksteps

The following table lists all worksteps available in MONICA. All worksteps require a `date` parameter. If `date` is missing, the workstep is interpreted as **dynamic**, meaning it depends on one or more **conditions** that MONICA checks daily. Once the conditions are met, the workstep is executed once per **CultivationMethod**.

Every non-dynamic workstep (e.g., `Sowing`, `Harvest`, `Tillage`, `SetValue`, etc.) can have a `days` (default 0[d]) and an `after` ([event-name]) property. If both are set, the workstep will be executed `days` after the `after` event occurs.

A workstep date can be **relative** (e.g., `"0000-12-01"` for every December 1st, or `"0001-12-01"` for the next year's December 1st), which means its year part is a four-digit number between 0 and 100, padded with 0s, or **absolute** (e.g., `"2017-12-01"` for December 1st, 2017). 

* In **relative crop rotation**, worksteps are repeated throughout the available climate data period, wrapping around when the end is reached. 
* In an **absolute crop rotation**, worksteps are only executed when their dates occur within the simulation period, so care must be taken to ensure they are reachable (e.g., a workstep before the start of the climate date will never execute).

All worksteps share the parameters of the `Workstep` base class. If `date` is not given, at least one conditional parameter must be defined. Otherwise, the workstep will never be executed.

---

### **Workstep**

The base workstep itself does nothing but can be used in conjunction with the `Workstep` event. If no `date` is given, `days` and `after` can be specified, or alternatively `at`, which is equivalent to `after` with `days` set to 0. Then the workstep will be executed the specified number of `days` after the `after` event occurs. 

!!! note
    Due to MONICA's internal scheduling, `after: 1` effectively behaves like `after: 0`, because dynamic worksteps are checked (and potentially executed) at the start of each day, while events like `anthesis`, etc. are triggered later in the daily calculation. Therefore, "before" and "after" distinctions may depend on implementation details.

Fires the `Workstep` event.

| Parameter name   | Unit/Type                 | Default | Example                          | Description               |
|------------------|---------------------------|---------|----------------------------------|---------------------------|
| **date**         | ISO date `"YYYY-MM-DD"`   |         | `"1991-05-21"` or `"0001-05-21"` | Absolute or relative date |
| **days**         | [d]                       | 0       |                                  | Days after event          |
| **after**        | string                    |         | `"Harvest"`                      | Event/workstep name       |
| **at**           | string                    |         | `"Harvest"`                      | Event/workstep name       |

---

### **Sowing**

Defines the sowing date via the `date` parameter, and specifies the crop to be planted via the `crop` parameter.

Fires the `Sowing` event.

| Parameter name      | Unit/Type    | Default  | Example        | Description                              |
|---------------------|--------------|----------|----------------|------------------------------------------|
| **`crop`**          | JSON object  |          |                | JSON object specifying the crop to plant |
| **`PlantDensity`**  | [plants m-2] |          | `10` for maize | Plant density                            |

#### **crop** JSON object

The `crop` key references a complex JSON structure describing the crop to be planted. It might look like:

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

If the crop is a perennial, `is-perennial-crop` should be set to **true**, even though the cultivar parameters may already contain the key `Perennial`, which can define at the cultivar level that a crop is by default a perennial crop. 

If `is-perennial-crop` is given, it overrides `Perennial`. 

If `is-perennial-crop` is set to **true** but no `perennialCropParams` key is specified, then the **perennialCropParams** will be the same as **cropParams**. 

Both `cropParams` and `residueParams` are compulsory keys. 

If `is-winter-crop` is not specified, MONICA determines it automatically. A crop is treated as a winter crop if its sowing day of year (DOY) is after its harvest DOY. `is-winter-crop` is currently used to determine when the N-Min method (if activated) is applied. If **true**, the N-Min method is applied on the defined DOY (e.g., DOY = 89). For summer crops, the N-Min method is applied at sowing.

| Key name                   | Unit/Type               | Default                                                           | Optional? | Description                                                                                                               |
|----------------------------|-------------------------|-------------------------------------------------------------------|-----------|---------------------------------------------------------------------------------------------------------------------------|
| **`cropParams`**           | JSON object             |                                                                   |           | Contains the MONICA species and cultivar parameters                                                                       |
| **`residueParams`**        | JSON object             |                                                                   |           | The MONICA residue parameters                                                                                             |
| **`perennialCropParams`**  | JSON object             | = `cropParams` if `is-perennial-crop` = **true**                  | x         | The `cropParams` used for the years after the planting year                                                               |
| **`is-winter-crop`**       | **true** or **false**   | If DOY(seed-date) > DOY(harvest-date) -> **true**, else **false** | x         | Defines whether the crop is a winter crop. If not provided, MONICA derives it automatically from sowing and harvest DOYs. |
| **`is-perennial-crop`**    | **true** or **false**   | **false**                                                         | x         | Defines whether the crop is perennial. Overrides the cultivar's `Perennial` parameter if given.                           |
| **`cuttingDates`**         | JSON array of ISO dates | []                                                                | x         | Defines a list of cutting dates for the crop. Serves as a shortcut for the same number of **Cutting** worksteps.          |

#### `cropParams`/`perennialCropParams` JSON object

| Key name        | Unit/Type   | Description                             |
|-----------------|-------------|-----------------------------------------|
| **`species`**   | JSON object | The MONICA species parameters           |
| **`cultivar`**  | JSON object | The MONICA cultivar-specific parameters |

---

### **Automatic Sowing**

An **automatic sowing** workstep whose algorithm starts checking conditions from `earliest-date` onward. If no conditions are met before `latest-date`, sowing will still occur on that date.

**Conditions**

- `min-temp` and `days-in-temp-window`:
  - **Spring crops**: sowing is possible if the daily minimum air temperature (`tmin`) >= `min-temp` and the average temperature (`tavg`) over the time window `days-in-temp-window` >= `min-temp`.
  - **Winter crops**: sowing is possible if the running average air temperature (`tavg`) within `days-in-temp-window` <= `min-temp`.
- `min-%-asw` and `max-%-asw`: bounds (in % of available soil water) within which the 0-10 cm soil moisture (`["Mois", 1]`) must lie for possible sowing.
- `max-3d-precip`: maximum 3-day precipitation sum [mm] (including the current day) for possible sowing
- `max-curr-day-precip`: maximum daily precipitation sum [mm] allowed on the current day for possible sowing
- `temp-sum-above-base-temp` and `base-temp`: defines the temperature sum above `base-temp` required for sowing

Fires the `AutomaticSowing` event.

| Parameter name                  | Unit/Type   | Default  | Example  | Description                                                     |
|---------------------------------|-------------|----------|----------|-----------------------------------------------------------------|
| **`crop`**                      | JSON object |          |          | The crop to be sown                                             |
| **`earliest-date`**             | ISO date    |          |          | Earliest date when sowing may take place                        |
| **`latest-date`**               | ISO date    |          |          | Latest date by which sowing will occur regardless of conditions |
| **`min-temp`**                  | [°C]        |          |          | Minimum temperature required                                    |
| **`days-in-temp-window`**       | [d]         |          |          | Number of consecutive days the minimum temperature must be met  |
| **`min-%-asw`**                 | [%]         |          |          | Minimum percentage of available soil water                      |
| **`max-%-asw`**                 | [%]         |          |          | Maximum percentage of available soil water                      |
| **`max-3d-precip`**             | [mm]        |          |          | Maximum precipitation sum over the last 3 days                  |
| **`max-curr-day-precip`**       | [mm]        |          |          | Maximum precipitation allowed on the current day                |
| **`temp-sum-above-base-temp`**  | [K]         |          |          | Required temperature sum above base temperature                 |
| **`base-temp`**                 | [°C]        |          |          | Base temperature used to calculate temperature sums             |

---

### **Harvest**

Defines the harvesting time of the previously sown crop and specifies whether to **export** biomass (add only roots and residues to AOMs).

Fires `Harvest` event.

| Parameter name                                                                          | Unit/Type             | Default  | Example                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------------------------------------------------------------------------------------|-----------------------|----------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`exported`**                                                                          | **true** or **false** | **true** |                                              | If **exported = false**, the whole crop is incorporated into the soil. If **true**, only roots and residues remain. If no particular export rules for the organs are defined (see below), then the **cultivar parameters** `OrganIdsForPrimaryYield` and `OrganIdsForSecondaryYield` are used. If organ-specific parameters (e.g., `leaf`, `fruit`, etc.) are provided, the `export` parameter is ignored and the more specific ones are used.       |
| **`leaf`** and/or **`fruit`** and/or **`shoot`** and/or **`struct`** and/or **`sugar`** | JSON object           |          | `{"export": [85, "%"], "incorporate": true}` | Tells **MONICA** to **export** a certain **percentage** (if missing, **defaults to 100%**) of the organ used as parameter (e.g., `leaf`) and `incorporate` the residues into the soil (~~or leave the them as layer on the ground~~ -> right now always true and not changeable!) (if missing, **defaults to true**). Missing organs are treated as residues and **incorporated** by default (as if specified `{"export": 0, "incorporate": true}`). |

---

### **AutomaticHarvest**

An **automatic harvesting** workstep that is triggered once conditions are met after crop **maturity**, or on `latest-date` if conditions remain unmet.

**Conditions**

- `min-%-asw` and `max-%-asw`: soil moisture bounds (in % of available soil water) wherein the soil moisture (0-10cm) `["Mois", 1]` has to lie for possible harvesting
- `max-3d-precip`: maximum 3-day precipitation sum [mm] (including current day) for possible harvesting
- `max-curr-day-precip`: maximum precipitation sum [mm] on the current day for possible harvesting

Fires `AutomaticHarvest` event.

| Parameter name             | Unit/Type | Default | Example | Description                                               |
|----------------------------|-----------|---------|---------|-----------------------------------------------------------|
| **`latest-date`**          | ISO date  |         |         | Latest possible harvest date                              |
| **`min-%-asw`**            | [%]       |         |         | Percentage of minimum available soil water for harvesting |
| **`max-%-asw`**            | [%]       |         |         | Percentage of maximum available soil water for harvesting |
| **`max-3d-precip`**        | [mm]      |         |         | Maximum 3-day precipitation sum                           |
| **`max-curr-day-precip`**  | [mm]      |         |         | Maximum current day precipitation                         |

---

### **Cutting**

Performs a cutting operation on the crop. 

The parameters `organs`, `export`, and `cut-max-assimilation-rate` help to parameterize the cutting operation. 

* `organs` specifies which crop organs are cut or left on the plot
* `exports` specifies how much of each cut organ part to export (the rest will be left on the plot and added to the organic matter pools). 

If `organs` are not defined, MONICA uses the cultivar parameters `OrganIdsForCutting` (`yieldPercentage` and `organId`). To specify which organ to cut or how much to export, the same organ names can be used as in the specification of the outputs section (`"Root"`, `"Leaf"`, `"Shoot"`, `"Fruit"`, `"Struct"`, `"Sugar"`). To further specify how much of an organ to cut, a second parameter to the organ can be a **unit**, which specifies the **biomass (kg ha-1)**, the **LAI (m2 m-2)** or **percentage (%)** (default). A third parameter to the JSON array for the value in the **organs** array can be **left** or **cut** (default), specifying either how much to cut or how much to leave on the plot.

Fires `Cutting` event.

| Parameter name                  | Unit/Type                           | Default  | Example                                                                                               | Description                                                                                                                   |
|---------------------------------|-------------------------------------|----------|-------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| **`organs`**                    | JSON object                         |          | `{"Leaf": 85, "Fruit": [100, "%"]}` or `{"Leaf": [1.5, "m2 m-2", "left"], "Fruit": [50, "%", "cut"]}` | Mapping of organs to percentage to cut                                                                                        |
| **`export`**                    | JSON object or (**true**/**false**) | **true** | `{"Leaf": 100, "Fruit": 0}`                                                                           | Mapping of organs to percentage to export after cut or just **true** or **false** if everything or nothing should be exported |
| **`cut-max-assimilation-rate`** | [%]                                 | **100**  |                                                                                                       | Percentage reduction in the maximum assimilation rate                                                                         |

---

### **MineralFertilization**

Applies a given **amount** nitrogen (kg N) fertilizer, whose composition is described by `partition`. The `partition` object can also be included from an external **JSON file**.

Fires `MineralFertilization` event.

| Parameter name    | Unit/Type   | Default | Example                                   | Description                                                       |
|-------------------|-------------|---------|-------------------------------------------|-------------------------------------------------------------------|
| **`partition`**   | JSON object |         | `{"Carbamid": 0, "NH4": 0.5, "NO3": 0.5}` | JSON object describing the composition of the nitrogen fertilizer |
| **`amount`**      | [kg N]      |         |                                           | Amount of nitrogen fertilizer to be applied to the soil           |

### **NDemandFertilization**

Applies a **calculated** amount of nitrogen fertilizer based on the crop's N demand. The composition of the fertilizer is described by `partition`, which can also be included from an external **JSON file**. The fertilizer occurs either at the specified `date` or, if no date is given, when the specified development stage `stage` is reached. The soil nitrogen status is measured down to the given `depth`.

Fires `NDemandFertilization` event.

| Parameter name    | Unit/Type                 | Default | Example                                                                           | Description                                                                                                   |
|-------------------|---------------------------|---------|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| **`partition`**   | JSON object               |         | `{"id": "AN",	"name": "ammonium nitrate", "Carbamid": 0, "NH4": 0.5, "NO3": 0.5}` | JSON object describing the composition of the nitrogen fertilizer                                             |
| **`N-demand`**    | [kg N]                    |         |                                                                                   | The crop's nitrogen demand. The model applies the difference between the demand and the actually available N. |
| **`depth`**       | [m]                       |         |                                                                                   | Depth down to which soil nitrogen is measured                                                                 |
| **`stage`**       | Development stage [1-6/7] |         | 1 (sowing) or 6 (maturity of maize)                                               | MONICA development stage number at which fertilization occurs (if no date is specified)                       |


### **OrganicFertilization**

Applies an organic fertilizer of the specified **amount** (kg), described by the `parameters` JSON object. The material is incorporated into the soil if `incorporation` is set to **true**.

Fires `OrganicFertilization` event.

| Parameter name       | Unit/Type             | Default | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Description                                                                                        |
|----------------------|-----------------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **`parameters`**     | JSON object           |         | `{"id": "CAM", "name": "cattle manure", "AOM_DryMatterContent": [0.196, "kg DM kg FM-1"], "AOM_FastDecCoeffStandard": [0.002, "d-1"], "AOM_NH4Content": [0.007, "kg N kg DM-1"], "AOM_NO3Content": [0, "kg N kg DM-1"], "AOM_SlowDecCoeffStandard": [0.0002, "d-1"], "CN_Ratio_AOM_Fast": 6.5, "CN_Ratio_AOM_Slow": 100, "NConcentration": 0, "PartAOM_Slow_to_SMB_Fast": [1, "kg kg-1"], "PartAOM_Slow_to_SMB_Slow": [0, "kg kg-1"], "PartAOM_to_AOM_Fast": [0.18, "kg kg-1"], "PartAOM_to_AOM_Slow": [0.72, "kg kg-1"]}` | JSON object describing the composition and properties of the organic fertilizer                    |
| **`amount`**         | [kg]                  |         | [30000, "kg"]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Amount of organic fertilizer applied                                                               |
| **`incorporation`**  | **true** or **false** |         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Whether the fertilizer is incorporated into the soil (**true**) or left on the surface (**false**) |

---

### **Tillage**

Performs tillage down to a specified **depth** (m).

Fires `Tillage` event.

| Parameter name    | Unit/Type | Default | Example | Description   |
|-------------------|-----------|---------|---------|---------------|
| **`depth`**       | [m]       |         |         | Tillage depth |

---

### **Irrigation**

Applies an **amount** of irrigation water with optionally specified **nitrate** and **sulfate** concentrations.

Fires `Irrigation` event.

| Parameter name    | Unit/Type   | Default | Example                                                                  | Description                                                                           |
|-------------------|-------------|---------|--------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| **`parameters`**  | JSON object |         | `{"nitrateConcentration": [mg dm-3], "sulfateConcentration": [mg dm-3]}` | JSON object describing the nitrate and sulfate concentrations of the irrigation water |
| **`amount`**      | [mm]        |         |                                                                          | Amount of irrigation water to be applied                                              |

---

### **SetValue**

Sets the given **settable output expression** to a new value at the specified **date**. The **value** can be:

* A single scalar (e.g., `5.0`),
* A **JSON array** if settable output expression is also a **JSON array** (e.g., output expression is `["Mois, [1,3]]`, then value could be `[20, 30, 20]`), or
* An arithmetic expression where **OP** can be `+`, `-`, `*`, `/` and either side a value or an output expression. 

!!! note
    Only a limited set of variables are currently available.

Fires `SetValue` event.

| Parameter name    | Unit/Type                                                                   | Default | Example                       | Description                                                     |
|-------------------|-----------------------------------------------------------------------------|---------|-------------------------------|-----------------------------------------------------------------|
| **`var`**         | Settable output expression                                                  |         | `["NO2", 1]`                  | Variable or array element to set                                |
| **`value`**       | Value or JSON array `["=", output-name or value, OP, output-name or value]` |         | `["=", ["NO2", 1], "*", 0.5]` | Expression or number like the ones to be used to define outputs |

Some examples for the **SetValue** workstep:

Set every September 22 the NO<sub>2</sub> value in layer 0 (first layer) to the value which will be calculated by layer 1 * 0.5:

```json
{ "date": "0000-09-22", "type": "SetValue", "var": ["NO2", 1], "value": ["=", ["NO2", 1], "*", 0.5] }
```

Double the NO<sub>3</sub> values in layers 1-10 every September 23:

```json
{ "date": "0000-09-23", "type": "SetValue", "var": ["NO3", [1, 10]], "value": ["=", ["NO3", [1, 10]], "*", 2] }
```

Set the NH<sub>4</sub> value in layer 1 to its current value every September 24 (no effect):

```json
{ "date": "0000-09-24", "type": "SetValue", "var": ["NH4", 1], "value": ["NH4", 1] }
```

Set the carbamide (Carb) value in all layers to 0.01 every September 25:

```json
{ "date": "0000-09-25", "type": "SetValue", "var": ["Carb", [1,20]], "value": 0.01 }
```

---

## Example **crop.json** file

Below is an example `crop.json` file defining a complete crop rotation over multiple years.

It demonstrates:

* How to define crops and residues under the `"crops"` key.
* How to reference them in the `"cropRotation"` using the `["ref", "crops", "<crop code>"]` syntax.
* How to schedule various worksteps such as **sowing**, **fertilization**, **irrigation**, **tillage**, and **harvest**.
* How to include parameter files from external sources (via `include-from-file`).

This example illustrates a crop rotation that includes winter rye, silage maize, potato, winter wheat, winter barley, spring barley, and winter rape.

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