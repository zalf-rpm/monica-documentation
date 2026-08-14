# MONICA Configuration Files

## Overview

A typical file-based MONICA run uses four kinds of input files:

- a simulation configuration, conventionally named `sim.json`
- a site configuration, conventionally named `site.json`
- a crop configuration, conventionally named `crop.json`
- one or more climate CSV files

The filenames are conventions, not fixed requirements. The simulation configuration identifies the site, crop, and climate files to use:

```json
{
  "crop.json": "crop.json",
  "site.json": "site.json",
  "climate.csv": "climate.csv"
}
```

The three JSON configurations collectively contain the information required for a model run:

- `sim.json` defines simulation settings, output configuration, and climate input
- `site.json` defines the site, soil profile, and environmental parameters
- `crop.json` defines crop rotations, management worksteps, and crop parameters

Parameters can be written directly in these files or loaded from other JSON files using `include-from-file`. Configuration objects can also be reused within the same file using `ref`.

!!! note

    The current C++ configuration loaded does not support `include-from-db`. Applications may obtain parameters from a database themselves, but they must provide the resulting JSON objects to MONICA.

MONICA configuration objects reflect the data structures used internally by the model. Many optional model parameters have built-in defaults, which are defined in the corresponding C++ data structures in the [MONICA source code](https://github.com/zalf-rpm/monica).

Required input data cannot always be replaced by defaults. For example, a run normally requires a valid soil profile, crop rotation, and climate data. Missing or invalid required input can prevent creation or execution of the model environment.

---

## Parameter values and units

A parameter can be written as a plain JSON value:

```json
"threshold": 0.35
```

For numeric parameters, MONICA also commonly uses an array whose first element is the value and whose second element describes its unit:

```json
"amount": [17, "mm"]
```

A third element may be used to describe the parameter:

```json
"amount": [17, "mm", "irrigation amount"]
```

For example, automatic irrigation can be configured as follows:

```json
"UseAutomaticIrrigation": false,
"AutoIrrigationParams": {
  "irrigationParameters": {
    "nitrateConcentration": [0, "mg dm-3"],
    "sulfateConcentration": [0, "mg dm-3"]
  },
  "amount": [17, "mm"],
  "threshold": 0.35
}
```

!!! warning

    For most scalar numeric parameters, MONICA reads only the first array element. Unit and description elements are documentation for users and are not used to convert or validate the value.

    Always provide values in the units expected by the corresponding parameter.

---

## Loading default values into an object

Many MONICA parameter objects support a `DEFAULT` member. It supplies an initial object whose values can then be overriden by members of the containing object.

In the following example, `EnvironmentParameters` is initialized from a JSON file. `LeachingDepth` and `WindSpeedHeight` then replace the corresponding values loaded from that file:

```json
"EnvironmentParameters": {
  "DEFAULT": ["include-from-file", "user-parameters/hermes-environment.json"],
  "LeachingDepth": 2.0,
  "WindSpeedHeight": 2.5
}
```

The value of `DEFAULT` must resolve to a JSON object. It may be written inline or produced by a supported configuration function such as `include-from-file`.

This mechanism is useful when most parameters remain unchanged between projects and only a small subset needs to be overridden.

---

## Functions in JSON

MONICA supports a small set of functions inside JSON configurations. A function call is represented by a JSON array:

```json
["function-name", argument1, argument2]
```

If the first array element matches a supported function name, MONICA evaluates the function and replaces the array with its result. Function arguments may themselves contain function calls, allowing expressions to be nested.

For example:

```json
"Lambda": [
  "sand-and-clay->lambda",
  ["KA5-texture-class->sand", "Sl2"],
  ["KA5-texture-class->clay", "Sl2"]
]
```

If a recognized function cannot be evaluated, for example, because an argument has the wrong type or an included file cannot be read, configuration processing reports an error and the model environment is not created.

If an array begins with an unrecognized string, it remains an ordinary JSON array. This means that a misspelled function may not be detected immediately and may instead cause an error when the containing configuration object is interpreted.

---

## Include data from a file

The `include-from-file` function loads a complete JSON value from another file:

```json
["include-from-file", "path/to/file.json"]
```

For example:

```json
"cropParams": {
  "species": ["include-from-file", "crops/rye.json"],
  "cultivar": ["include-from-file", "crops/rye/winter-rye.json"]
},
"residueParams": ["include-from-file", "crop-residues/rye.json"]
```

The included file must contain valid JSON. Its contents may contain additional supported functions, which are evaluated recursively.

### Include base path

Relative include paths are resolved using `include-file-base-path`. Supplied example configurations commonly define this in `sim.json`:

```json
"include-file-base-path": "${MONICA_PARAMETERS}/"
```

`${MONICA_PARAMETERS}` is replaced with the value of the `MONICA_PARAMETERS` environment variable. A typical installation sets this variable to the installed `monica-parameters` directory.

For example, if:

```
MONICA_PARAMETERS=C:\Users\USER_NAME\MONICA\monica-parameters
```

then:

```json
["include-from-file", "crops/rye.json"]
```

resolves to:

```
C:\Users\USER_NAME\MONICA\monica-parameters\crops\rye.json
```

!!! important

    `include-file-base-path` should be set explicitly in `sim.json`. `${MONICA_PARAMETERS}/` is a convention used by supplied configurations, not a built-in loader default.

If `crop.json` or `site.json` does not define its own `include-file-base-pth`, it inherits the value from `sim.json`. Either file can override the inherited value by defining a local base path:

```json
"include-file-base-path": "../project-parameters/"
```

Absolute include paths are not prefixed with `include-file-base-path`. MONICA still expands environment variables and normalized platform-specific path separators.

Nested includes use the configuration's effective `include-file-base-path`. They are not automatically resolved relative to the directory of the file containing the nested include.

---

## Referencing data within a file

The `ref` function reuses a value stored elsewhere in the same top-level configuration object:

```json
["ref", "top-level-key", "member-key"]
```

For example, `crop.json` can define reusable crop objects:

```json
{
  "crops": {
    "WR": {
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
          "date": "1991-09-23",
          "type": "Sowing",
          "crop": ["ref", "crops", "WR"]
        }
      ]
    }
  ]
}
```

In this example:

- `"crops"` identifies a top-level object
- `"WR"` identifies a member of that object
- The value of `crops.WR` replaces the `ref` expression.

References are resolved within the JSON document in which they occur.

---

## Conversion functions

The current C++ configuration loader supports the following canonical function names:

| Function Name                        | Argument 1                                    | Argument 2                         | Description                                         |                                                               
|--------------------------------------|-----------------------------------------------|------------------------------------|-----------------------------------------------------|
| `"humus-class->corg"`                | Humus class from `0` to `7`                   |                                    | Converts the humus class to an organic carbon value |                                   
| `"bulk-density-class->raw-density"`  | Effective bulk density class from `1` to `5`  | Clay fraction from `0.0` to `1.0`  | Calculates raw soil density                         |
| `"KA5-texture-class->clay"`          | KA5 texture class, for example `"fSms"`       |                                    | Returns the representative clay fraction            |             
| `"KA5-texture-class->sand"`          | KA5 texture class, for example `"Lt3"`        |                                    | Returns the representative sand fraction            |            
| `"sand-and-clay->lambda"`            | Sand fraction from `0.0` to `1.0`             | Clay fraction from `0.0` to `1.0`  | Calculates the soil lambda value                    | 
| `%`                                  | Percentage value                              |                                    | Divides the value by 100                            |

Examples:

```json
"SoilRawDensity": ["bulk-density-class->raw-density", 2, ["KA5-texture-class->clay", "Sl2"]],
"Lambda": ["sand-and-clay->lambda", ["KA5-texture-class->sand", "Sl2"], ["KA5-texture-class->clay", "Sl2"]],
"SomeFraction": ["%", 35]
```

The final expression produces `0.35`.

Some older configurations use legacy aliases for these functions. New configurations should use the canonical names listed above.

---

## JSON syntax

MONICA configuration files must contain valid JSON. In particular:

- Object member names and string values require double quotes.
- Objects and arrays must not have trailing commas.
- JSON does not support comments.
- Backslashes in JSON strings must be escaped, for example `"C:\\MONICA\\parameters"`.

Use an editor with JSON validation or a local JSON parser to check configuration syntax. Avoid uploading configurations containing private paths or project data to third-party online formatters.

### Comment conventions

MONICA configurations commonly use otherwise unused keys beginning with two underscores as human-readable comments:

```json
"__description": "Parameters for the Hohenfinow example"
```

An empty value can also be used:

```json
"__soil profile": ""
```

These are ordinary JSON members, not actual JSON comments. They work because configuration readers generally ignore unrecognized members.

A configuration member can sometimes be disabled by renaming it to an unrecognized name:

```json
"_UseAutomaticIrrigation": true
```

!!! warning

    Renaming a required member can make the configuration incomplete, while misspelled member names may be silently ignored. Use this technique carefully and restore the exact documented name when reenabling a setting.