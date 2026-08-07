# Soil Transport Parameters

This section describes the parameters in MONICA's `SoilTransportParameters` JSON object. They control nitrate diffusion and hydrodynamic dispersion through the soil profile.

---

## List of soil transport parameters

| Parameter Name                  | Type    | Unit           | Description                                                                                                                                                         | Example                                    |
|---------------------------------|---------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| `AD`                            | Float   |                | Empirical factor \(a\) used to calculate the moisture-dependent molecular diffusion coefficient according to Kersebaum (1989)                                       | `"AD": 0.002`                              |
| `DiffusionCoefficientStandard`  | Float   | m2 d-1         | Base diffusion coefficient \(D_0\) used in the moisture-dependent diffusion calculation                                                                             | `"DiffusionCoefficientStandard": 0.000214` |
| `DispersionLength`              | Float   | m              | Longitudinal dispersivity used to calculate hydrodynamic dispersion from pore-water velocity                                                                        | `"DispersionLength": 0.049`                |
| `NDeposition`                   | Float   | kg N ha-1 yr-1 | Annual atmospheric nitrogen deposition added to the soil surface. `NDeposition` is currently taken from `SiteParameters` and not from the soil transport parameter. | `"NDeposition": 0`                         |
| `type`                          | String  |                | Declares that this JSON defines soil transport parameters                                                                                                           | `"type": "UserSoilTransportParameters"`    |

!!! note
    These parameters are set globally for the entire soil profile.

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