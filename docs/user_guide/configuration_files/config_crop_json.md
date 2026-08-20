# The `crop.json` configuration file

`crop.json` defines crop management schedules and global crop module parameters for MONICA.

A typical file contains the following top-level keys:

- `cropRotation`: an array of cultivation method objects. Each object is parsed as a `CultivationMethod` and normally contains a sequence of management operations under `worksteps`.
- `CropParameters`: global user parameters for MONICA's crop module. These parameters may be specified as an inline JSON object or loaded from another JSON file using `["include-from-file", "<path>"]`.

Additional top-level keys are allowed, for example reusable crop or fertilizer definitions referenced elsewhere in the file. MONICA also supports `cropRotations` for date-bounded rotations and the corresponding `cropRotation2` and `cropRotations2` keys for a second crop in intercropping configurations.

---

## Cultivation methods

A **cultivation method** is an ordered sequence of **worksteps**, represented by the `Workstep` class hierarchy in MONICA. Worksteps are processed in the order in which they appear in the `worksteps` array. The order is particularly important for static worksteps with fixed relative or absolute dates. Incorrect ordering can shift relative dates into the wrong year or prevent worksteps from being executed.

Worksteps can represent operations such as sowing, harvesting, fertilization, irrigation, and tillage. A sowing workstep specifies when a crop is sown and which crop is grown. Its `crop` property must resolve to a valid crop JSON object containing `cropParams`, with `species` and `cultivar` objects, and `residueParams`. The crop object can be written inline, loaded with `include-from-file`, or referenced with `ref`.

### Reusing crop definitions

Crops used by multiple worksteps or cultivation methods can be stored in a separate mapping. In the following example, the top-level `crops` object maps user-defined shortcut names to crop parameter objects:

```json
{
  "crops": {
    "WR": {
      "is-winter-crop": true,
      "cropParams": {
        "species": ["include-from-file", "crops/rye.json"],
        "cultivar": ["include-from-file", "crops/rye/winter-rye.json"]
      },
      "residueParams": ["include-from-file", "crop-residues/rye.json"]
    }
  }
}
```

A workstep can reference the crop as follows:

```json
{
  "type": "Sowing",
  "date": "0000-09-23",
  "crop": ["ref", "crops", "WR"]
}
```

Here, `crops` identifies the top-level mapping and `WR` identifies an entry within it. Both names are arbitrary and may be chosen by the user.

### Cultivation method parameters

A cultivation method supports the following parameters:

| Parameter name        | Type                  | Default | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|-----------------------|-----------------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`customId`**        | Integer               | `0`     | Optional identifier used to associate the cultivation method with an entity in another domain. It does not affect model calculations.                                                                                                                                                                                                                                                                                                                                                                                                  |
| **`name`**            | String                | unset   | Optional name for the cultivation method. When omitted, MONICA normally derives the name from the crop or uses `Fallow`.                                                                                                                                                                                                                                                                                                                                                                                                                |
| **`can-be-skipped`**  | Boolean | `false` | Controls what happens when MONICA reaches this cultivation method after its planned start. If `true`, MONICA may skip the entire method and continue with the next one in the crop rotation. For example, a method planned to start on March 10 may be skipped if MONICA reaches it on 20 March. If `false`, the method is not skipped automatically. It may be moved to a later date, or the crop rotation may stop if a required fixed date has already passed.                                                                      |
| **`is-cover-crop`**   | Boolean | `false` | Marks the cultivation method as a cover or catch crop. MONICA checks the crop's latest sowing date rather than the start of the whole method. If the cover crop can still be sown, the method is used. If the latest sowing date has passed, MONICA skips the entire method and continues with the next one. For example, if the sowing window ends on September 30, the cover crop can still be grown on September 20 but will be skipped on October 1. This is useful when the preceding main crop is harvested later than expected. |
| **`repeat`**          | Boolean | `true`  | Controls whether the cultivation method participates in later crop rotation cycles. When `false`, MONICA removes it after its first traversal.                                                                                                                                                                                                                                                                                                                                                                                         |
| **`worksteps`**       | Array of JSON objects | `[]`    | Ordered management operations belonging to the cultivation method. A productive cultivation method normally contains at least one workstep; an empty array can represent fallow or an intentionally empty method.                                                                                                                                                                                                                                                                                                                      |

### Example

```json
{
  "crops": {
    "WR": {
      "is-winter-crop": true,
      "cropParams": {
        "species": ["include-from-file", "crops/rye.json"],
        "cultivar": ["include-from-file", "crops/rye/winter-rye.json"]
      },
      "residueParams": ["include-from-file", "crop-residues/rye.json"]
    }
  },

  "cropRotation": [
    {
      "worksteps": [
        {
          "type": "Sowing",
          "date": "0000-09-23",
          "crop": ["ref", "crops", "WR"]
        },
        {
          "type": "Harvest",
          "date": "0001-07-27"
        }
      ]
    }
  ]
}
```

The relative years `0000` and `0001` express the intended sequence across a year boundary: sowing takes place first, followed by harvesting in the next calendar year. The worksteps must therefore remain in this order.

---

## Worksteps

The following sections describe the worksteps supported by MONICA.

A workstep can be scheduled in one of two ways:

- A **dated workstep** has a valid `date` and executes when that date is reached.
- A **dynamic workstep** has no valid `date` and is checked daily while its `CultivationMethod` is active.

Dated worksteps and dynamic worksteps use different scheduling rules. If a valid `date` is present, MONICA executes the workstep by date. The event-relative properties `days`, `after`, and `at` do not affect its scheduling.

Dynamic worksteps may use conditions defined by their concrete workstep type. Worksteps that use the base event condition require either:

```json
{
  "after": "Harvest",
  "days": 2
}
```

or the `at` shortcut:

```json
{
  "at": "Harvest"
}
```

`at: "<event>"` is equivalent to:

```json
{
  "after": "<event>",
  "days": 1
}
```

### Dated worksteps

A workstep date can be either relative or absolute.

#### Relative dates

A relative date uses a four-digit year between `0000` and `0099`. The year is interpreted as an offset from the year in which the `CultivationMethod` is initialized:

- `"0000-12-01"` means December 1 in the initialization year.
- `"0001-12-01"` means December 1 in the following year.
- `"0002-12-01"` means December 1 two years later.

For example, if the cultivation method is initialized for 2017:

| Workstep date  | Effective date |
|----------------|----------------|
| `"0000-12-01"` | 2017-12-01     |
| `"0001-12-01"` | 2018-12-01     |
| `"0002-12-01"` | 2019-12-01     |

Relative cultivation methods normally repeat when MONICA reaches the end of the crop rotation. Their dates are recalculated for the next rotation cycle.

A cultivation method does not repeat if its `repeat` property is `false`.

#### Absolute dates

A year of `0100` or greater is interpreted as an absolute year:

```json
{
  "date": "2017-12-01",
  "type": "Tillage"
}
```

Absolute worksteps execute only when their dates are reached during the simulation. They are not shifted into another year.

