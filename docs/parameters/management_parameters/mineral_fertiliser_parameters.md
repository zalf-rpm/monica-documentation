# Mineral Fertiliser Parameters

This section provides an overview of the key-value pairs in the mineral fertiliser JSON files used by MONICA. Each file represents one fertiliser type and defines how its total nitrogen content is split between three nitrogen forms. These files are located in the `mineral-fertilisers/` folder of the `monica-parameters` repository.

---

## List of mineral fertiliser parameters

| Parameter Name | Type   | Unit        | Description                                                                                                                  | Example                              |
|----------------|--------|-------------|------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| `id`           | String |             | Short identifier code for the fertiliser, used to reference it in management configurations                                  | `"id": "AN"`                         |
| `name`         | String |             | Full descriptive name of the fertiliser                                                                                      | `"name": "Ammonium Nitrate"`         |
| `Carbamid`     | Float  | kg kg-1 N   | Fraction of total nitrogen present as urea (carbamide). Values range from 0 to 1.                                           | `"Carbamid": 0`                      |
| `NH4`          | Float  | kg kg-1 N   | Fraction of total nitrogen present as ammonium (NH4). Values range from 0 to 1.                                             | `"NH4": 0.5`                         |
| `NO3`          | Float  | kg kg-1 N   | Fraction of total nitrogen present as nitrate (NO3). Values range from 0 to 1.                                              | `"NO3": 0.5`                         |
| `type`         | String |             | Declares that this JSON defines mineral fertiliser parameters                                                                | `"type": "MineralFertiliserParameters"` |

!!! note
    The three nitrogen fractions `Carbamid`, `NH4`, and `NO3` must sum to 1.0. These fractions describe the nitrogen form distribution, not the total nitrogen content of the product. The actual nitrogen application amount is defined separately in the crop management configuration.

---

## Fertiliser abbreviations

The following table lists all available mineral fertilisers in MONICA with their IDs, full names, and nitrogen form breakdown.

| ID    | Full Name                                                          | Carbamid | NH4   | NO3   |
|-------|--------------------------------------------------------------------|----------|-------|-------|
| AHLS  | Ammoniumnitrat-Harnstoff-Lösung + Ammoniumthiosulfat, 27-3        | 0.48     | 0.30  | 0.22  |
| ALZON | Alzon flüssig-S                                                    | 0.46     | 0.33  | 0.21  |
| AN    | Ammonium Nitrate                                                   | 0.00     | 0.50  | 0.50  |
| AP    | Ammonium Phosphate                                                 | 0.00     | 1.00  | 0.00  |
| AS    | Ammonium Sulphate                                                  | 0.00     | 1.00  | 0.00  |
| ASH   | Ammonsulfat-Harnstoff, 'Piamon'                                    | 0.77     | 0.23  | 0.00  |
| CF4   | Compound fertiliser (43% NO3, 57% NH4)                            | 0.00     | 0.565 | 0.435 |
| CP1   | Compound fertiliser (0% NO3, 100% NH4)                            | 0.00     | 1.00  | 0.00  |
| CP2   | Compound fertiliser (35% NO3, 65% NH4)                            | 0.00     | 0.65  | 0.35  |
| CP3   | Compound fertiliser (50% NO3, 50% NH4)                            | 0.00     | 0.50  | 0.50  |
| NPK   | NPK-Dünger                                                         | 0.00     | 0.66  | 0.34  |
| NS    | Stickstoffdüngerlösung mit Schwefel, Piasan 24 S                  | 0.50     | 0.33  | 0.21  |
| PN    | Potassium Nitrate                                                  | 0.00     | 0.00  | 1.00  |
| U     | Urea                                                               | 1.00     | 0.00  | 0.00  |
| UAN   | Urea Ammonium Nitrate                                              | 0.50     | 0.25  | 0.25  |
| UAS   | Urea Ammonium Sulphate                                             | 0.82     | 0.18  | 0.00  |
| UNI   | Urea (nitrification inhibitor)                                     | 1.00     | 0.00  | 0.00  |

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