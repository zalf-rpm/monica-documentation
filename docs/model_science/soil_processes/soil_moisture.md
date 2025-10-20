# Soil moisture

A capacity approach was used to describe soil water dynamics (Wegehenkel, 2000). The capacity parameters are derived from the soil texture and modified by soil organic matter content and bulk density. Water contents at saturation, field capacity, and permanent wilting point for different bulk densities and correction values for different soil organic matter classes (Ad-hoc-AG Boden, 2005) are provided by Wessolek et al. (2009) and stored in the database.

If a crop is present, precipitation is partly intercepted and evaporates from the crop surface. Interception $I$ is calculated as:

$$I = (2.5 \cdot h_c \cdot \beta) - S_i$$

$I$	Interception $[mm]$<br>
$h_c$ Crop height $[m]$<br>
$\beta$	Canopy closure $[m^2\,m^{-2}]$<br>
$S_i$ Interception storage $[mm]$<br>

The remainder falls on the ground and is stored in a surface pond, from which water infiltrates into the soil. As long as the surface pond contains water, it is the only source of evaporation. The percolation of water volumes above field capacity is governed by an empirical, texture-dependent rate coefficient ($\lambda$):

$$\lambda = 1.15 \cdot f^2_s + 0.1 \cdot f_c + 0.35 \cdot f_u$$

$\lambda$ Empirical percolation rate coefficient<br>
$f_s$ Soil sand content	$[kg \, kg^{-1}]$<br>
$f_c$ Soil clay content	$[kg \, kg^{-1}]$<br>
$f_u$ Soil silt content	$[kg \, kg^{-1}]$<br>

If the groundwater level is located within the simulated soil profile, constant groundwater discharge can be adjusted to allow for the rising and falling groundwater level, depending on the soil water balance. The capillary rise from groundwater is considered according to empirical ascent rates (Ad-hoc-AG Boden, 2005). The groundwater level oscillates between given maximum and minimum levels with a period of one year.

Reference evapotranspiration $ET0$ is calculated using the Penman-Monteith method, according to Allen et al. (1998). This method requires the diurnal minimum and maximum temperature, the water vapour pressure deficit, wind velocity, and total global radiation.

$$ET_0 = \frac{0.408 \cdot \Delta \cdot (R_n - G) + \gamma \cdot \frac{900}{T + 273} \cdot u_2 \cdot (e_s - e_a)} {\Delta + \gamma \cdot (1 + \frac{r_a}{r_s} )}$$

$\Delta$ Slope of the vapour pressure curve	$[kPa \, K^{-1}]$<br>
$R_n$ Net radiation at the crop surface	$[MJ \, m^{-2} \, d^{-1}]$<br>
$G$ Soil heat flux density $[MJ \, m^{-2} \, d^{-1}]$<br>
$T$	Mean daily air temperature at 2 m height $[^{\circ}C]$<br>
$u_2$ Wind speed at 2 m height $[m\,s^{-1}]$<br>
$e_s$ Saturation vapour pressure $[kPa]$<br>
$e_a$ Actual vapour pressure $[kPa]$<br>
$\gamma$ Psychrometric constant	$[kPa\, K^{-1}]$<br>
$r_s$ Atmospheric resistance $[s \, m^{-1}]$<br>
$r_a$ Surface resistance $[s \, m^{-1}]$<br>

where

$$\gamma = 6.65 \cdot 10^{-4} \cdot P$$

$P$	Atmospheric pressure $[Pa]$<br>

The surface resistance for the reference evapotranspiration assumes a 12 cm cut grass crop and is calculated using:

$$r_s = \frac{r_1} {1.44}$$

$r_1$ Stomata resistance; 100 s m-1	$[s \, m^{-1}]$<br>

The surface resistance for the actual crop is calculated in the crop growth module.