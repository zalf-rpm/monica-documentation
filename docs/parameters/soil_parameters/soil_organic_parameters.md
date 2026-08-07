# Soil Organic Parameters

This section documents the `SoilOrganicModuleParameters` JSON object used by MONICA. 

The parameters control:

- soil organic matter (SOM) turnover
- soil microbial biomass (SMB) turnover
- added organic matter (AOM) decomposition
- nitrogen mineralisation and immobilisation
- urea hydrolysis and ammonia volatilisation
- nitrification, denitrification, and N<sub>2</sub>O production
- optional STICS-based nitrogen transformations

---

## JSON conventions

- Most numeric parameters may be supplied either as a plain number:

    ```json
    "QTenFactor": 2.9
    ```

    or as a value-unit array:

    ```json
    "QTenFactor": [2.9, ""]
    ```
  
    MONICA ignores the unit string when reading the parameter. Its serializer generally emits the array form.

---

## List of soil organic parameters

| Parameter Name                      | Type   | Unit                   | Description                                                                                                                                                                                                       | Example                                    |
|-------------------------------------|--------|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| `QTenFactor`                        | Float  |                        | Empirical sensitivity and shape parameter in MONICA's soil temperature response for decomposition                                                                                                                 | `"QTenFactor": 2.95`                       |
| `TempDecOptimal`                    | Float  | °C                     | Temperature parameter defining the optimum region of the empirical decomposition temperature response                                                                                                             | `"TempDecOptimal": 38.2`                   |
| `MoistureDecOptimal`                | Float  | fraction of saturation | Water-filled pore space (WFPS) fraction at which the decomposition moisture response is maximal. A value of `0.45` means 45% of saturation.                                                                       | `"MoistureDecOptimal": 0.45`               |
| `AOM_FastMaxC_to_N`                 | Float  | ratio                  | Maximum C:N ratio assigned to the fast AOM pool                                                                                                                                                                   | `"AOM_FastMaxC_to_N": 1000`                |
| `AOM_FastUtilizationEfficiency`     | Float  | fraction               | Fraction of decomposed fast AOM carbon incorporated into fast microbial biomass                                                                                                                                   | `"AOM_FastUtilizationEfficiency": 0.1`     |
| `AOM_SlowUtilizationEfficiency`     | Float  | fraction               | Fraction of decomposed slow AOM carbon incorporated into microbial biomass                                                                                                                                        | `"AOM_SlowUtilizationEfficiency": 0.4`     |
| `ActivationEnergy`                  | Float  | J mol-1                | Activation energy used in the Arrhenius temperature response for urea hydrolysis                                                                                                                                  | `"ActivationEnergy": 41000`                |
| `AmmoniaOxidationRateCoeffStandard` | Float  | d-1                    | Standard rate coefficient for oxidation of ammonium to nitrite in the standard nitrification model                                                                                                                | `"AmmoniaOxidationRateCoeffStandard": 0.1` |
| `AtmosphericResistance`             | Float  | s m-1                  | Compatibility parameter historically associated with NH<sub>3</sub> volatilisation. It is loaded and serialized but is not used by the current soil organic calculations.                                         | `"AtmosphericResistance": 0.0025`          |
| `CN_Ratio_SMB`                      | Float  | ratio                  | C:N ratio of the soil microbial biomass                                                                                                                                                                           | `"CN_Ratio_SMB": 6.7`                      |
| `Denit1`                            | Float  | fraction               | Moisture-response value at the upper intermediate breakpoint of the standard denitrification response. With the default settings, the response reaches `0.2` at a WFPS of 0.9.                                    | `"Denit1": 0.2`                            |
| `Denit2`                            | Float  | fraction               | Nominal lower WFPS breakpoint used by the standard denitrification moisture-response interpolation                                                                                                                | `"Denit2": 0.8`                            |
| `Denit3`                            | Float  | fraction               | Nominal upper intermediate WFPS breakpoint used by the standard denitrification moisture-response interpolation                                                                                                   | `"Denit3": 0.9`                            |
| `HydrolysisKM`                      | Float  |                        | Michaelis-Menten half-saturation constant for urea hydrolysis                                                                                                                                                     | `"HydrolysisKM": 0.00334`                  |
| `HydrolysisP1`                      | Float  |                        | First empirical coefficient used to derive the maximum urea hydrolysis rate from soil organic matter                                                                                                              | `"HydrolysisP1": 4.259e-12`                |
| `HydrolysisP2`                      | Float  |                        | Second empirical coefficient used to derive the maximum urea hydrolysis rate from soil organic matter                                                                                                             | `"HydrolysisP2": 1.408e-12`                |
| `ImmobilisationRateCoeffNH4`        | Float  | d-1                    | Maximum fraction of soil ammonium available for microbial immobilisation per day                                                                                                                                  | `"ImmobilisationRateCoeffNH4": 0.5`        |
| `ImmobilisationRateCoeffNO3`        | Float  | d-1                    | Maximum fraction of soil nitrate available for microbial immobilisation per day                                                                                                                                   | `"ImmobilisationRateCoeffNO3": 0.5`        |
| `Inhibitor_NH3`                     | Float  | kg N m-3               | Half-saturation-style coefficient in the NH<sub>3</sub> inhibition factor applied to nitrite oxidation                                                                                                            | `"Inhibitor_NH3": 1`                       |
| `LimitClayEffect`                   | Float  | fraction               | Parameter controlling the clay modifier applied to microbial maintenance. With the default decomposition response, it acts as the lower/asymptotic value of the clay response.                                    | `"LimitClayEffect": 0.25`                  |
| `MaxMineralisationDepth`            | Float  | m                      | Target maximum soil depth included in organic matter turnover and mineralisation. Whole model layers are included until their cumulative depth reaches or exceeds this value.                                     | `"MaxMineralisationDepth": 0.4`            |
| `N2OProductionRate`                 | Float  | d-1                    | Rate coefficient applied to the nitrite pool in the standard N<sub>2</sub>O production calculation, together with temperature and pH response factors.                                                            | `"N2OProductionRate": 0.015`               |
| `NitriteOxidationRateCoeffStandard` | Float  | d-1                    | Standard rate coefficient for oxidation of nitrite to nitrate in the standard nitrification model                                                                                                                 | `"NitriteOxidationRateCoeffStandard": 0.2` |
| `PartSMB_Fast_to_SOM_Fast`          | Float  | fraction               | Fraction of dead fast microbial biomass transferred to the fast SOM pool                                                                                                                                          | `"PartSMB_Fast_to_SOM_Fast": 0.6`          |
| `PartSMB_Slow_to_SOM_Fast`          | Float  | fraction               | Fraction of dead slow microbial biomass transferred to the fast SOM pool                                                                                                                                          | `"PartSMB_Slow_to_SOM_Fast": 0.6`          |
| `PartSOM_Fast_to_SOM_Slow`          | Float  | fraction               | Fraction of decomposed fast SOM transferred to the slow SOM pool                                                                                                                                                  | `"PartSOM_Fast_to_SOM_Slow": 0.3`          |
| `PartSOM_to_SMB_Fast`               | Float  | fraction               | Fraction of initial soil organic carbon assigned to the fast microbial biomass pool during pool initialization                                                                                                    | `"PartSOM_to_SMB_Fast": 0.0002`            |
| `PartSOM_to_SMB_Slow`               | Float  | fraction               | Fraction of initial soil organic carbon assigned to the slow microbial biomass pool during pool initialization                                                                                                    | `"PartSOM_to_SMB_Slow": 0.015`             |
| `SMB_FastDeathRateStandard`         | Float  | d-1                    | Standard death rate coefficient of the fast microbial biomass pool                                                                                                                                                | `"SMB_FastDeathRateStandard": 0.01`        |
| `SMB_FastMaintRateStandard`         | Float  | d-1                    | Standard maintenance respiration rate of the fast microbial biomass pool                                                                                                                                          | `"SMB_FastMaintRateStandard": 0.01`        |
| `SMB_SlowDeathRateStandard`         | Float  | d-1                    | Standard death rate coefficient of the slow microbial biomass pool                                                                                                                                                | `"SMB_SlowDeathRateStandard": 0.001`       |
| `SMB_SlowMaintRateStandard`         | Float  | d-1                    | Standard maintenance respiration rate of the slow microbial biomass pool                                                                                                                                          | `"SMB_SlowMaintRateStandard": 0.001`       |
| `SMB_UtilizationEfficiency`         | Float  | fraction               | Fraction of eligible carbon from dead microbial biomass that is incorporated into the fast SMB pool. The remainder contributes to respiration.                                                                    | `"SMB_UtilizationEfficiency": 0`           |
| `SOM_FastDecCoeffStandard`          | Float  | d-1                    | Standard decomposition rate coefficient for the fast SOM pool. Temperature and moisture response factors are applied during simulation.                                                                           | `"SOM_FastDecCoeffStandard": 0.00014`      |
| `SOM_FastUtilizationEfficiency`     | Float  | fraction               | Fraction of the decomposed fast SOM carbon not transfered to slow SOM that is incorporated into slow microbial biomass                                                                                            | `"SOM_FastUtilizationEfficiency": 0.5`     |
| `SOM_SlowDecCoeffStandard`          | Float  | d-1                    | Standard decomposition rate coefficient for the slow SOM pool. Temperature and moisture response factors are applied during simulation.                                                                           | `"SOM_SlowDecCoeffStandard": 0.000043`     |
| `SOM_SlowUtilizationEfficiency`     | Float  | fraction               | Fraction of decomposed slow SOM carbon incorporated into slow microbial biomass                                                                                                                                   | `"SOM_SlowUtilizationEfficiency": 0.4`     |
| `SpecAnaerobDenitrification`        | Float  | g gas-N (g CO2-C)-1    | Potential anaerobic denitrification relative to microbial CO<sub>2</sub>-C production                                                                                                                             | `"SpecAnaerobDenitrification": 0.1`        |
| `TransportRateCoeff`                | Float  | d-1                    | Maximum fraction of the nitrate pool that can be consumed by denitrification per day in the standard denitrification model. Despite its name, it does not transport dissolved organic matter between soil layers. | `"TransportRateCoeff": 0.1`                |
| `type`                              | String |                        | Serialization metadata identifying the object as soil organic parameters                                                                                                                                          | `"type": "UserSoilOrganicParameters"`      |

