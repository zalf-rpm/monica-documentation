# Drought stress
Crop growth is limited by four different stress factors: drought, nitrogen deficiency oxygen deficiency and heat stress.

Drought is indicated when potential transpiration increases beyond the amount stored in the soil. The reduction factor for drought stress is calculated by the relation of actual to potential transpiration.

$`\small \zeta_W = \frac{T_a}{T_p}`$

$`\small \zeta_W`$	Reduction factor drought stress	

$`\small T_a`$	Actual transpiration	$`\small [mm] `$<br>
$`\small T_p`$	Potential transpiration	$`\small [mm] `$<br>

Drought stress in the model affects the supply of assimilates produced from photosynthesis if the reduction factor drops below a threshold value which is specific for each crop and its developmental stages.

$`\small A_{gm} = A_g \cdot \zeta_w \,\,\,\,\,\,\,\,\,\,\,\,  \zeta_w < \bar{\zeta}_w`$

$`\small A_{gm}`$	Amount of assimilates	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small A_g`$	Gross CO2 assimilation	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small \zeta_w`$	Reduction factor drought stress	 <br>
$`\small \bar{\zeta}_w`$	Crop- and stage-specific threshold for drought stress	 <br>

Some crops react very sensitively on drought stress during bloom. Within a defined sensitive phase, drought reduces the fertility of the ovules.

## References
Challinor et al. (2005): Simulation of the impact of high temperature stress on annual crop yields. Agricultural and Forest Meteorology 135, 180 - 189.

Mirschel, W. & Wenkel, K.-O., 2007. Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel and M. Wegehenkel (Editors), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, pp. 59- 74.

Moriondo et al. (2011): Climate change impact assessment: the role of climate extremes in crop yield simulation. Climatic Change 104 (3-4), 679-701.