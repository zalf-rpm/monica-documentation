# Heat stress

Heat stress affects the production of ovules during bloom. Heat in the sense of the model is defined as exceeding a plant-specific threshold during the daylight period (photo period). For this period the temperature is assumed as being

$`\small T_{photo} = T_{max} - \frac{T_{max}-T_{min}}{4} `$

$`\small T_{photo} `$	Temperature during the photo period	$`\small [^{\circ}C] `$<br>
$`\small T_{max}`$	Daily maximum temperature	$`\small [^{\circ}C] `$<br>
$`\small T_{min}`$	Daily minimum temperature	$`\small [^{\circ}C] `$<br>

During a defined plant-specific phase of increased sensitivity extreme temperatures affect the crop according to Challinor et al. (2005)

$`\small F_H = 1 - \left( \frac{T_{photo} - T_{critH}}{T_{limH} - T_{critH}} \right) \cdot r_F  `$

$`\small F_H`$	Heat impact on ovules	$`\small [d^{-1}] `$<br>
$`\small T_{photo}`$	Temperature during the photo period	$`\small [^{\circ}C] `$<br>
$`\small T_{critH} `$	Critical temperature for heat stress effect	$`\small [^{\circ}C] `$<br>
$`\small T_{limH} `$	Maximum temperature for heat stress effect	$`\small [^{\circ}C] `$<br>
$`\small r_F `$	Daily flower emergence rate	$`\small [d^{-1}] `$<br>

The daily flower emergence rate during bloom is calculated following an idea of Moriondo et al. (2011).

$`\small r_F = p_{F,d} - p_{F, d-1} `$

$`\small r_F`$	Daily flower emergence rate	$`\small [d^{-1}] `$<br>
$`\small p_{F, d}`$	Fraction of today opened flowers	 <br>
$`\small p_{F, d-1} `$	Fraction of yesterdays opened flowers	 <br>

where

$`\small p_F =  \frac{1} { 1 + (\frac{1}{0.015} - 1) \cdot e^{-1.4 \cdot D_{BF}} } `$

$`\small p_F `$	Fraction of open flowers	 <br>
$`\small D_{BF}`$	Days after begin of bloom	$`\small [d] `$<br>

The reduction factor is derived from the smallest value of FH during the sensitive phase.

$`\small \zeta_H = min(F_{H1},\, ... \,,F_{Hn} ) `$

$`\small \zeta_H `$	Reduction factor heat stress	 <br>
$`\small F_{H1}`$	Heat impact at the first day of the sensitive phase	$`\small [d^{-1}] `$<br>
$`\small F_{Hn}`$	Heat impact at the last day of the sensitive phase	$`\small [d^{-1}] `$<br>

Heat stress reduces the assimilate flow to the storage organ.

$`\small W_s = A_g \cdot a_s \cdot \zeta_H`$

$`\small W_s`$	Biomass of the storage organ	$`\small [kg \, TM \, ha^{-1}] `$<br>
$`\small A_g`$	Gross CO2 assimilation	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small a_s`$	Assimilate partitioning coefficient for the storage organ	 <br>
$`\zeta_H`$	Reduction factor heat stress	<br>

## References
Challinor et al. (2005): Simulation of the impact of high temperature stress on annual crop yields. Agricultural and Forest Meteorology 135, 180 - 189.

Mirschel, W. & Wenkel, K.-O., 2007. Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel and M. Wegehenkel (Editors), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, pp. 59- 74.

Moriondo et al. (2011): Climate change impact assessment: the role of climate extremes in crop yield simulation. Climatic Change 104 (3-4), 679-701.