# Transpiration

Reference evapotranspiration $ET0$ is calculated using the Penman-Monteith method, according to Allen et al. (1998). This method requires the diurnal minimum and maximum temperature, the water vapour pressure deficit, wind velocity, and total global radiation.

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
 
where

$$\gamma = 6.65 \cdot 10^{-4} \cdot P$$

The surface resistance for the reference evapotranspiration assumes a 12 cm cut grass crop and is calculated using:

$$r_s = \frac {r_1} {1.44}$$

$r_1$ Stomata resistance; 100 s m–1	$[s \, m^{-1}]$

The leaf area index $LAI$ and crop height $h$ are considered for the surface resistance of the actual crop:

$$r_s = \frac{r_1}{LAI \cdot h_c}$$

$r_s$ Surface resistance $[s \, m^{-1}]$<br>
$r_1$ Stomata resistance $[s \, m^{-1}]$<br>
$LAI$ Leaf area index $[m^2 \, m^{-2}]$<br>
$h_c$ Crop height $[m]$<br>
 
The crop height is calculated using:

$$H_c = \frac{h_{c\,max}}{1+ e^{-a \cdot (DD_{relh}-b)}}$$

$h_{c\,max}$ Crop-specific maximum height $[m]$<br>
$a, b$ Crop-specific parameter<br>
$DD_{relh}$	Relative crop development for height<br>
 
where

$$DD_{relh} = \frac{DD_{acth}} {DD_{croph}}$$

$DD_{acth}$ Actual temperature sum from emergence $[^{\circ}C \, d]$<br>
$DD_{croph}$ Crop-specific total temperature sum up to maximum height $[^{\circ}C \, d]$<br>
 
Specific leaf weights to calculate $LAI$ from the leaf biomass are given for each development stage and adjusted linearly to the relative phenological development.

---

The stomata resistance of the actual crop is calculated according to a suggestion of Yu et al. (2001):

$$r_1  = \frac{C_s(1+\frac{e_a}{e_s}) } {a \cdot A_g}$$

$r_1$ Stomata resistance $[s^{-1}]$<br>
$C_s$ CO<sub>2</sub> concentration outside the leaf (= $C_a$)	$[\mu mol \, mol^{-1}]$<br>
$A_g$ Gross CO<sub>2</sub> assimilation rate $[kg \, CO_2 \, ha^{-1} \, d^{-1}]$<br>
$e_a$ Actual air vapour pressure $[Pa]$<br>
$e_s$ Saturation air vapour pressure $[Pa]$<br>
 
In a rough simplification, $C_s$ is considered equal to the atmospheric CO<sub>2</sub> concentration $C_a$. Its seasonal dynamic from 1958 up until today is described using:

$$C_a = 222 + e^{ 0.0119 \cdot (t_{dec} - 1580) } + 2.5 \cdot \left(   \frac{t_{dec} - 0.5} {0.1592} \right)$$

$C_a$ Atmospheric CO<sub>2</sub> concentration $[\mu mol \, mol^{-1}]$<br>
$t_{dec}$ Decimal date<br>
 
Additionally, this function carries forward the atmospheric CO<sub>2</sub> concentration until 2100, similar to the assumption made in the IPCC A1B scenario (IPCC, 2007).

---

Crop-specific potential evapotranspiration is calculated using also crop-specific factors ($K_c$) during the crop’s growth period, and a factor for bare soil between harvest and emergence of the subsequent crop. The $K_c$ factors are coupled to the crop’s developmental stages.

$$ET_p = ET_0 \cdot K_c - I$$

$ET_p$ Potential evapotranspiration	$[mm]$<br>
$ET_0$ Reference evapotranspiration	$[mm]$<br>
$K_c$ ncrop-specific factor<br>
$I$	Evaporation from interception storage $[mm]$<br>
 
To what extent transpiration contributes to total evapotranspiration is determined by the ground coverage:

$$T_p = ET_p \cdot \beta$$

$T_p$ Potential transpiration $[mm]$<br>
$ET_p$ Potential evapotranspiration	$[mm]$<br>
$\beta$	Ground coverage<br>

---

Transpiration is calculated layer-wise for water uptake from the respective layer, considering root distribution, efficiency, and possible oxygen deficit.

$$T_z = T_p \cdot \frac {\omega_z \cdot \Lambda_z}   {\sum^{z_{max}}_{i=1} \omega_i \cdot \Lambda_j} \cdot \zeta_o$$

$T_z$ Actual transpiration in layer $z$	$[mm]$<br>
$T_p$ Potential transpiration $[mm]$<br>
$\omega_z$ Root water uptake efficiency in layer $z$ (Fig. 1)<br>
$\Lambda_z$	Root length density in layer $z$ $[m \, m^{-3}]$<br>
$\zeta_o$ Stress factor oxygen deficit<br>
 
![](../../images/model_science/crop_processes/transpiration_fig1.png){ width="50%" }

*Figure 1: Reduction function for transpiration ($\tau_z$) and for root water uptake efficiency ($\omega_z$) in dependency of water availability in the respective soil layer.*

Given sufficient amounts of available water, it is removed from the soil layers according to the layer-wise calculated transpiration, from the soil surface downwards. A potential transpiration deficit is calculated for every layer:

$$\zeta_{Wpot} = \left(  \frac{T_z}{d\cdot 1000} - (\theta - \theta_{PWP} ) \right) \cdot \Delta z \cdot 1000$$

$\zeta_{Wpot}$ Potential transpiration deficit in layer $z$	$[mm]$<br>
$T_z$ Actual transpiration in layer $z$	$[mm]$<br>
$\Delta z$ Depth of soil layer $[m]$<br>
$\theta$ Water content in soil layer $[m^3\, m^{-3}]$<br>
$\theta_{PWP}$ Soil water content at permanent wilting point $[m^3\, m^{-3}]$<br>
 
At decreasing water contents water conductivity in the soil is reduced. The resulting reduction of transpiration in the soil layer is calculated as:

$$T_{red} = T_z \cdot (1-\tau_z)$$

$T_{red}$ Reduced transpiration in layer $z$ $[mm]$<br>
$T_z$ Actual transpiration in layer $z$ $[mm]$<br>
$\tau_z$ Reduction factor water availability (Fig.)<br>
 
The actual transpiration deficit of the respective layer is calculated from the larger of both values. It will be used to update actual transpiration.

$$\zeta_{Wact} = max(\zeta_{Wpot}, T_{red})$$

$\zeta_{Wact}$ Actual transpiration deficit in layer $z$ $[mm]$<br>
$\zeta_{Wpot}$ Potential transpiration deficit in layer $z$	$[mm]$<br>
$T_{red}$ Reduced transpiration in layer $z$ $[mm]$<br>
 
The total actual transpiration of the crop adds up from the actual transpiration of the layers:

$$T_a = \sum^{z_{max}}_{z=1} T_z$$

$T_a$ Actual transpiration $[mm]$<br>
$T_z$ Actual transpiration in layer $z$ $[mm]$<br>
$z$	Layer number<br>
$z_{max}$ Lowest layer in soil profile<br>
 
Crop growth is limited by water availability. Drought stress is indicated by the relation of actual to potential transpiration (Kersebaum, 1995).