!!! warning

    Ensure that absolute dates are reachable and ordered consistently with the simulation and crop rotation. A workstep dated before the beginning of the simulation cannot execute. Unreachable dates may also prevent MONICA from advancing through the cultivation method as intended.

A cultivation method containing only absolute worksteps is removed from the repeating crop rotation after its first traversal. In a cultivation method containing both absolute and relative worksteps, the absolute dates remain fixed while the relative dates can be recalculated for later cycles.

### Dynamic worksteps

A workstep without a valid `date` is dynamic. MONICA checks active dynamic worksteps once at the beginning or end of each simulation day, depending on `runAtStartOfDay`.

Dynamic worksteps are checked only while their containing `CultivationMethod` is active.

Most event-relative worksteps finish after they execute and are therefore executed once per activation of the cultivation method. Some automatic worksteps have their own conditions and can remain active or execute repeatedly.

If a dynamic workstep has neither a type-specific condition nor a valid event-relative condition, it will never execute.

#### Event-relative scheduling

The common event-relative condition uses:

- `after`: the name of an event to wait for
- `days`: the delay counter after that event is detected
- `at`: shorthand for `after` with `days: 1`

Event names are case-sensitive. Examples include `"Harvest"`, `"Sowing"`, `"anthesis"`, and `"run-started"`.

The current implementation requires `days` to be greater than zero. Consequently, the following workstep will not execute:

```json
{
  "type": "Tillage",
  "after": "Harvest",
  "days": 0
}
```

Omitting `days` has the same effect because its default value is `0`. Use `at` or specify a positive `days` value.

!!! note

    Event-relative timing depends on when the source event and the dependent workstep are processed during the daily calculation.

    By default, dynamic worksteps are checked at the start of the day. Events such as `anthesis`, as well as dated workstep events, are usually generated later in the day. Such an event is therefore normally first detected by a start-of-day workstep on the following day. The delay counter advances on subsequent checks.

    As a result, `at: "anthesis"` does not necessarily execute on the calendar day on which `anthesis` is generated. Set `runAtStartOfDay` explicitly when the distinction between start-of-day and end-of-day execution matters.

If both `at` and the explicit properties are supplied, the explicit values take precedence:

```json
{
  "type": "Tillage",
  "at": "Harvest",
  "after": "Sowing",
  "days": 3
}
```

This is interpreted as `after: "Sowing"` with `days: 3`. Supplying both forms is discouraged because it makes the configuration harder to understand.

### Common workstep parameters

`Workstep` is the internal abstract base class for concrete worksteps such as `Sowing`, `Harvest`, `Tillage`, and `SetValue`.

Concrete worksteps inherit the following scheduling parameters:

| Parameter name        | Type    | Unit                    | Default | Example                          | Description                                                                              |
|-----------------------|---------|-------------------------|---------|----------------------------------|------------------------------------------------------------------------------------------|
| **`date`**            | String  | ISO date `"YYYY-MM-DD"` | unset   | `"1991-05-21"` or `"0001-05-21"` | Absolute or relative execution date                                                      |
| **`days`**            | Integer | d                       | `0`     | `2`                              | Delay counter used with `after`. Must be greater than zero for the base event condition. |
| **`after`**           | String  |                         | unset   | `"Harvest"`                      | Case-sensitive event name                                                                |
| **`at`**              | String  |                         | unset   | `"Harvest"`                      | Shortcut for `after` with `days: 1`                                                      |
| **`runAtStartOfDay`** | Boolean |                         | `true`  | `false`                          | Whether a dynamic workstep is checked at the start (`true`) or end (`false`) of the day  |

#### Generic `Workstep` event

The common workstep implementation adds the `"Workstep"` event when it is applied. Concrete worksteps generally also add a type-specific event, such as `"Sowing"`, `"Harvest"`, or `"Tillage"`.

Use the concrete event name when a dependent operation must follow a particular kind of workstep:

```json
{
  "type": "Tillage",
  "at": "Harvest",
  "depth": 0.15
}
```

Use `"Workstep"` only when the dependent operation is intended to react to the generic event emitted by compatible workstep implementations.

---

### **Sowing**

Plants a crop on the date specified by `date`. 

When applied, the workstep fires the generic `Workstep` event followed by the `Sowing` event.

| Parameter name     | Type        | Unit       | Default           | Required? | Example                                                 | Description                                                                                                                                                                                                |
|--------------------|-------------|------------|-------------------|-----------|---------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`type`**         | String      |            |                   | Yes       | `"type": "Sowing"`                                      | Workstep type                                                                                                                                                                                              |
| **`date`**         | String      | ISO date   | unset             | Conditional | `"date": "2026-04-15"` or `"date": "0000-04-15"`      | Date on which the crop is sown, for example `"2026-04-15"` or `"0000-04-15"` for an annually recurring date. Required for fixed-date sowing; event-relative scheduling can instead use `after` and `days`. |
| **`crop`**         | JSON object |            |                   | Yes       | `"crop": {"cropParams": {...}, "residueParams": {...}}` | Crop to sow                                                                                                                                                                                                |
| **`PlantDensity`** | Integer     | plants m-2 | species parameter | No        | `"PlantDensity": 10`                                    | Overrides the species-level `PlantDensity` when greater than zero                                                                                                                                          |

Example:

```json
{
  "type": "Sowing",
  "date": "0000-04-15",
  "PlantDensity": 10,
  "crop": {
    "is-winter-crop": false,
    "cropParams": {
      "species": ["include-from-file", "crops/maize.json"],
      "cultivar": ["include-from-file", "crops/maize/silage-maize.json"]
    },
    "residueParams": ["include-from-file", "crop-residues/maize.json"]
  }
}
```

#### `crop` object

The `crop` object defines the species, cultivar, and residue parameters of the crop to plant.

| Key name                  | Type               | Unit | Default                       | Required? | Description                                                                                               |
|---------------------------|--------------------|------|-------------------------------|-----------|-----------------------------------------------------------------------------------------------------------|
| **`cropParams`**          | JSON object        |      |                               | Yes       | Contains the MONICA species and cultivar parameters                                                       |
| **`residueParams`**       | JSON object        |      |                               | Yes       | Contains the MONICA crop residue parameters                                                               |
| **`perennialCropParams`** | JSON object        |      | `cropParams`                  | No        | Parameters intended for subsequent years of a perennial crop. Only considered when the crop is perennial. |
| **`is-winter-crop`**      | Boolean            |      | cultivar's `WinterCrop` value | No        | Explicitly defines whether the crop is a winter crop                                                      |
| **`is-perennial-crop`**   | Boolean            |      | cultivar's `Perennial` value  | No        | Explicitly defines whether the crop is perennial and overrides the cultivar's `Perennial` parameter       |
| **`cuttingDates`**        | Array of ISO dates |      | `[]`                          | No        | Stores cutting dates associated with the crop                                                             |

Example for a perennial crop:

