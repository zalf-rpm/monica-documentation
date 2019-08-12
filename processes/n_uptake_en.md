# N uptake

The N uptake of the crop from soil is modelled following an idea of Kersebaum (1989). The crop’s daily N requirement is calculated using

$`\small N_{pot} = \left( N_{target} \cdot W + N_{max \, root} \cdot W_{root} + N_{target} \cdot \frac{W_b}{p_N} - N_{crop}^{*} \right) \cdot \Delta t`$

$`\small  N_{pot}`$	Daily N uptake potential	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small N_{target} `$ Maximum N concentration in above-ground plant parts $`\small [kg \, N \,kg \, TM^{-1}] `$<br>
$`\small W`$	Above-ground dry matter biomass	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small N_{max \, root} `$	Maximum N concentration in the root	$`\small [kg \, N \,kg \, TM^{-1}] `$<br>
$`\small W_{root}`$	Root dry matter biomass	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small W_b`$	Below-ground dry matter biomass (not root)	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small p_N`$	N distribution coefficient	 <br>
$`\small N_{crop}^{*}`$	Total crop N content in the past time step	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small \Delta t`$	Time step	$`\small [d] `$<br>

Its maximum amount is $`\small 6 \, kg \, N \, ha^{-1} \, d^{-1} `$. The daily N uptake is determined by the root length and a threshold which decreases linearly with the plant’s ontogenesis and thus considers the decreasing fraction of active root surface as compared to transport roots.

$`\small \Delta N_{lim} = L_{root} \cdot \left( N_{up \, max} - \frac{DD_{act}}{DD_{crop}} \right)`$

$`\small \Delta N_{lim}`$	Limit of the daily N uptake	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small L_{root}`$	Total root length	$`\small [m \, m^{-2}]`$<br>
$`\small N_{up \, max}`$	Plant-specific maximum N uptake	$`\small [kg \, N \, m \, Wurzel^{-1}]`$<br>
$`\small DD_{act}`$	Actual temperature sum	$`\small [^{\circ}C \, d]`$<br>
$`\small DD_{crop}`$	Plant-specific total temperature sum	$`\small [^{\circ}C \, d]`$<br>

It is assumed that N is taken up solely in the form of nitrate. In this form it is convectively transported in the upward stream of transpiration water.

$`\small N_{konv \, max} = \sum_{z=1}^{Q \cdot R_z} T_z \cdot c_z \cdot \Delta z \cdot \Delta t `$

$`\small N_{konv \, max}`$	Maximum convective N uptake	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small R_z `$	Rooting depth	$`\small [m]`$<br>
$`\small q`$	Ratio absolute to simulated rooting depth	 <br>
$`\small T_z`$	Transpiration	$`\small [mm]`$<br>
$`\small c_z`$	N concentration in the soil solution	$`\small [kg \, N \, m^{-3}]`$<br>
$`\small \Delta z`$	Layer depth	$`\small [m] `$<br>
$`\small \Delta t `$	Time step	$`\small [d]`$<br>

In case the convective supply exceeds the crop’s uptake potential, N uptake will be calculated in the single layers as

$`\small N_{konv_z} = T_z \cdot c_z \cdot \frac{N_{pot}}{N_{mas}} \cdot \Delta z \cdot \Delta t`$

$`\small N_{konv_z}`$	Daily convective N uptake from the soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small T_z`$	Transpiration from layer z	$`\small [mm] `$<br>
$`\small c_z`$	N concentration in the soil solution	$`\small [kg \, N \, m^{-3}]`$<br>
$`\small N_{pot}`$	Daily N uptake potential	$`\small [kg \, N \, ha^{-1}]`$<br>
$`\small N_{mas}`$	Limit of the daily N uptake	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small \Delta z`$	Layer depth	$`\small [m]`$<br>
$`\small \Delta t `$	Time step	$`\small [d]`$<br>

In case the convective N supply does not satisfy the uptake potential an additional supply via diffusion will be considered (Baldwin et al. 1973) whose maximum contribution is calculated as follows:

$`\small N_{diff \, max} = \sum_{z=1}^{q \cdot R_z} N_{diff \, max_z} = \sum_{z=1}^{q \cdot R_z} 2 \cdot \pi \cdot r_w \cdot \Lambda_z \cdot D \cdot \frac{c_z - c_{min}} {r_z} \cdot \Delta z \cdot  \Delta t `$

$`\small N_{diff \, max}`$	Maximum diffusive N uptake	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small R_z`$	Rooting depth	$`\small [m] `$<br>
$`\small q `$	Ratio absolute to simulated rooting depth	 <br>
$`\small N_{diff \, max_z}`$	Maximum diffusive N uptake from soil layer z	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small r_w`$	Root radius	$`\small [m] `$<br>
$`\small \Lambda_z `$	Root length density in layer z	$`\small [m \, m^{-3}] `$<br>
$`\small D`$	Effective dispersion coefficient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small c_z`$	N concentration in the soil solution	$`\small [kg \, N \, m^{-3}] `$<br>
$`\small c_{min}`$	Minimum N concentration at the root surface (1.4·10–6)	$`\small [kg \, N \, m^{-3}] `$<br>
$`\small r_z`$	Half distance between roots	$`\small [m] `$<br>
$`\small \Delta z`$	Layer depth	$`\small [m] `$<br>
$`\small \Delta t `$	Time step	$`\small [d] `$<br>

