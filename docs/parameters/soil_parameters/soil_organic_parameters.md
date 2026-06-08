# Soil Organic Parameters

This section provides an overview of the key-value pairs in the `UserSoilOrganicParameters` JSON file used by MONICA. These parameters control the turnover of soil organic matter (SOM), microbial biomass (SMB), nitrification, denitrification, and nitrogen cycling processes in the soil.

---

## List of soil organic parameters

| Parameter Name                        | Type    | Unit              | Description                                                                                                           | Example                                                    |
|---------------------------------------|---------|-------------------|-----------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| `QTenFactor`                          | Float   |                   | Q10 temperature sensitivity factor controlling the increase in decomposition rate per 10°C rise in temperature       | `"QTenFactor": 2.95`                                       |
| `TempDecOptimal`                      | Float   | °C                | Optimal temperature for decomposition of organic matter                                                              | `"TempDecOptimal": 38.2`                                   |
| `MoistureDecOptimal`                  | Float   | %                 | Optimal soil moisture for decomposition of organic matter                                                            | `"MoistureDecOptimal": 0.45`                               |
| `AOM_FastMaxC_to_N`                   | Float   |                   | Maximum C:N ratio of the fast decomposable added organic matter (AOM) pool                                           | `"AOM_FastMaxC_to_N": 1000`                                |
| `AOM_FastUtilizationEfficiency`       | Float   |                   | Fraction of fast AOM carbon that is incorporated into microbial biomass during decomposition                         | `"AOM_FastUtilizationEfficiency": 0.1`                     |
| `AOM_SlowUtilizationEfficiency`       | Float   |                   | Fraction of slow AOM carbon that is incorporated into microbial biomass during decomposition                         | `"AOM_SlowUtilizationEfficiency": 0.4`                     |
| `ActivationEnergy`                    | Float   | J mol-1           | Activation energy used in the Arrhenius equation for temperature-dependent decomposition                             | `"ActivationEnergy": 41000`                                |
| `AmmoniaOxidationRateCoeffStandard`   | Float   | d-1               | Standard rate coefficient for the first step of nitrification (ammonia oxidation to nitrite)                         | `"AmmoniaOxidationRateCoeffStandard": 0.1`                 |
| `AtmosphericResistance`               | Float   | s m-1             | Aerodynamic resistance used in the calculation of ammonia volatilisation from the soil surface                       | `"AtmosphericResistance": 0.0025`                          |
| `CN_Ratio_SMB`                        | Float   |                   | C:N ratio of the soil microbial biomass                                                                              | `"CN_Ratio_SMB": 6.7`                                      |
| `Denit1`                              | Float   |                   | First empirical parameter controlling denitrification rate as a function of soil nitrate content                     | `"Denit1": 0.2`                                            |
| `Denit2`                              | Float   |                   | Second empirical parameter controlling denitrification rate as a function of soil water content                      | `"Denit2": 0.8`                                            |
| `Denit3`                              | Float   |                   | Third empirical parameter controlling denitrification rate as a function of soil organic carbon                      | `"Denit3": 0.9`                                            |
| `HydrolysisKM`                        | Float   |                   | Michaelis-Menten half-saturation constant for urea hydrolysis                                                        | `"HydrolysisKM": 0.00334`                                  |
| `HydrolysisP1`                        | Float   |                   | First rate parameter for the urea hydrolysis equation                                                                | `"HydrolysisP1": 4.259e-12`                                |
| `HydrolysisP2`                        | Float   |                   | Second rate parameter for the urea hydrolysis equation                                                               | `"HydrolysisP2": 1.408e-12`                                |
| `ImmobilisationRateCoeffNH4`          | Float   | d-1               | Rate coefficient for microbial immobilisation of ammonium (NH4) from soil solution                                   | `"ImmobilisationRateCoeffNH4": 0.5`                        |
| `ImmobilisationRateCoeffNO3`          | Float   | d-1               | Rate coefficient for microbial immobilisation of nitrate (NO3) from soil solution                                    | `"ImmobilisationRateCoeffNO3": 0.5`                        |
| `Inhibitor_NH3`                       | Float   | kg N m-3          | Ammonia concentration threshold above which nitrification is inhibited                                              | `"Inhibitor_NH3": 1`                                       |
| `LimitClayEffect`                     | Float   | kg kg-1           | Clay content threshold above which clay no longer provides additional protection to SOM from decomposition           | `"LimitClayEffect": 0.25`                                  |
| `MaxMineralisationDepth`              | Float   | m                 | Maximum soil depth at which organic matter mineralisation is simulated                                               | `"MaxMineralisationDepth": 0.4`                            |
| `N2OProductionRate`                   | Float   | d-1               | Rate coefficient for the production of N2O during nitrification                                                     | `"N2OProductionRate": 0.015`                               |
| `NitriteOxidationRateCoeffStandard`   | Float   | d-1               | Standard rate coefficient for the second step of nitrification (nitrite oxidation to nitrate)                        | `"NitriteOxidationRateCoeffStandard": 0.2`                 |
| `PartSMB_Fast_to_SOM_Fast`            | Float   |                   | Fraction of fast microbial biomass (SMB fast) that flows into the fast SOM pool upon death                           | `"PartSMB_Fast_to_SOM_Fast": 0.6`                          |
| `PartSMB_Slow_to_SOM_Fast`            | Float   |                   | Fraction of slow microbial biomass (SMB slow) that flows into the fast SOM pool upon death                           | `"PartSMB_Slow_to_SOM_Fast": 0.6`                          |
| `PartSOM_Fast_to_SOM_Slow`            | Float   |                   | Fraction of fast SOM that is transferred to the slow SOM pool during decomposition                                   | `"PartSOM_Fast_to_SOM_Slow": 0.3`                          |
| `PartSOM_to_SMB_Fast`                 | Float   |                   | Fraction of SOM turnover products that enter the fast microbial biomass pool                                         | `"PartSOM_to_SMB_Fast": 0.0002`                            |
| `PartSOM_to_SMB_Slow`                 | Float   |                   | Fraction of SOM turnover products that enter the slow microbial biomass pool                                         | `"PartSOM_to_SMB_Slow": 0.015`                             |
| `SMB_FastDeathRateStandard`           | Float   | d-1               | Standard death rate of the fast microbial biomass pool under reference conditions                                    | `"SMB_FastDeathRateStandard": 0.01`                        |
| `SMB_FastMaintRateStandard`           | Float   | d-1               | Standard maintenance respiration rate of the fast microbial biomass pool                                             | `"SMB_FastMaintRateStandard": 0.01`                        |
| `SMB_SlowDeathRateStandard`           | Float   | d-1               | Standard death rate of the slow microbial biomass pool under reference conditions                                    | `"SMB_SlowDeathRateStandard": 0.001`                       |
| `SMB_SlowMaintRateStandard`           | Float   | d-1               | Standard maintenance respiration rate of the slow microbial biomass pool                                             | `"SMB_SlowMaintRateStandard": 0.001`                       |
| `SMB_UtilizationEfficiency`           | Float   | d-1               | Efficiency with which microbial biomass utilises substrate carbon                                                    | `"SMB_UtilizationEfficiency": 0`                           |
| `SOM_FastDecCoeffStandard`            | Float   | d-1               | Standard decomposition rate coefficient for the fast SOM pool                                                        | `"SOM_FastDecCoeffStandard": 0.00014`                      |
| `SOM_FastUtilizationEfficiency`       | Float   |                   | Fraction of fast SOM carbon converted to microbial biomass during decomposition                                      | `"SOM_FastUtilizationEfficiency": 0.5`                     |
| `SOM_SlowDecCoeffStandard`            | Float   | d-1               | Standard decomposition rate coefficient for the slow SOM pool                                                        | `"SOM_SlowDecCoeffStandard": 0.000043`                     |
| `SOM_SlowUtilizationEfficiency`       | Float   |                   | Fraction of slow SOM carbon converted to microbial biomass during decomposition                                      | `"SOM_SlowUtilizationEfficiency": 0.4`                     |
| `SpecAnaerobDenitrification`          | Float   | g gas-N g CO2-C-1 | Specific anaerobic denitrification rate relating gas-N production to CO2-C respiration                               | `"SpecAnaerobDenitrification": 0.1`                        |
| `TransportRateCoeff`                  | Float   | d-1               | Rate coefficient for the transport of dissolved organic matter between soil layers                                    | `"TransportRateCoeff": 0.1`                                |
| `type`                                | String  |                   | Declares that this JSON defines soil organic parameters                                                              | `"type": "UserSoilOrganicParameters"`                      |