```json
{
  "is-perennial-crop": true,
  "is-winter-crop": false,
  "cropParams": {
    "species": ["include-from-file", "crops/clover-grass-ley.json"],
    "cultivar": ["include-from-file", "crops/clover-grass-ley/.json"]
  },
  "residueParams": ["include-from-file", "crop-residues/clover-grass-ley.json"]
}
```

##### Winter crops and N-min fertilization

If `is-winter-crop` is supplied, its value overrides the cultivar's `WinterCrop` parameter. If it is omitted, MONICA uses `WinterCrop` from the cultivar parameters.

When the N-min fertilization method is enabled:

- Summer crops receive the N-Min application at sowing.
- Winter crops receive it on the configured `JulianDayAutomaticFertilising`.

MONICA does not currently infer winter crop status by comparing the sowing and harvest days of the year.

##### Perennial crops

If `is-perennial-crop` is supplied, its value overrides the cultivar's `Perennial` parameter. If it is omitted, MONICA uses the cultivar value.

For perennial crops, `perennialCropParams` is intended to provide the crop parameters used after the planting year. If it is omitted, the regular `cropParams` are reused.

!!! warning

    Current implementation limitation: Although `perennialCropParams` is recognized, the current parser copies `cropParams` instead of loading the supplied `perennialCropParams`. Until this is corrected, separate perennial parameters do not take effect.

##### Cutting dates

`cuttingDates` is parsed and stored by the crop configuration. It does not currently create or execute `Cutting` worksteps automatically. Add explicit `Cutting` worksteps to the cultivation method when cuts must be performed.

##### `cropParams` and `perennialCropParams` objects

| Key name       | Type        | Required | Description                         |
|----------------|-------------|----------|-------------------------------------|
| **`species`**  | JSON object | Yes      | MONICA species parameters           |
| **`cultivar`** | JSON object | Yes      | MONICA cultivar-specific parameters |

The objects may be provided directly or resolved through MONICA's `include-from-file` mechanism.

---

### **AutomaticSowing**

The `AutomaticSowing` workstep determines the sowing date dynamically from weather and soil conditions. MONICA begins evaluating the conditions on `earliest-date`. Sowing occurs on the first day when all conditions are satisfied. If no suitable day is found earlier, sowing is performed on `latest-date` regardless of the conditions.

When sowing occurs, MONICA fires both the `Sowing` and `AutomaticSowing` events.

#### Conditions

##### Air temperature

The interpretation of `min-temp` depends on the crop type:

- **Spring crops**: both of the following must be true:
    - The current day's minimum air temperature (`tmin`) is greater than or equal to `min-temp`.
    - The mean daily minimum air temperature (`tmin`) over the most recent `days-in-temp-window` days is greater than or equal to `min-temp`.
- **Winter crops**: the mean daily average air temperature (`tavg`) over the most recent `days-in-temp-window` days must be less than or equal to `min-temp`.

The current day is included in the averaging window. If fewer observations are available, the implementation uses the available observations. `days-in-temp-window` must be greater than zero.

##### Soil moisture

`min-%-asw` and `max-%-asw` define the inclusive range of available soil water within which sowing is permitted.

MONICA calculates the percentage of available soil water for the first soil layer:

$$\mathrm{ASW} = 100 \cdot \frac{\max\left(0,\theta-\mathrm{PWP}\right)} {\mathrm{FC}-\mathrm{PWP}}$$

where:

- $\theta$ is the current volumetric soil-moisture content
- $\mathrm{PWP}$ is the permanent wilting point
- $\mathrm{FC}$ is field capacity
- $\mathrm{ASW}$ is expressed as a percentage

The first soil layer is often 0–10 cm deep, but its actual depth depends on the configured soil-layer structure.

##### Precipitation

Both precipitation conditions must be satisfied:

- The precipitation sum over the current day and two preceding days must not exceed `max-3d-precip`.
- Precipitation on the current day must not exceed `max-curr-day-precip`.

##### Temperature sum

`temp-sum-above-base-temp` defines the required accumulated temperature sum above `base-temp`:

$$T_{\mathrm{sum}} = \sum_{d=d_{\mathrm{start}}}^{d_{\mathrm{current}}} \max\left(0,T_{\mathrm{avg},d}-T_{\mathrm{base}}\right)$$

where:

- $T_{\mathrm{avg},d}$ is the daily average air temperature on day $d$
- $T_{\mathrm{base}}$ is `base-temp`
- $T_{\mathrm{sum}}$ is expressed in degree-days $({}^{\circ}\mathrm{C}\,\mathrm{d})$, equivalently $\mathrm{K}\,\mathrm{d}$.

The current day is included. The implementation accumulates the temperature sum over all climate data available since the beginning of the simulation.

##### Optional soil temperature condition

The optional `avg-soil-temp` object adds a minimum average soil temperature requirement.

Soil temperature is spatially averaged from the surface down to `avg-soil-temp.depth`, and then temporally averaged over the most recent `avg-soil-temp.days` days. Sowing is permitted only when the resulting average is greater than or equal to `avg-soil-temp.Tavg`.

The condition is enabled only when `depth`, `days`, and `Tavg` are all greater than zero.

| Parameter name                 | Type        | Unit     | Default | Required? | Example                                                 | Description                                                                                                                 |
|--------------------------------|-------------|----------|---------|-----------|---------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| **`type`**                     | String      |          |         | Yes       | `"type": "AutomaticSowing"`                             | Workstep type                                                                                                               |
| **`date`**                     | String      | ISO date | unset   | No        | `"date": "0000-04-15"`                                  | Fixed execution date. When supplied, the workstep is dated and the automatic sowing conditions are not used for scheduling. |
| **`crop`**                     | JSON object |          |         | Yes       | `"crop": ["ref", "crops", "WW"]`                        | Crop to sow                                                                                                                 |
| **`earliest-date`**            | String      | ISO date |         | Yes       | `"earliest-date": "0000-03-01"`                         | First date on which the sowing conditions are evaluated                                                                     |
| **`latest-date`**              | String      | ISO date |         | Yes       | `"latest-date": "0000-04-15"`                           | Date on which sowing is forced if it has not occurred earlier                                                               |
| **`min-temp`**                 | Number      | °C       | `0`     | No        | `"min-temp": 5`                                         | Air temperature threshold                                                                                                   |
| **`days-in-temp-window`**      | Integer     | d        | `0`     | No        | `"days-in-temp-window": 7`                              | Number of recent days used to calculate the air temperature average. Must be greater than zero.                             |
| **`min-%-asw`**                | Number      | %        | `0`     | No        | `"min-%-asw": 20`                                       | Minimum permitted percentage of available soil water                                                                        |
| **`max-%-asw`**                | Number      | %        | `100`   | No        | `"max-%-asw": 80`                                       | Maximum permitted percentage of available soil water                                                                        |
| **`max-3d-precip`**            | Number      | mm       | `0`     | No        | `"max-3d-precip": 10`                                   | Maximum precipitation sum over the current day and two preceding days                                                       |
| **`max-curr-day-precip`**      | Number      | mm       | `0`     | No        | `"max-curr-day-precip": 2`                              | Maximum precipitation on the current day                                                                                    |
| **`temp-sum-above-base-temp`** | Number      | °C d     | `0`     | No        | `"temp-sum-above-base-temp": 100`                       | Required accumulated temperature sum above `base-temp`                                                                      |
| **`base-temp`**                | Number      | °C       | `0`     | No        | `"base-temp": 5`                                        | Base temperature used to calculate the temperature sum                                                                      |
| **`avg-soil-temp`**            | JSON object |          |         | No        | `"avg-soil-temp": {"depth": 0.3, "days": 5, "Tavg": 8}` | Optional average soil temperature condition                                                                                 |
| **`avg-soil-temp.depth`**      | Number      | m        | `0.30`  | No        | `"depth": 0.30`                                         | Soil depth over which the spatial average is calculated                                                                     |
| **`avg-soil-temp.days`**       | Integer     | d        | `0`     | No        | `"days": 5`                                             | Number of recent days used for the temporal average                                                                         |
| **`avg-soil-temp.Tavg`**       | Number      | °C       | `0`     | No        | `"Tavg": 8`                                             | Minimum required average soil temperature                                                                                   |