---

## Decomposition-response compatibility switches

MONICA also accepts the following internal compatibility switches:

| Parameter Name                              | Type    | Description                                                                                                                    | Example                                             |
|---------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| `__enable_kaiteew_TempOnDecompostion__`     | Boolean | Enables the current empirical temperature response. If `false`, the legacy temperature response is used.                       | `"__enable_kaiteew_TempOnDecompostion__": true`     |
| `__enable_kaiteew_MoistureOnDecompostion__` | Boolean | Enables the current water-filled pore space decomposition response. If `false`, the legacy pF-based moisture response is used. | `"__enable_kaiteew_MoistureOnDecompostion__": true` |
| `__enable_kaiteew_ClayOnDecompostion__`     | Boolean | Enables the current sigmoid clay response. If `false`, the legacy threshold response is used.                                  | `"__enable_kaiteew_ClayOnDecompostion__": true`     |

---

## Optional STICS nitrogen parameters

The optional `stics` object configures alternative nitrification, denitrification, and N<sub>2</sub>O calculations.

The three processes are selected independently:

- `use_nit` selects STICS nitrification
- `use_denit` selects STICS denitrification
- `use_n2o` selects STICS N<sub>2</sub>O calculation

When a switch is `false`, MONICA uses its standard implementation for that process.