!!! note
    This file also contains an optional `stics` sub-object with additional nitrification and denitrification parameters from the STICS model. These are only active when the corresponding `use_n2o`, `use_nit`, or `use_denit` flags are set to `true`.

---

## STICS nitrification and denitrification parameters (optional)

The `stics` block contains parameters for an alternative N2O, nitrification, and denitrification submodel. These are disabled by default (`false`) and only relevant when STICS-based N cycling is activated.

| Parameter Name          | Type    | Unit               | Description                                                                                                  | Example                          |
|-------------------------|---------|--------------------|--------------------------------------------------------------------------------------------------------------|----------------------------------|
| `use_n2o`               | Boolean |                    | Activates the STICS N2O submodel                                                                             | `"use_n2o": false`               |
| `use_nit`               | Boolean |                    | Activates the STICS nitrification submodel                                                                   | `"use_nit": false`               |
| `use_denit`             | Boolean |                    | Activates the STICS denitrification submodel                                                                 | `"use_denit": false`             |
| `hminn`                 | Float   |                    | Fraction of field capacity below which nitrification is zero                                                 | `"hminn": 0.3`                   |
| `hoptn`                 | Float   |                    | Fraction of field capacity above which nitrification is optimal                                              | `"hoptn": 0.9`                   |
| `pHminnit`              | Float   |                    | pH below which nitrification is zero                                                                         | `"pHminnit": 4.0`                |
| `pHmaxnit`              | Float   |                    | pH above which nitrification is optimal                                                                      | `"pHmaxnit": 7.2`                |
| `nh4_min`               | Float   | mg NH4-N kg-1 soil | Minimum soil ammonium content not available for nitrification (fixed ammonium)                               | `"nh4_min": 1.0`                 |
| `pHminden`              | Float   |                    | pH below which denitrification only produces N2O (at ~80% WFPS)                                             | `"pHminden": 5.6`                |
| `pHmaxden`              | Float   |                    | pH above which denitrification only produces N2 (at ~80% WFPS)                                              | `"pHmaxden": 9.2`                |
| `wfpsc`                 | Float   |                    | Soil water-filled pore space (WFPS) threshold beyond which denitrification becomes active                    | `"wfpsc": 0.62`                  |
| `tdenitopt_gauss`       | Float   | °C                 | Optimum temperature for denitrification                                                                      | `"tdenitopt_gauss": 47`          |
| `scale_tdenitopt`       | Float   | °C                 | Parameter controlling the temperature range favourable for denitrification                                   | `"scale_tdenitopt": 25`          |
| `Kd`                    | Float   | mg NO3-N L-1       | Half-saturation constant relating NO3 concentration to the denitrification rate                              | `"Kd": 148`                      |
| `fnx`                   | Float   | d-1                | Potential nitrification rate as fraction of available ammonium nitrified per day (linear option)             | `"fnx": 0.8`                     |
| `vnitmax`               | Float   | mg NH4-N kg-1 d-1  | Nitrification potential under Michaelis-Menten kinetics                                                      | `"vnitmax": 27.3`                |
| `Kamm`                  | Float   | mg NH4-N L-1       | Half-saturation constant for NH4 concentration in nitrification rate (Michaelis-Menten option)               | `"Kamm": 24`                     |
| `tnitmin`               | Float   | °C                 | Temperature below which nitrification is zero (piecewise linear option)                                      | `"tnitmin": 5.0`                 |
| `tnitopt`               | Float   | °C                 | Temperature above which nitrification is optimal (piecewise linear option)                                   | `"tnitopt": 30.0`                |
| `tnitop2`               | Float   | °C                 | Temperature above which nitrification starts to decrease after the optimum (piecewise linear option)         | `"tnitop2": 35.0`                |
| `tnitmax`               | Float   | °C                 | Temperature above which nitrification is zero (piecewise linear option)                                      | `"tnitmax": 58.0`                |
| `tnitopt_gauss`         | Float   | °C                 | Optimum temperature for nitrification (Gaussian function option)                                             | `"tnitopt_gauss": 32.5`          |
| `scale_tnitopt`         | Float   | °C                 | Parameter controlling the temperature range favourable to nitrification (Gaussian option)                    | `"scale_tnitopt": 16.0`          |
| `rationit`              | Float   |                    | Proportion of nitrified nitrogen emitted as N2O (constant ratio option)                                      | `"rationit": 0.0016`             |
| `profdenit`             | Integer | cm                 | Maximum soil depth affected by denitrification                                                               | `"profdenit": 20`                |
| `vpotdenit`             | Float   | kg N ha-1 d-1      | Denitrification potential over the soil thickness defined by `profdenit` (constant potential option)         | `"vpotdenit": 2.0`               |
| `ratiodenit`            | Float   |                    | Proportion of denitrified nitrogen emitted as N2O (constant ratio option)                                    | `"ratiodenit": 0.2`              |

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