Example:

```json
{
  "type": "AutomaticSowing",
  "crop": ["ref", "crops", "WW"],
  "earliest-date": "0000-09-15",
  "latest-date": "0000-10-20",
  "min-temp": 12,
  "days-in-temp-window": 7,
  "min-%-asw": 20,
  "max-%-asw": 90,
  "max-3d-precip": 10,
  "max-curr-day-precip": 3,
  "temp-sum-above-base-temp": 0,
  "base-temp": 0,
  "avg-soil-temp": {
    "depth": 0.3,
    "days": 5,
    "Tavg": 8
  }
}
```

---

### **Harvest**

Harvests the currently growing crop and controls which biomass is exported from the field and which biomass is returned to the soil as added organic matter (AOM).

When a crop is present, the workstep fires the `Harvest` event.

| Parameter name                                                  | Type        | Unit     | Default  | Required? | Example                         | Description                                                                                                                                                                                                                                                                                                              |
|-----------------------------------------------------------------|-------------|----------|----------|-----------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`type`**                                                      | String      |          |          | Yes       | `"type": "Harvest"`             | Workstep type                                                                                                                                                                                                                                                                                                            |
| **`date`**                                                      | String      | ISO date | unset    | Conditional | `"date": "0001-08-15"`        | Date on which the crop is harvested. Required unless event-relative scheduling is used.                                                                                                                                                                                                                                 |
| **`exported`**                                                  | Boolean     |          | `true`   | No        | `"exported": false`             | Default behavior when no organ settings are provided. If `true`, primary yield, and secondary yield when `UseSecondaryYields` is enabled, is exported. Living root biomass and the remaining crop residues are incorporated into the soil. If `false`, living root biomass and all aboveground biomass are incorporated. |
| **`leaf`**, **`fruit`**, **`shoot`**, **`struct`**, **`sugar`** | JSON object |          |          | No        | `"leaf": {"export": [85, "%"]}` | Defines the percentage of an organ exported from the field. `export` defaults to `100%`. Unexported and unspecified biomass is incorporated. Organ settings override `exported`. The `incorporate` property is currently ignored, so `"incorporate": false` does not leave biomass on the soil surface.                  |

Example:

```json
{
  "type": "Harvest",
  "date": "0001-08-15",
  "leaf": {
    "export": [85, "%"]
  },
  "fruit": {
    "export": [100, "%"]
  }
}
```

---

### **AutomaticHarvest**

A dynamic harvesting workstep that evaluates its harvesting conditions every day.

Harvest takes place when:

- crop maturity has been reached
- available soil water in the first soil layer is within the configured range
- the three-day precipitation sum is below its configured maximum, and
- precipitation on the current day is below its configured maximum.

If these conditions remain unmet, harvest is forced on or after `latest-date`. `AutomaticHarvest` inherits all biomass-handling parameters from `Harvest`, including `exported` and the organ-specific settings.

When executed, the workstep fires both the `Harvest` and `AutomaticHarvest` events.

#### Conditions

- `min-%-asw` and `max-%-asw` define the permitted available soil water range in the first soil layer
- `max-3d-precip-sum` defines the maximum precipitation accumulated over the current day and the preceding two days
- `max-curr-day-precip` defines the maximum precipitation on the current day

| Parameter name            | Type   | Unit     | Default      | Required? | Example                       | Description                                                                                 |
|---------------------------|--------|----------|--------------|-----------|-------------------------------|---------------------------------------------------------------------------------------------|
| **`type`**                | String |          |              | Yes       | `"type": "AutomaticHarvest"`  | Workstep type                                                                               |
| **`date`**                | String | ISO date | unset        | No        | `"date": "0001-08-15"`        | Fixed execution date. When supplied, the workstep is dated and the automatic harvest conditions are not used for scheduling.      |
| **`latest-date`**         | String | ISO date | unset        | No        | `"latest-date": "0001-09-05"` | Date that forces harvest if the conditions remain unmet. Omitting it removes this fallback. |
| **`harvest-time`**        | String |          | `"maturity"` | No        | `"harvest-time": "maturity"`  | Conditional harvest trigger. Currently, only `"maturity"` is implemented.                   |
| **`min-%-asw`**           | Number | %        | `0`          | No        | `"min-%-asw": 10`             | Minimum available soil water in the first soil layer                                        |
| **`max-%-asw`**           | Number | %        | `999`        | No        | `"max-%-asw": 99`             | Maximum available soil water in the first soil layer                                        |
| **`max-3d-precip-sum`**   | Number | mm       | `9999`       | No        | `"max-3d-precip-sum": 2`      | Maximum precipitation over the current and preceding two days                               |
| **`max-curr-day-precip`** | Number | mm       | `9999`       | No        | `"max-curr-day-precip": 0.1`  | Maximum precipitation on the current day                                                    |

Example:

```json
{
  "type": "AutomaticHarvest",
  "latest-date": "0001-09-05",
  "harvest-time": "maturity",
  "min-%-asw": 10,
  "max-%-asw": 99,
  "max-3d-precip-sum": 2,
  "max-curr-day-precip": 0.1,
  "exported": true
}
```

---

### **Cutting**

Performs a cutting operation on the current crop.

The operation determines:

- which crop organs are cut
- how much of each organ is cut or left
- how much of the cut biomass is exported from the field
- how the crop's maximum assimilation rate is adjusted after cutting

Cut biomass that is not exported is returned to the field and added to the soil organic matter pools.

After the operation, MONICA fires the `Cutting` event.

#### Organ specification

The `organs` parameter maps organ names to cutting specifications:

```json
{
  "Leaf": [85, "%", "cut"],
  "Fruit": [50, "%", "cut"]
}
```

Supported organ names are: `"Root"`, `"Leaf"`, `"Shoot"`, `"Fruit"`, `"Struct"`, `"Sugar"`. Organ names are matched case-insensitively.

Each organ specification has the following form:

```
[<value>, "<unit>", "<mode>"]
```

The supported units are:

| Unit        | Meaning                                      |
|-------------|----------------------------------------------|
| `"%"`       | Percentage of the organ biomass              |
| `"kg ha-1"` | Organ dry matter biomass                     |
| `"m2 m-2"`  | Leaf area index. Supported only for `"Leaf"` |

The mode controls how the value is interpreted:

| Mode     | Meaning                                                                   |
|----------|---------------------------------------------------------------------------|
| `"cut"`  | The specified amount is removed from the organ. This is the default mode. |
| `"left"` | The specified amount remains on the crop. The remainder is cut.           |

For LAI specifications, use `"left"`:

```json
{
  "Leaf": [1.5, "m2 m-2", "left"]
}
```

This leaves the crop with an LAI of `1.5`. The current implementation interprets LAI as the amount left even if `"cut"` is supplied.

Always provide organ values as arrays with explicit units. Scalar specifications such as `"Leaf": 85` are not parsed correctly by the current implementation.

If `organs` is omitted or empty, MONICA uses the cultivar's `OrganIdsForCutting` entries, consisting of `organId` and `yieldPercentage`.

| Parameter name                  | Type                       | Unit                  | Default                       | Required? | Example                                                                                                         | Description                                                                       |
|---------------------------------|----------------------------|-----------------------|-------------------------------|-----------|-----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| **`type`**                      | String                     |                       |                               | Yes       | `"type": "Cutting"`                                                                                             | Workstep type                                                                     |
| **`date`**                      | String                     | ISO date              | unset                         | Conditional | `"date": "2026-06-15"`                                                                                        | Date on which the crop is cut. Required unless event-relative scheduling is used. |
| **`organs`**                    | JSON object                | %, kg ha-1, or m2 m-2 | Cultivar `OrganIdsForCutting` | No        | `"organs": {"Leaf": 85, "Fruit": [100, "%"]}` or `{"Leaf": [1.5, "m2 m-2", "left"], "Fruit": [50, "%", "cut"]}` | Maps organ names to cutting specifications.                                       |
| **`export`**                    | Boolean or JSON object     | % for object values   | `true`                        | No        | `"export": {"Leaf": 100, "Fruit": 0}`                                                                           | Controls how much cut biomass is removed from the field                           |
| **`cut-max-assimilation-rate`** | Number or unit-value array | %                     | `100`                         | No        | `"cut-max-assimilation-rate": [80, "%"]`                                                                        | Maximum assimilation rate retained after cutting                                  |

Example: date-based cutting

```json
{
  "type": "Cutting",
  "date": "2026-06-15",
  "organs": {
    "Leaf": [1.5, "m2 m-2", "left"],
    "Fruit": [50, "%", "cut"]
  },
  "export": {
    "Leaf": 100,
    "Fruit": 50
  },
  "cut-max-assimilation-rate": [80, "%"]
}
```

This operation:

- leaves an LAI of `1.5`
- cuts 50% of the fruit biomass
- exports all cut leaf biomass and 50% of the cut fruit biomass
- leaves non-exported biomass on the field
- reduces the current maximum assimilation rate to 80%

Example: event-relative cutting

```json
{
  "type": "Cutting",
  "after": "Sowing",
  "days": 30,
  "organs": {
    "Leaf": [85, "%", "cut"],
    "Fruit": [100, "%", "cut"]
  },
  "export": true
}
```

This performs the cutting operation 30 days after the `Sowing` event and exports all cut biomass.

---

### **MineralFertilization**

Applies mineral nitrogen fertilizer to the topsoil. The total nitrogen application rate is specified by `amount`, while `partition` defines the fractions supplied as carbamide, ammonium (`NH4`), and nitrate (`NO3`).

The partition fractions should sum to `1`. A missing component is treated as `0`.

After application, MONICA fires the `MineralFertilization` event.

| Parameter name  | Type        | Unit      | Default  | Required | Example                                                | Description                                                                        |
|-----------------|-------------|-----------|----------|----------|--------------------------------------------------------|------------------------------------------------------------------------------------|
| **`type`**      | String      |           |          | Yes      | `"type": "MineralFertilization"`                     | Workstep type                                                                      |
| **`date`**      | String      | ISO date  | unset    | Conditional | `"date": "2026-04-15"`                           | Date of application. Required unless event-relative scheduling is used.            |
| **`partition`** | JSON object |           |          | Yes      | `"partition": {"Carbamid": 0, "NH4": 0.5, "NO3": 0.5}` | Fractions of the total nitrogen amount supplied as carbamide, ammonium, and nitrate |
| **`amount`**    | Number      | kg N ha-1 |          | Yes      | `"amount": 30`                                         | Total amount of nitrogen applied per hectare                                       |

Example:

```json
{
  "type": "MineralFertilization",
  "date": "2026-04-15",
  "amount": 30,
  "partition": {
    "Carbamid": 0,
    "NH4": 0.5,
    "NO3": 0.5
  }
}
```

The `partition` object may also be loaded from an external JSON file:

```json
{
  "type": "MineralFertilization",
  "date": "2026-04-15",
  "amount": 30,
  "partition": ["include-from-file", "mineral-fertilisers/AN.json"]
}
```

The referenced file should contain the partition object:

```json
{
  "Carbamid": 0,
  "NH4": 0.5,
  "NO3": 0.5
}
```

---

### **NDemandFertilization**

Calculates and applies mineral nitrogen fertilizer to reach a specified target N supply. The fertilizer amount is calculated as `max(0, N-demand - available soil mineral N)`.

Available soil mineral N is the sum of nitrate (`NO3`) and ammonium (`NH4`) within the effective measurement depth. The effective depth is the smaller of the specified `depth` and the crop's current rooting depth.

The fertilizer composition is defined by `partition`. It can be specified directly as a JSON object or loaded from an external JSON file.

Fertilization occurs on `date`, when a date is specified, or when the crop reaches `stage`, if no date is specified.

If neither `date` nor `stage` is specified, `stage` defaults to `1`.

The workstep fires the `NDemandFertilization` event, including when the calculated fertilizer amount is zero.

| Parameter name  | Type        | Unit                     | Default | Required? | Example                                                                                        | Description                                                                                                                                  |
|-----------------|-------------|--------------------------|---------|-----------|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **`type`**      | String      |                          |         | Yes       | `"type": "NDemandFertilization"`                                                             | Workstep type                                                                                                                                |
| **`partition`** | JSON object |                          |         | Yes       | `"partition": {"id": "AN",	"name": "ammonium nitrate", "Carbamid": 0, "NH4": 0.5, "NO3": 0.5}`  | Mineral fertilizer composition. `Carbamid`, `NH4`, and `NO3` specify the fractions of the applied N assigned to urea, ammonium, and nitrate. |
| **`N-demand`**  | Number      | kg N ha-1                | `0`     | Yes       | `"N-demand": [70, "kg"]`                                                                       | Target mineral N supply within the evaluated soil profile. Fertilizer is applied only when the available mineral N is below this value.      |
| **`depth`**     | Number      | m                        | `0`     | Yes       | `"depth": [0.3, "m"]`                                                                          | Maximum depth down to which available soil mineral N is determined. The crop's current rooting depth may further limit the effective depth.  |
| **`date`**      | String      | ISO date                 | unset   | No        | `"date": "0001-03-15"`                                                                         | Date on which fertilization occurs. When omitted, stage-based scheduling is used.                                                            |
| **`stage`**     | Integer     | Development stage number | `1`     | No        | `"stage": 1`                                                                                   | One-based MONICA development stage at which fertilization occurs when `date` is omitted. The valid range depends on the crop.                |