using half the distance between neighbouring roots, for which an equal distribution is assumed

$`\small r_z = (\pi \cdot \Lambda_z)^{-0.5}`$

$`\small r_z`$	Half distance between roots	$`\small [m]`$<br>
$`\small \Lambda_z `$	Root length density in layer z	$`\small [m \, m^{-3}]`$<br>

and the effective dispersion coefficient

$`\small D= \frac{1}{\tau} \cdot D_0 + D_v \cdot \vert \frac{q}{\theta}\vert`$

$`\small D`$	Effective dispersion coefficient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small \tau `$	Tortuosity	 <br>
$`\small D_0`$	Diffusion coefficient in solution (2.14·10–5)	$`\small [m^2 \, d^{-1}]`$<br>
$`\small d_v`$	Dispersion factor (0.05)	$`\small [m] `$<br>
$`\small q`$	Water flux density	$`\small [m^2 \, d^{-1}]`$<br>
$`\small \theta`$	Volumetric water content	$`\small [m^3 \, m^{-3}]`$<br>

where

$`\small \tau = \frac {\theta}{a \cdot e^{b \cdot \theta}}`$

$`\small \tau`$	Tortuosity	 <br>
$`\small \theta`$	Volumetric water content	$`\small [m^3 \, m^{-3}]`$<br>
$`\small a, b`$	Factors	 <br>

In case the additional diffusive supply exceeds the crop’s uptake potential, it will be distributed over the soil layers, only limited by the uptake potential.

$`\small N_{diff_z} = (N_{pot} - N_{konv \, max}) \cdot \frac{N_{diff \, max_z}}{N_{diff \, max}}`$

$`\small N_{diff_z} `$	Daily diffusive N uptake from the soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{pot}  `$	Daily N uptake potential	$`\small[kg \, N \, m^{-2}] `$<br>
$`\small N_{konv \, max}  `$	Maximum convective N uptake	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{diff \, max_z} `$	Maximum diffusive N uptake from soil layer z	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small N_{diff \, max} `$	Maximum diffusive N uptake	$`\small [kg \, N \, m^{-2}]`$<br>

The daily N uptake is finally calculated from

$`\small N_{up_z} = N_{konv_z} + N_{diff_z} \,\,\,\,\,\,\,\,\,\,\, N_{up_z} \leq N_z - N_{min\,av}`$

$`\small N_{up_z} `$	Daily N uptake from soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{konv_z} `$	Daily convective N uptake from soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{diff_z} `$	Daily diffusive N uptake from soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_z`$	N content of the soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{min\,av} `$	Minimum N content in the soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>

and

$`\small N_{up} = \sum_{z=1}^{q \cdot R_z} N_{up_z} `$

$`\small N_{up} `$	Daily N uptake	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small R_z`$	Rooting depth	$`\small [m]`$<br>
$`\small q`$	Ratio of absolute to simulated rooting depth	 <br>
$`\small N_{up_z}`$	Daily N uptake from soil layer z	$`\small [kg \, N \, m^{-2}]`$<br>

## References
Baldwin, J.P. et al. (1973): Uptake of solutes by multiple root systems from soil. III. A model for calculating the solute uptake by a randomly dispersed root system developing in a finite volume of soil. Plant Soil 38, 621 – 635.

Groot, J.J.R. (1987): Simulation of nitrogen balance in a system of winter wheat and soil. Simulation Report CABO-TT 13. Centre for agrobiological research and Department of Theoretical Production Ecology, Landwirtschaftliche Universität Wageningen, Niederlande.

Kersebaum, K.C. (1989): Die Simulation der Stickstoff-Dynamik von Ackerböden. Dissertation, Universität Hannover, p. 143.