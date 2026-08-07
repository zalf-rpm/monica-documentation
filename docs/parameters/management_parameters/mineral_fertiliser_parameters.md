# Mineral Fertiliser Parameters

Mineral fertiliser parameter files define how an applied amount of mineral nitrogen is distributed among carbamide-N, ammonium-N, and nitrate-N. Each JSON file represents one fertiliser parameter set and is located in the `mineral-fertilisers/` directory of the [`monica-parameters`](https://github.com/zalf-rpm/monica-parameters) repository. The total nitrogen application amount is not defined in these files. It is specified separately in the crop management configuration, normally in `kg N`.

---

## List of mineral fertiliser parameters

| Parameter Name | Type   | Unit                        | Description                                                                                   | Example                                 |
|----------------|--------|-----------------------------|-----------------------------------------------------------------------------------------------|-----------------------------------------|
| `id`           | String |                             | Identifier of the parameter set. It normally matches the JSON filename without its extension. | `"id": "AN"`                            |
| `name`         | String |                             | Full descriptive name of the fertiliser                                                       | `"name": "Ammonium Nitrate"`            |
| `Carbamid`     | Number | kg carbamide-N kg-1 total N | Fraction of the applied nitrogen added as carbamide-N (urea-N)                                | `"Carbamid": 0`                         |
| `NH4`          | Number | kg NH4-N kg-1 total N       | Fraction of the applied nitrogen added as ammonium-N                                          | `"NH4": 0.5`                            |
| `NO3`          | Number | kg NO3-N kg-1 total N       | Fraction of the applied nitrogen added as nitrate-N                                           | `"NO3": 0.5`                            |
| `type`         | String |                             | Declares that this JSON defines mineral fertiliser parameters                                 | `"type": "MineralFertiliserParameters"` |

!!! note
    `Carbamid`, `NH4`, and `NO3` describe the distribution of the applied nitrogen among the three nitrogen forms. They do not describe the nitrogen concentration of the fertiliser product.

!!! warning
    The three fractions should normally sum to approximately `1.0`. MONICA does not validate or normalize their sum when loading the parameter file.

    Most supplied files sum to `1.0`, apart from insignificant floating-point differences. However, the current `ALZON` values sum to approximately `0.99666`, and the current `NS` values sum to approximately `1.03833`.


---

## Available mineral fertilisers

The following table lists all available mineral fertilisers in MONICA with their IDs, full names, and nitrogen form breakdown.

| ID    | Full Name                                                  | Carbamid-N | NH4-N | NO3-N |
|-------|------------------------------------------------------------|------------|-------|-------|
| AHLS  | Ammoniumnitrat-Harnstoff-Lösung + Ammoniumthiosulfat, 27-3 | 0.48       | 0.30  | 0.22  |
| ALZON | Alzon flüssig-S                                            | 0.46       | 0.33  | 0.21  |
| AN    | Ammonium Nitrate                                           | 0.00       | 0.50  | 0.50  |
| AP    | Ammonium Phosphate                                         | 0.00       | 1.00  | 0.00  |
| AS    | Ammonium Sulphate                                          | 0.00       | 1.00  | 0.00  |
| ASH   | Ammonsulfat-Harnstoff, 'Piamon'                            | 0.77       | 0.23  | 0.00  |
| CF4   | Compound fertiliser (43% NO3, 57% NH4)                     | 0.00       | 0.565 | 0.435 |
| CP1   | Compound fertiliser (0% NO3, 100% NH4)                     | 0.00       | 1.00  | 0.00  |
| CP2   | Compound fertiliser (35% NO3, 65% NH4)                     | 0.00       | 0.65  | 0.35  |
| CP3   | Compound fertiliser (50% NO3, 50% NH4)                     | 0.00       | 0.50  | 0.50  |
| NPK   | NPK-Dünger                                                 | 0.00       | 0.66  | 0.34  |
| NS    | Stickstoffdüngerlösung mit Schwefel, Piasan 24 S           | 0.50       | 0.33  | 0.21  |
| PN    | Potassium Nitrate                                          | 0.00       | 0.00  | 1.00  |
| U     | Urea                                                       | 1.00       | 0.00  | 0.00  |
| UAN   | Urea Ammonium Nitrate                                      | 0.50       | 0.25  | 0.25  |
| UAS   | Urea Ammonium Sulphate                                     | 0.82       | 0.18  | 0.00  |
| UNI   | Urea (nitrification inhibitor)                             | 1.00       | 0.00  | 0.00  |

---

## Example: `AN.json`

```json
{
    "Carbamid": 0,
    "NH4": 0.5,
    "NO3": 0.5,
    "id": "AN",
    "name": "Ammonium Nitrate",
    "type": "MineralFertiliserParameters"
}
```