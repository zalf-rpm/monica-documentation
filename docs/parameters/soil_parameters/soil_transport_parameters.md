# Soil Transport Parameters

This section provides an overview of the key-value pairs in the `UserSoilTransportParameters` JSON file used by MONICA. These parameters control how solutes (primarily nitrate) are transported through the soil profile via diffusion and dispersion.

---

## List of soil transport parameters

| Parameter Name                  | Type    | Unit    | Description                                                                                                   | Example                                        |
|---------------------------------|---------|---------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------|
| `AD`                            | Float   | m2 d-1  | Diffusion coefficient for solute transport in soil water                                                      | `"AD": 0.002`                                  |
| `DiffusionCoefficientStandard`  | Float   | m2 d-1  | Standard diffusion coefficient used as a reference for solute movement under standard conditions              | `"DiffusionCoefficientStandard": 0.000214`     |
| `DispersionLength`              | Float   | m       | Longitudinal dispersivity of the soil, describing the spreading of solutes due to variations in pore velocity | `"DispersionLength": 0.049`                    |
| `NDeposition`                   | Float   | kg N ha-1 yr-1 | Annual atmospheric nitrogen deposition added to the soil surface                                      | `"NDeposition": 0`                             |
| `type`                          | String  |         | Declares that this JSON defines soil transport parameters                                                     | `"type": "UserSoilTransportParameters"`        |

!!! note
    These parameters are set globally for the entire soil profile. `NDeposition` can be set to `0` if no atmospheric nitrogen input is considered, or adjusted to reflect site-specific deposition measurements.

---

## Example: `soiltransport.json`

```json
{
    "AD": 0.002,
    "DiffusionCoefficientStandard": 0.000214,
    "DispersionLength": 0.049,
    "NDeposition": 0,
    "type": "UserSoilTransportParameters"
}
```