### Process switches and option codes

| Parameter Name           | Type    | Description                                                                                                                            | Example                       |
|--------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| `use_n2o`                | Boolean | Enables the STICS calculation of N<sub>2</sub>O from nitrificaiton and denitrification                                                 | `"use_n2o": false`            |
| `use_nit`                | Boolean | Enables the STICS nitrification                                                                                                        | `"use_nit": false`            |
| `use_denit`              | Boolean | Enables the STICS denitrification                                                                                                      | `"use_denit": false`          |
| `code_vnit`              | Integer | Selects the potential nitrification formulation: `1` = linear fraction using `fnx`; `2` = Michaelis-Menten using `vnitmax` and `Kamm`. | `"code_vnit": 1`              |
| `code_tnit`              | Integer | Selects the nitrification temperature response: `1` = piecewise linear; `2` = Gaussian                                                 | `"code_tnit": 2`              |
| `code_rationit`          | Integer | Selects the nitrification N<sub>2</sub>O ratio: `1` = constant `rationit`; `2` = WFPS-dependent ratio                                  | `"code_rationit": 2`          |
| `code_hourly_wfps_nit`   | Integer | Compatibility option for hourly nitrification WFPS handling. Currently parsed but not used by the active calculation.                  | `"code_hourly_wfps_nit": 2`   |
| `code_pdenit`            | Integer | Selects potential denitrification: `1` = constant `vpotdenit`; `2` = soil organic carbon response.                                     | `"code_pdenit": 1`            |
| `code_ratiodenit`        | Integer | Selects the denitrification N<sub>2</sub>O ratio: `1` = constant `ratiodenit`; `2` = pH-, WFPS-, and nitrate-dependent ratio.          | `"code_ratiodenit": 2`        |
| `code_hourly_wfps_denit` | Integer | Compatibility option for hourly denitrification WFPS handling. Currently parsed but not used by the active calculation.                | `"code_hourly_wfps_denit": 2` |