Example using a date:

```json
{
  "type": "NDemandFertilization",
  "date": "0001-03-15",
  "N-demand": [70, "kg"],
  "depth": [0.3, "m"],
  "partition": {
    "id": "AN",
    "name": "ammonium nitrate",
    "Carbamid": 0,
    "NH4": 0.5,
    "NO3": 0.5
  }
}
```

Example using a development stage:

```json
{
  "type": "NDemandFertilization",
  "stage": 2,
  "N-demand": [70, "kg"],
  "depth": [0.3, "m"],
  "partition": ["include-from-file", "mineral-fertilisers/AN.json"]
}
```

---

### **OrganicFertilization**

Applies organic fertilizer at the specified fresh-matter application rate. The fertilizer's composition and decomposition properties are defined by the `parameters` object.

When `incorporation` is `true`, the application is treated as incorporated for processes such as ammonia volatilization. The target soil layer can be selected with `incorporateIntoLayerNo`.

When applied, the workstep fires the `OrganicFertilization` event.

| Parameter name               | Type                       | Unit              | Default | Required? | Example                           | Description                                                                             |
|------------------------------|----------------------------|-------------------|---------|-----------|-----------------------------------|-----------------------------------------------------------------------------------------|
| **`type`**                   | String                     |                   |         | Yes       | `"type": "OrganicFertilization"` | Workstep type                                                                           |
| **`date`**                   | String                     | ISO date          | unset   | Conditional | `"date": "1993-12-16"`       | Date of application. Required unless event-relative scheduling is used.                  |
| **`parameters`**             | JSON object                |                   |         | Yes       | See below                         | Defines the organic fertilizer's composition and decomposition properties               |
| **`amount`**                 | Number or value-unit array | kg FM ha-1        |         | Yes       | `"amount": [30000, "kg FM ha-1"]` | Fresh matter application rate per hectare                                               |
| **`incorporation`**          | Boolean                    |                   | `false` | No        | `"incorporation": true`           | Whether the fertilizer is treated as incorporated (`true`) or surface-applied (`false`) |
| **`incorporateIntoLayerNo`** | Integer                    | soil layer number | `1`     | No        | `"incorporateIntoLayerNo": 2`     | One-based number of the soil layer that receives the fertilizer                         |

Example:

```json
{
  "type": "OrganicFertilization",
  "date": "1993-12-16",
  "amount": [30000, "kg FM ha-1"],
  "incorporation": true,
  "incorporateIntoLayerNo": 1,
  "parameters": {
    "id": "CAM",
    "name": "cattle manure",
    "AOM_DryMatterContent": [0.196, "kg DM kg FM-1"],
    "AOM_FastDecCoeffStandard": [0.002, "d-1"],
    "AOM_NH4Content": [0.007, "kg N kg DM-1"],
    "AOM_NO3Content": [0, "kg N kg DM-1"],
    "AOM_SlowDecCoeffStandard": [0.0002, "d-1"],
    "CN_Ratio_AOM_Fast": 6.5,
    "CN_Ratio_AOM_Slow": 100,
    "NConcentration": 0,
    "PartAOM_Slow_to_SMB_Fast": [1, "kg kg-1"],
    "PartAOM_Slow_to_SMB_Slow": [0, "kg kg-1"],
    "PartAOM_to_AOM_Fast": [0.18, "kg kg-1"],
    "PartAOM_to_AOM_Slow": [0.72, "kg kg-1"]
  }
}
```

---

### **Tillage**

Performs a tillage operation from the soil surface down to the specified depth. The operation mixes soil properties and incorporated material within the affected soil layers.

After the operation is applied, MONICA fires the `Tillage` event.

| Parameter name | Type        | Unit | Default | Required? | Example                    | Description                                           |
|----------------|-------------|------|---------|-----------|----------------------------|-------------------------------------------------------|
| **`type`**     | String      |      |         | Yes       | `"type": "Tillage"`      | Workstep type                                         |
| **`date`**     | String      | ISO date | unset | Conditional | `"date": "1993-04-27"` | Date of operation. Required unless event-relative scheduling is used. |
| **`depth`**    | Number      | m    | `0.3`   | No        | `"depth": [0.15, "m"]` or `"depth": 0.15` | Tillage depth measured downward from the soil surface |

Example:

```json
{
  "type": "Tillage",
  "date": "1993-04-27",
  "depth": 0.15
}
```

If `depth` is omitted, MONICA uses the default depth of `0.3` m.

---

### **Irrigation**

Applies a specified amount of irrigation water. The irrigation water can optionally include nitrate and sulfate concentrations.

The workstep fires the `Irrigation` event when it is applied.

| Parameter name   | Type        | Unit | Default | Required? | Example                                                  | Description                         |
|------------------|-------------|------|---------|-----------|----------------------------------------------------------|-------------------------------------|
| **`type`**       | String      |      |         | Yes       | `"type": "Irrigation"`                                | Workstep type                       |
| **`date`**       | String      | ISO date | unset | Conditional | `"date": "2026-06-15"`                           | Date of application. Required unless event-relative scheduling is used. |
| **`amount`**     | Number      | mm   |         | Yes       | `"amount": 20`                                           | Amount of irrigation water to apply |
| **`parameters`** | JSON object |      |         | No        | `"parameters": {"nitrateConcentration": [10, "mg dm-3"]}` | Irrigation water composition       |

#### `parameters` object

| Parameter name             | Type   | Unit    | Default | Required? | Example                                  | Description                                                                                                    |
|----------------------------|--------|---------|---------|-----------|------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| **`nitrateConcentration`** | Number | mg dm-3 | `0`     | No        | `"nitrateConcentration": [10, "mg dm-3]` | Nitrate concentration in the irrigation water                                                                  |
| **`sulfateConcentration`** | Number | mg dm-3 | `0`     | No        | `"sulfateConcentration": [5, "mg dm-3"]` | Sulfate concentration in the irrigation water. Currently accepted and serialized but not applied by the model. |

Example:

```json
{
  "type": "Irrigation",
  "date": "2026-06-15",
  "amount": [20, "mm"],
  "parameters": {
    "nitrateConcentration": [10, "mg dm-3"],
    "sulfateConcentration": [5, "mg dm-3"]
  }
}
```

---

### **AutomaticIrrigation**

Automatically applies irrigation when the plant-available soil water falls to or below a configured threshold.

