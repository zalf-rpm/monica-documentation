# Drought stress

Crop growth is limited by four different stress factors: drought, nitrogen deficiency, oxygen deficiency and heat stress.

Drought is indicated when potential transpiration increases beyond the amount stored in the soil. The reduction factor for drought stress is calculated by the relation of actual to potential transpiration:

$$\zeta_W = \frac{T_a}{T_p}$$

$\zeta_W$ Reduction factor drought stress<br>
$T_a$ Actual transpiration $[mm]$<br>
$T_p$ Potential transpiration $[mm]$<br>

Drought stress in the model affects the supply of assimilates produced from photosynthesis if the reduction factor drops below a threshold value which is specific for each crop and its developmental stages.

$$A_{gm} = A_g \cdot \zeta_w \,\,\,\,\,\,\,\,\,\,\,\,  \zeta_w < \bar{\zeta}_w$$

$A_{gm}$ Amount of assimilates $[kg \, CO_2 \, ha^{-1} \, d^{-1}]$<br>
$A_g$ Gross CO<sub>2</sub> assimilation	$[kg \, CO_2 \, ha^{-1} \, d^{-1}]$<br>
$\zeta_w$ Reduction factor drought stress<br>
$\bar{\zeta}_w$	Crop- and stage-specific threshold for drought stress<br>

Some crops react very sensitively to drought stress during bloom. Within a defined sensitive phase, drought reduces the fertility of the ovules.

---

#### References

* Challinor et al. (2005): Simulation of the impact of high temperature stress on annual crop yields. Agricultural and Forest Meteorology 135, 180 - 189.

* Mirschel, W. & Wenkel, K.-O., 2007. Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel and M. Wegehenkel (Editors), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, pp. 59- 74.

* Moriondo et al. (2011): Climate change impact assessment: the role of climate extremes in crop yield simulation. Climatic Change 104 (3-4), 679-701.