# Transpiration

Reference evapotranspiration ET0 is calculated using the Penman-Monteith method, according to Allen et al. (1998). This method requires the diurnal minimum and maximum temperature, the water vapour pressure deficit, wind velocity and total global radiation.

$`\small ET_0 = \frac{0.408 \cdot \Delta \cdot (R_n - G) + \gamma \cdot \frac{900}{\gamma + 273} \cdot u_2 \cdot (e_s - e_a) } {\Delta + \gamma \cdot (1+\frac{r_a}{r_s})} `$

$`\small  \Delta`$	Slope of the vapour pressure curve	$`\small  [kPa\,K^{-1}]`$<br>
$`\small  R_n`$	Net radiation at the crop surface	$`\small  [MJ\, m^{-2} \, d^{-1}]`$<br>
$`\small  G`$	Soil heat flux density	$`\small  [MJ\, m^{-2} \, d^{-1}]`$<br>
$`\small  T`$	Mean daily air temperature at 2 m height	$`\small  [^{\circ} C]`$<br>
$`\small  u_2`$	Wind speed at 2 m height	$`\small  [m \, s^{-1}]`$<br>
$`\small  e_s`$	Saturation vapour pressure	$`\small  [kPa]`$<br>
$`\small  e_a`$	Actual vapour pressure	$`\small  [kPa]`$<br>
$`\small  \gamma`$	Psychrometric constant	$`\small  [kPa \, K^{-1}]`$<br>
$`\small  r_a`$	Atmospheric resistance	$`\small  [s\, m^{-1}]`$<br>
$`\small  r_s`$	Surface resistance	$`\small  [s\, m^{-1}]`$<br>
 
where

$`\small \gamma = 6.65 \cdot 10^{-4} \cdot P `$

The surface resistance for the reference evapotranspiration assumes a 12 cm cut grass crop and is calculated using

$`\small r_s = \frac {r_1} {1.44} `$

$`\small r_1`$	Stomata resistance; 100 s m–1	$`\small [s \, m^{-1}] `$

The leaf area index LAI and crop height h are considered for the surface resistance of the actual crop.

$`\small r_s = \frac{r_1}{LAI \cdot h_c} `$

$`\small r_s `$	Surface resistance	$`\small [s \, m^{-1}] `$<br>
$`\small r_1 `$	Stomata resistance	$`\small [s \, m^{-1}] `$<br>
$`\small LAI `$	Leaf area index	$`\small [m^2 \, m^{-2}] `$<br>
$`\small h_c`$	Crop height	$`\small [m] `$<br>
 
The crop height is calculated using

$`\small H_c = \frac{h_{c\,max}}{1+ e^{-a \cdot (DD_{relh}-b)}} `$

$`\small h_{c\,max}`$	crop-specific maximum height	$`\small [m] `$<br>
$`\small a, b `$	crop-specific parameter<br>
$`\small DD_{relh} `$	relative crop development for height<br>
 
where

$`\small DD_{relh} = \frac{DD_{acth}} {DD_{croph}}`$

$`\small DD_{acth} `$	actual temperature sum from emergence	$`\small [^{\circ}C \, d] `$<br>
$`\small DD_{croph} `$	crop-specific total temperature sum up to maximum height	$`\small [^{\circ}C \, d] `$<br>
 
Specific leaf weights to calculate LAI from the leaf biomass are given for each development stage and adjusted linearly to the relative phenological development. For all other variables in Equation (?), the reader is referred to Allen et al. (1998).

The stomata resistance of the actual crop is calculated according to a suggestion of Yu et al. (2001):

$`\small r_1  = \frac{C_s(1+\frac{e_a}{e_s}) } {a \cdot A_g} `$

$`\small r_1`$	Stomata resistance	$`\small [s^{-1}] `$<br>
$`\small C_s`$	CO2 concentration outside the leaf (= Ca)	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small A_g`$	Gross CO2 assimilation rate	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small e_a`$	Actual air vapour pressure	$`\small [Pa] `$<br>
$`\small e_s`$	Saturation air vapour pressure	$`\small [Pa] `$<br>
 
In a rough simplification, Cs is considered equal to the atmospheric CO2 concentration Ca. Its seasonal dynamic from 1958 up until today is described using

$`\small C_a = 222 + e^{ 0.0119 \cdot (t_{dec} - 1580) } + 2.5 \cdot \left(   \frac{t_{dec} - 0.5} {0.1592} \right) `$

