# Evaporation

Reference evapotranspiration for short-cut grass $ET_0$ is calculated using the Penman-Monteith method (1998).

$$ET_0 = \frac{0.408 \cdot \Delta \cdot (R_n - G) + \gamma \cdot \frac{900}{\gamma + 273} \cdot u_2 \cdot (e_s - e_a) } {\Delta + \gamma \cdot (1+\frac{r_a}{r_s})}$$

$\Delta$ Slope of the vapour pressure curve $[kPa\,K^{-1}]$<br>
$R_n$ Net radiation at the crop surface	$[MJ\, m^{-2} \, d^{-1}]$<br>
$G$	Soil heat flux density $[MJ\, m^{-2} \, d^{-1}]$<br>
$T$	Mean daily air temperature at 2 m height $[^{\circ} C]$<br>
$u_2$ Wind speed at 2 m height $[m \, s^{-1}]$<br>
$e_s$ Saturation vapour pressure $[kPa]$<br>
$e_a$ Actual vapour pressure $[kPa]$<br>
$\gamma$ Psychrometric constant	$[kPa \, K^{-1}]$<br>
$r_a$ Atmospheric resistance $[s\, m^{-1}]$<br>
$r_s$ Surface resistance $[s\, m^{-1}]$<br>

Crop-specific potential evapotranspiration is calculated using also crop-specific factors ($k_c$) during the crop’s growth period and a factor for bare soil between harvest and emergence of the subsequent crop. The $k_c$ factors are coupled to the crop’s developmental stages.

$$ET_p = ET_0 \cdot k_c - I$$

$ET_p$ Potential evapotranspiration	$[mm]$<br>
$ET_0$ Reference evapotranspiration	$[mm]$<br>
$k_c$ Crop-specific factor<br>
$I$	Evaporation from interception storage $[mm]$<br>

To what extent evaporation contributes to total evapotranspiration is determined by the ground coverage.

$$E_p = ET_p \cdot (1-\beta)$$

$E_p$ Potential evaporation	$[mm]$<br>
$ET_p$ Potential evapotranspiration	$[mm]$<br>
$\beta$	Ground coverage<br>

Evaporation is limited by water availability and the soil-air vapour pressure gradient. The reduction factor $\varepsilon_1(z)$, which describes the limitation of evaporation by water availability, is determined by an empirical HERMES function (Figure 1), for which the relative water availability is calculated as:

$$W_{ERel}(z) =  \begin{cases} 0 & \theta<0.33 \cdot \theta_{PWP}  \\   \frac{\theta - (0.33 \cdot \theta_{PWP})} {\theta_{FC} - (0.33 \cdot \theta_{PWP})} & \theta \geq 0.33 \cdot \theta_{PWP} \end{cases}$$

$W_{ERel}(z)$ Relative water availability for evaporation in soil layer $z$<br>
$\theta$ Water content in soil layer $z$ $[m^3 \, m^{-3}]$<br>
$\theta{PWP}$ Water content at permanent wilting point in soil layer $z$ $[m^3 \, m^{-3}]$<br>
$\theta{FC}$ Water content at field capacity in soil layer $z$ $[m^3 \, m^{-3}]$<br>

![](../../images/model_science/soil_processes/evaporation_fig1.png){ width="50%"}

*Figure 1: The reduction factor for water availability for evaporation $\varepsilon_2(z)$ in relation to the relative water availability for evaporation (Kersebaum, 1989).*

The limitation of evaporation by the soil-air vapour pressure gradient is expressed in two further reduction factors. The first of these &ndash; referred to as the deprivation factor $\varepsilon_2(z)$ &ndash; considers the depth of the soil layer from which the water volume evaporates and thus, although implicitly, the slope of the vapour pressure gradient in the air-filled soil volume.

$$\varepsilon_2(z) = (a_1 - a_2) \cdot \frac{\zeta + 1} {log(\zeta + 1) - \zeta}$$

$\varepsilon_2(z)$	Reduction factor deprivation depth<br>
$\zeta$	Curvature of the deprivation function<br>

where

$$a_1 = log \left( \frac { \frac{Z_{E \,max}}{\Delta Z} + \zeta \cdot Z}  {\frac{Z_{E \,max}}{\Delta Z} + \zeta \cdot (z-1) }\right)$$

$\zeta$	Curvature of the deprivation function<br>
$Z_{E \, max}$ Maximum impact depth of evaporation $[m]$<br>
$z$	Layer number<br>
$\Delta z$ Layer depth $[m]$<br>

and

$$a_2 = \frac{\zeta} {\frac{Z_{E \, max}}{\Delta z} \cdot (\zeta + 1)}$$

$\zeta$	Curvature of the deprivation function<br>
$Z_{E \, max}$ Maximum impact depth of evaporation $[m]$<br>
$\Delta z$ Layer depth $[m]$<br>

The second factor expresses is a simple way that almost no water vapour evaporates upwards from a soil layer if the adjacent layer above contains more water than the other because of a reverse vapour pressure gradient.

$$\varepsilon_3(z)=\begin{cases}0.1 & \theta_z \leq \theta_{z-1} \\1.0 & \theta_z > \theta_{z-1}\end{cases}$$

$\varepsilon_3(z)$ Reduction factor vapour pressure gradient in soil layer $z$<br>
$\theta$ Water content in soil layer $z$ $[m^3 \, m^{-3}]$<br>
$\theta_{z-1}$ Water content in soil layer $z–1$ $[m^3 \, m^{-3}]$<br>

The reduction factors for evaporation simply merge to:

$$\varepsilon_z = \varepsilon_1(z) \cdot \varepsilon_2(z) \cdot \varepsilon_3(z)$$

$\varepsilon_z$	Reduction factor for evaporation in soil layer $z$<br>
$\varepsilon_1(z)$ Reduction factor water availability in soil layer $z$<br> 
$\varepsilon_2(z)$ Reduction factor deprivation depth<br>
$\varepsilon_3(z)$ Reduction factor vapour pressure gradient in soil layer $z$<br>

If water is ponding at the soil surface, evaporation is only fed from this pond, assuming a 10% higher evaporation than from moist soil. If snow covers the soil, evaporation is set to zero. Sublimation of snow crystals is not explicitly modelled. However, the snow module considers this process in the snow ageing process.

The total actual evaporation adds up from the contribution of the affected layers:

$$E_a = \sum_{z=1}^{z_{max}} E_p \cdot \varepsilon_z$$

$E_a$ Actual evaporation $[mm]$<br>
$E_p$ Potential evaporation	$[mm]$<br>
$\varepsilon_z$	Reduction factor for evaporation in soil layer $z$<br>
$z$	Layer number<br>
$z_{Emax}$ Lowest soil layer affected by evaporation<br>