Irrigation can be restricted by:

- crop presence
- crop developmental stage
- a start and/or stop date
- the crop-specific irrigation temperature sum window

When irrigation water is applied successfully, the workstep fires the `AutomaticIrrigation` event.

| Parameter name     | Type        | Unit | Default | Required? | Example                       | Description                                                                                                                              |
|--------------------|-------------|------|---------|-----------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| **`type`**         | String      |      |         | Yes       | `"type": "AutomaticIrrigation"` | Workstep type                                                                                                                          |
| **`date`**         | String      | ISO date | unset | No        | `"date": "0000-06-15"` | Fixed execution date. When supplied, the workstep is dated and its automatic conditions are not used for scheduling.                                         |
| **`irrigateCrop`** | Boolean     |      | `false` | No        | `"irrigateCrop": true`        | If `true`, irrigation is evaluated only while a crop is growing. Supplying `startStage` or `endStage` automatically enables this option. |
| **`startStage`**   | Integer     |      | unset   | No        | `"startStage": 4`             | First developmental stage during which automatic irrigation is active                                                                    |
| **`endStage`**     | Integer     |      | unset   | No        | `"endStage": 4`               | Last developmental stage during which automatic irrigation is active. The stage is included in the active range.                         |
| **`parameters`**   | JSON object |      |         | Yes       | (See table below)             | Configures the soil moisture threshold, irrigation amount, date range, evaluation depth, and irrigation water composition                |

#### `parameters` object

| Parameter name                 | Type        | Unit     | Default | Required? | Example                                | Description                                                                                                                        |
|--------------------------------|-------------|----------|---------|-----------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| **`startDate`**                  | String      | ISO date |         | No          | `"startDate": "0000-09-23"`            | First date on which automatic irrigation may be applied. Absolute and relative dates are supported.                                |
| **`stopDate`**                   | String      | ISO date |         | No          | `"stopDate": "0001-08-25"`             | Last date on which automatic irrigation may be applied. The specified date is included. Absolute and relative dates are supported. |
| **`amount`**                     | Number      | mm       | unset   | Alternative | `"amount": [17, "mm"]`                 | Fixed amount of irrigation water to apply when the threshold is reached                                                            |
| **`set_to_%nFC`**                | Number      | % nFC    | unset   | Alternative | `"set_to_%nFC": [100, "%"]`            | Calculates and applies the water needed to restore plant-available soil water to the specified percentage of nFC                   |
| **`trigger_if_nFC_below_%`**   | Number      | % nFC    | unset   | Yes       | `"trigger_if_nFC_below_%": [90, "%"]`  | Triggers irrigation when plant-available soil water is at or below the specified percentage of usable field capacity (nFC)         |
| **`calc_nFC_until_depth_m`**     | Number      | m        | `0.3`   | No          | `"calc_nFC_until_depth_m": [0.3, "m"]` | Soil depth over which plant-available water is evaluated and, for `set_to_%nFC`, restored                                          |
| **`irrigationParameters`**       | JSON object |          |         | No          | See below                              | Configures the composition of the irrigation water                                                                                 |

#### `irrigationParameters` object

| Parameter name               | Type   | Unit    | Default | Required? | Example                                  | Description                                   |
|------------------------------|--------|---------|---------|-----------|------------------------------------------|-----------------------------------------------|
| **`nitrateConcentration`** | Number | mg dm-3 | `0`     | No        | `"nitrateConcentration": [10, "mg dm-3"]` | Nitrate concentration in the irrigation water |

Example:

```json
{
  "type": "AutomaticIrrigation",
  "irrigateCrop": true,
  "startStage": 4,
  "endStage": 6,
  "parameters": {
    "startDate": "0000-09-23",
    "stopDate": "0001-08-25",
    "amount": [17, "mm"],
    "trigger_if_nFC_below_%": [90, "%"],
    "calc_nFC_until_depth_m": [0.3, "m"],
    "irrigationParameters": {
      "nitrateConcentration": [0, "mg dm-3"]
    }
  }
}
```

To restore soil moisture to a target instead of applying a fixed quantity, replace `amount` with:

```json
"set_to_%nFC": [100, "%"]
```

!!! note

    Set `UseAutomaticIrrigation` to `false` or omit it in `sim.json` when irrigation should be controlled exclusively by the `AutomaticIrrigation` workstep.

    If `UseAutomaticIrrigation` is `true`, the global automatic irrigation configuration from `sim.json` is also evaluated and may apply irrigation independently of the workstep.

---

### **SetValue**

Sets a model output variable to a new value on the specified date. Only output variables that are explicitly registered as **settable** can be modified.

The new value may be:

- a number, such as `5.0`
- another output expression, such as `["NH4", 1]`
- an arithmetic expression in the form: `["=", leftOperand, operator, rightOperand]`. Each operand may be a number or an output expression. Supported operators are `+`, `-`, `*`, and `/`.

When a scalar value is assigned to a layer range, the value is applied to every selected layer. Arithmetic between a layer range and a scalar is performed element by element.

!!! note

    Layer numbers are **one-based**, and layer ranges are **inclusive**. For example, `["NO3", [1, 10]]` selects layers 1 through 10.

!!! note

    Literal arrays of values, such as `[20, 30, 20]`, are not currently supported as the value of a `SetValue` workstep.

A successfully evaluated workstep fires the `SetValue` event.

| Parameter name    | Type                                                | Unit     | Default | Required? | Example                                | Description                                                       |
|-------------------|-----------------------------------------------------|----------|---------|-----------|----------------------------------------|-------------------------------------------------------------------|
| **`type`**        | String                                              |          |         | Yes       | `"type": "SetValue"`                 | Workstep type                                                     |
| **`date`**        | String                                              | ISO date | unset   | Conditional | `"date": "0000-09-22"`             | Date on which the value is set. Required unless event-relative scheduling is used. |
| **`var`**         | Output expression                                   |          |         | Yes       | `"var": ["NO2", 1]`                    | Settable output variable, array element, or layer range to modify |
| **`value`**       | Number, output expression, or arithmetic expression |          |         | Yes       | `"value": ["=", ["NO2", 1], "*", 0.5]` | Value to assign or expression used to calculate it                |

Examples:

Set the NO<sub>2</sub> concentration in the first soil layer to half the concentration in the second soil layer every September 22:

```json
{ "date": "0000-09-22", "type": "SetValue", "var": ["NO2", 1], "value": ["=", ["NO2", 2], "*", 0.5] }
```

Double the NO<sub>3</sub> concentration in soil layers 1 through 10 every September 23:

```json
{ "date": "0000-09-23", "type": "SetValue", "var": ["NO3", [1, 10]], "value": ["=", ["NO3", [1, 10]], "*", 2] }
```

Assign the current NH<sub>4</sub> concentration in the first soil layer back to the same layer every September 24. This does not change the value:

```json
{ "date": "0000-09-24", "type": "SetValue", "var": ["NH4", 1], "value": ["NH4", 1] }
```