### STICS nitrification parameters

| Parameter Name  | Type  | Unit                    | Description                                                                                                                                                                | Example                 |
|-----------------|-------|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------|
| `hminn`         | Float | fraction                | Multiplier of field capacity used to construct the lower WFPS threshold for nitrification                                                                                  | `"hminn": 0.3`          |
| `hoptn`         | Float | fraction                | Multiplier of field capacity used to construct the beginning of the optimal WFPS range. The response remains optimal up to field capacity and decreases toward saturation. | `"hoptn": 0.9`          |
| `pHminnit`      | Float | pH                      | pH at or below which the nitrification pH response is zero                                                                                                                 | `"pHminnit": 4.0`       |
| `pHmaxnit`      | Float | pH                      | pH at or above which the nitrification pH response reaches one                                                                                                             | `"pHmaxnit": 7.2`       |
| `nh4_min`       | Float | mg NH4-N kg-1 soil      | Residual ammonium content excluded from nitrification                                                                                                                      | `"nh4_min": 1.0`        |
| `fnx`           | Float | d-1                     | Fraction of ammonium above `nh4_min` nitrified per day when `code_vnit = 1`                                                                                                | `"fnx": 0.8`            |
| `vnitmax`       | Float | mg NH4-N kg-1 soil d-1  | Maximum nitrification rate when `code_vnit = 2`                                                                                                                            | `"vnitmax": 27.3`       |
| `Kamm`          | Float | mg NH4-N L-1 soil water | Ammonium half-saturation constant when `code_vnit = 2`                                                                                                                     | `"Kamm": 24`            |
| `tnitmin`       | Float | °C                      | Lower temperature boundary of the piecewise-linear response used when `code_tnit = 1`                                                                                      | `"tnitmin": 5.0`        |
| `tnitopt`       | Float | °C                      | Beginning of the optimal-temperature plateau when `code_tnit = 1`                                                                                                          | `"tnitopt": 30.0`       |
| `tnitop2`       | Float | °C                      | End of the optimal-temperature plateau when `code_tnit = 1`                                                                                                                | `"tnitop2": 35.0`       |
| `tnitmax`       | Float | °C                      | Upper temperature boundary above which the piecewise-linear response is zero                                                                                               | `"tnitmax": 58.0`       |
| `tnitopt_gauss` | Float | °C                      | Optimum temperature of the Gaussian nitrification response used when `code_tnit = 2`                                                                                       | `"tnitopt_gauss": 32.5` |
| `scale_tnitopt` | Float | °C                      | Width parameter of the Gaussian nitrification temperature response                                                                                                         | `"scale_tnitopt": 16.0` |
| `rationit`      | Float | fraction                | Constant fraction of nitrified N emitted as N₂O when `code_rationit = 1`                                                                                                   | `"rationit": 0.0016`    |

### STICS denitrification parameters