$`\small C_a `$	Atmospheric CO2 concentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small t_{dec}`$	Decimal date<br>
 
Additionally, this function carries forward the atmospheric CO2 concentration until 2100, similar to the assumption made in the IPCC A1B scenario (IPCC 2007).

Crop-specific potential evapotranspiration is calculated using also crop-specific factors (Kc) during the crop’s growth period and a factor for bare soil between harvest and emergence of the subsequent crop. The Kc factors are coupled to the crop’s developmental stages.

$`\small ET_p = ET_0 \cdot K_c - I `$

$`\small ET_p`$	Potential evapotranspiration	$`\small [mm] `$<br>
$`\small ET_0`$	Reference evapotranspiration	$`\small [mm] `$<br>
$`\small K_c`$	crop-specific factor<br>
$`\small I`$	Evaporation from interception storage	$`\small [mm] `$<br>
 
To what extent transpiration contributes to total evapotranspiration is determined by the ground coverage.

$`\small T_p = ET_p \cdot \beta `$

$`\small T_p `$	Potential transpiration	$`\small [mm] `$<br>
$`\small ET_p`$	Potential evapotranspiration	$`\small [mm] `$<br>
$`\small \beta`$	Ground coverage<br>
 
Transpiration is calculated layer-wise for water uptake from the respective layer, considering root distribution and efficiency and possible oxygen deficit.

$`\small T_z = T_p \cdot \frac {\omega_z \cdot \Lambda_z}   {\sum^{z_{max}}_{i=1} \omega_i \cdot \Lambda_j} \cdot \zeta_o`$

$`\small T_z`$	Actual transpiration in layer z	$`\small [mm] `$<br>
$`\small T_p`$	Potential transpiration	$`\small [mm] `$<br>
$`\small \omega_z`$	Root water uptake efficiency in layer z (Fig. 1)<br>
$`\small \Lambda_z`$	Root length density in layer z	$`\small [m \, m^{-3}] `$<br>
$`\small \zeta_o`$	Stress factor oxygen deficit<br>
 
![](MONICA_Transpiration_Fig_1.png)
Figure 1: Reduction function for transpiration ($`\tau_z`$) and for root water uptake efficiency ($`\omega_z`$) in dependency of water availability in the respective soil layer.

Given sufficient amounts of available water, it is removed from the soil layers according to the layer-wise calculated transpiration, from the soil surface downwards. A potential transpiration deficit is calculated for every layer.

$`\small \zeta_{Wpot} = \left(  \frac{T_z}{d\cdot 1000} - (\theta - \theta_{PWP} ) \right) \cdot \Delta z \cdot 1000 `$

$`\small \zeta_{Wpot}`$	Potential transpiration deficit in layer z	$`\small [mm] `$<br>
$`\small T_z`$	Actual transpiration in layer z	$`\small [mm] `$<br>
$`\small \Delta z`$	Depth of soil layer	$`\small [m] `$<br>
$`\small \theta `$	Water content in soil layer	$`\small [m^3\, m^{-3}] `$<br>
$`\small \theta_{PWP}`$	Soil water content at permanent wilting point	$`\small [m^3\, m^{-3}] `$<br>
 
At decreasing water contents water conductivity in the soil is reduced. The resulting reduction of transpiration in the soil layer is calculate as

$`\small T_{red} = T_z \cdot (1-\tau_z)`$

$`\small T_{red} `$	Reduced transpiration in layer z	$`\small [mm] `$<br>
$`\small T_z`$	Actual transpiration in layer z	$`\small [mm] `$<br>
$`\small \tau_z`$	Reduction factor water availability (Fig.)<br>
 
The actual transpiration deficit of the respective layer is calculated from the larger of both values. It will be used to update actual transpiration.

$`\small \zeta_{Wact} = max(\zeta_{Wpot}, T_{red})`$

$`\small \zeta_{Wact} `$	Actual transpiration deficit in layer z	$`\small [mm] `$<br>
$`\small \zeta_{Wpot}`$	Potential transpiration deficit in layer z	$`\small [mm] `$<br>
$`\small T_{red} `$	Reduced transpiration in layer z	$`\small [mm] `$<br>
 
The total actual transpiration of the crop adds up from the actual transpiration of the layers.

$`\small T_a = \sum^{z_{max}}_{z=1} T_z`$

$`\small T_a `$	Actual transpiration	$`\small [mm] `$<br>
$`\small T_z`$	Actual transpiration in layer z	$`\small [mm] `$<br>
$`\small z `$	Layer number<br>
$`\small z_{max}`$	Lowest layer in soil profile<br>
 
Crop growth is limited by water availability. Drought stress is indicated by the relation of actual to potential transpiration (Kersebaum, 1995).