Set the carbamide (`Carb`) concentration in soil layers 1 through 20 to `0.01 kg N m-3` every September 25:

```json
{ "date": "0000-09-25", "type": "SetValue", "var": ["Carb", [1, 20]], "value": 0.01 }
```

---

## Example `crop.json` file

The following example defines a multi-year crop rotation and its management operations.

It demonstrates how to:

- Define crops and their residue parameters under `"crops"`
- Reuse crop definitions with `["ref", "crops", "<crop code>"]`
- Schedule sowing, fertilization, irrigation, tillage, and harvest
- Load parameter sets with `include-from-file`
- Supply general crop model parameters through `"CropParameters"`

!!! note

    This is the crop management portion of a MONICA configuration. A complete simulation also requires compatible `sim.json`, `site.json`, climate data, and a correctly configured `include-file-base-path`.

    Relative `include-from-file` paths are resolved against `include-file-base-path` from `sim.json`, not necessarily against the directory containing `crop.json`. The paths below assume that `include-file-base-path` points to the MONICA source directory and that `monica-parameters` is located beside it.

```json
{
  "crops": {
    "WR": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/rye.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/rye/winter-rye.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/rye.json"]
    },
    "SM": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/maize.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/maize/silage-maize.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/maize.json"]
    },
    "MEP": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/potato.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/potato/moderately-early-potato.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/potato.json"]
    },
    "WW": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/wheat.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/wheat/winter-wheat.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/wheat.json"]
    },
    "WG": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/barley.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/barley/winter-barley.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/barley.json"]
    },
    "SG": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/barley.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/barley/spring-barley.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/barley.json"]
    },
    "WRa": {
      "cropParams": {
        "species": ["include-from-file", "../monica-parameters/crops/rape.json"],
        "cultivar": ["include-from-file", "../monica-parameters/crops/rape/winter-rape.json"]
      },
      "residueParams": ["include-from-file", "../monica-parameters/crop-residues/rape.json"]
    }
  },

  "cropRotation": [
    {
      "worksteps": [
        {
          "date": "1991-09-23",
          "type": "Sowing",
          "crop": ["ref", "crops", "WR"]
        },
        {
          "date": "1992-04-03",
          "type": "MineralFertilization",
          "amount": [40, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1992-05-05",
          "type": "Irrigation",
          "amount": [1, "mm"],
          "parameters": {"nitrateConcentration": [0, "mg dm-3"]}
        },
        {
          "date": "1992-05-07",
          "type": "MineralFertilization",
          "amount": [40, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1992-07-27",
          "type": "Harvest"
        }
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
        {
          "date": "1993-04-27",
          "type": "Tillage",
          "depth": [0.15, "m"]
        },
        {
          "date": "1993-05-04",
          "type": "Sowing",
          "crop": ["ref", "crops", "SM"]
        },
        {
          "date": "1993-05-10",
          "type": "MineralFertilization",
          "amount": [60, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1993-09-23",
          "type": "Harvest"
        },
        {
          "date": "1993-12-16",
          "type": "OrganicFertilization",
          "amount": [30000, "kg"],
          "parameters": ["include-from-file", "../monica-parameters/organic-fertilisers/CADLM.json"],
          "incorporation": true
        },
        {
          "date": "1993-12-22",
          "type": "Tillage",
          "depth": [0.10, "m"]
        }
      ]
    },
    {
      "worksteps": [
        {
          "date": "1994-04-25",
          "type": "Sowing",
          "crop": ["ref", "crops", "MEP"]
        },
        {
          "date": "1994-05-04",
          "type": "MineralFertilization",
          "amount": [90, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1994-09-06",
          "type": "Harvest"
        },
        {
          "date": "1994-09-29",
          "type": "Tillage",
          "depth": [0.15, "m"]
        }
      ]
    },
    {
      "worksteps": [
        {
          "date": "1994-10-11",
          "type": "Sowing",
          "crop": ["ref", "crops", "WW"]
        },
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
        {
          "date": "1995-08-02",
          "type": "Harvest"
        },
        {
          "date": "1995-08-03",
          "type": "Tillage",
          "depth": [0.15, "m"]
        }
      ]
    },
    {
      "worksteps": [
        {
          "date": "1995-09-07",
          "type": "Sowing",
          "crop": ["ref", "crops", "WG"]
        },
        {
          "date": "1996-03-21",
          "type": "MineralFertilization",
          "amount": [60, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1996-04-13",
          "type": "Harvest"
        },
        {
          "date": "1996-04-14",
          "type": "Tillage",
          "depth": [0.10, "m"]
        }
      ]
    },
    {
      "worksteps": [
        {
          "date": "1996-04-17",
          "type": "Sowing",
          "crop": ["ref", "crops", "SG"]
        },
        {
          "date": "1996-09-10",
          "type": "Harvest"
        },
        {
          "date": "1996-09-17",
          "type": "Tillage",
          "depth": [0.10, "m"]
        }
      ]
    },
    {
      "worksteps": [
        {
          "date": "1997-04-04",
          "type": "Sowing",
          "crop": ["ref", "crops", "WRa"]
        },
        {
          "date": "1997-04-10",
          "type": "MineralFertilization",
          "amount": [80, "kg N"],
          "partition": ["include-from-file", "../monica-parameters/mineral-fertilisers/AN.json"]
        },
        {
          "date": "1997-07-08",
          "type": "Harvest"
        },
        {
          "date": "1997-07-09",
          "type": "Tillage",
          "depth": [0.10, "m"]
        }
      ]
    }
  ],

  "CropParameters": {
    "DEFAULT": ["include-from-file", "../monica-parameters/general/crop.json"]
  }
}
```

### Understanding the structure

Each entry under `"crops"` defines a reusable crop object containing:

- `"cropParams.species"`: species-level growth parameters
- `"cropParams.cultivar"`: cultivar-specific parameters
- `"residueParams"`: parameters used when crop residues enter the soil

A sowing operation references one of these definitions:

```json
{
  "date": "1994-10-11",
  "type": "Sowing",
  "crop": ["ref", "crops", "WW"]
}
```

The reference is replaced with the complete `WW` crop definition while MONICA builds the simulation environment.

A harvest does not need another crop reference:

```json
{
  "date": "1995-08-02",
  "type": "Harvest"
}
```

MONICA associates the harvest with the crop established by the corresponding sowing workstep.

### Units

Values may be written as a number and a descriptive unit:

```json
"depth": [0.15, "m"]
```

```json
"amount": [40, "kg N"]
```

```json
"amount": [1, "mm"]
```

For organic fertilization, `"amount"` describes the amount of organic material applied. Its nitrogen composition is supplied by the referenced fertilizer parameter set:

```json
{
  "type": "OrganicFertilization",
  "amount": [30000, "kg"],
  "parameters": ["include-from-file", "../monica-parameters/organic-fertilisers/CADLM.json"]
}
```

The management dates are illustrative and should be adjusted to the crop, cultivar, climate, and experimental scenario. In particular, the example's winter barley harvest and spring sowing of winter rape may not represent typical regional practice.