| Parameter Name    | Type  | Unit                    | Description                                                                                                                  | Example                   |
|-------------------|-------|-------------------------|------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| `wfpsc`           | Float | fraction                | WFPS threshold at or below which the denitrification moisture response is zero                                               | `"wfpsc": 0.62`           |
| `tdenitopt_gauss` | Float | °C                      | Optimum temperature of the Gaussian denitrification response                                                                 | `"tdenitopt_gauss": 47.0` |
| `scale_tdenitopt` | Float | °C                      | Width parameter of the Gaussian denitrification temperature response                                                         | `"scale_tdenitopt": 25.0` |
| `Kd`              | Float | mg NO3-N L-1 soil water | Nitrate half-saturation constant in the denitrification response                                                             | `"Kd": 148.0`             |
| `k_desat`         | Float | d-1                     | Compatibility parameter associated with desaturation. It is parsed but is not used by the current calculation.               | `"k_desat": 3.0`          |         
| `cmin_pdenit`     | Float | % soil organic C        | Soil organic carbon value corresponding to `min_pdenit` when `code_pdenit = 2`.                                              | `"cmin_pdenit": 1.0`      |
| `cmax_pdenit`     | Float | % soil organic C        | Soil organic carbon value corresponding to `max_pdenit` when `code_pdenit = 2`.                                              | `"cmax_pdenit": 6.0`      |
| `min_pdenit`      | Float | mg N kg-1 soil d-1      | Lower potential-denitrification rate used by the organic-carbon response.                                                    | `"min_pdenit": 1.0`       |
| `max_pdenit`      | Float | mg N kg-1 soil d-1      | Upper potential-denitrification rate used by the organic-carbon response.                                                    | `"max_pdenit": 20.0`      |
| `vpotdenit`       | Float | kg N ha-1 d-1           | Constant potential-denitrification parameter selected when `code_pdenit = 1`                                                 | `"vpotdenit": 2.0`        |
| `profdenit`       | Float | cm                      | Configured denitrification depth. It is parsed but is not currently used to restrict the active denitrification calculation. | `"profdenit": 20.0`       |
| `pHminden`        | Float | pH                      | Lower pH breakpoint of the STICS denitrification N<sub>2</sub>O-fraction response used when `code_ratiodenit = 2`            | `"pHminden": 5.6`         |
| `pHmaxden`        | Float | pH                      | Upper pH breakpoint at which the pH contribution to the denitrification N<sub>2</sub>O fraction reaches zero                 | `"pHmaxden": 9.2`         |
| `ratiodenit`      | Float | fraction                | Constant fraction of denitrified N emitted as N<sub>2</sub>O when `code_ratiodenit = 1`                                      | `"ratiodenit": 0.2`       |

---

## Example: `soilorganic.json`

```json
{
    "QTenFactor": [2.95, ""],
    "TempDecOptimal": [38.2, "C"],
    "MoistureDecOptimal": [0.45, "%"],
    "AOM_FastMaxC_to_N": [1000, ""],
    "AOM_FastUtilizationEfficiency": [0.1, ""],
    "AOM_SlowUtilizationEfficiency": [0.4, ""],
    "ActivationEnergy": [41000, ""],
    "AmmoniaOxidationRateCoeffStandard": [0.1, "d-1"],
    "AtmosphericResistance": [0.0025, "s m-1"],
    "CN_Ratio_SMB": [6.7, ""],
    "Denit1": [0.2, ""],
    "Denit2": [0.8, ""],
    "Denit3": [0.9, ""],
    "HydrolysisKM": [0.00334, ""],
    "HydrolysisP1": [4.259e-12, ""],
    "HydrolysisP2": [1.408e-12, ""],
    "ImmobilisationRateCoeffNH4": [0.5, "d-1"],
    "ImmobilisationRateCoeffNO3": [0.5, "d-1"],
    "Inhibitor_NH3": [1, "kg N m-3"],
    "LimitClayEffect": [0.25, "kg kg-1"],
    "MaxMineralisationDepth": 0.4,
    "N2OProductionRate": [0.015, "d-1"],
    "NitriteOxidationRateCoeffStandard": [0.2, "d-1"],
    "PartSMB_Fast_to_SOM_Fast": [0.6, ""],
    "PartSMB_Slow_to_SOM_Fast": [0.6, ""],
    "PartSOM_Fast_to_SOM_Slow": [0.3, ""],
    "PartSOM_to_SMB_Fast": [0.0002, ""],
    "PartSOM_to_SMB_Slow": [0.015, ""],
    "SMB_FastDeathRateStandard": [0.01, "d-1"],
    "SMB_FastMaintRateStandard": [0.01, "d-1"],
    "SMB_SlowDeathRateStandard": [0.001, "d-1"],
    "SMB_SlowMaintRateStandard": [0.001, "d-1"],
    "SMB_UtilizationEfficiency": [0, "d-1"],
    "SOM_FastDecCoeffStandard": [0.00014, "d-1"],
    "SOM_FastUtilizationEfficiency": [0.5, ""],
    "SOM_SlowDecCoeffStandard": [0.000043, "d-1"],
    "SOM_SlowUtilizationEfficiency": [0.4, ""],
    "SpecAnaerobDenitrification": [0.1, "g gas-N g CO2-C-1"],
    "TransportRateCoeff": [0.1, "d-1"],
    "type": "UserSoilOrganicParameters"
}
```