# Heat stress

Heat stress affects the production of ovules during bloom. Heat, in the sense of the model, is defined as exceeding a plant-specific threshold during the daylight period (photo period). For this period, the temperature is assumed as being:

$$T_{photo} = T_{max} - \frac{T_{max}-T_{min}}{4}$$

$T_{photo}$	Temperature during the photo period	$[^{\circ}C]$<br>
$T_{max}$ Daily maximum temperature	$[^{\circ}C]$<br>
$T_{min}$ Daily minimum temperature	$[^{\circ}C]$<br>

During a defined plant-specific phase of increased sensitivity, extreme temperatures affect the crop according to Challinor et al. (2005):

$$F_H = 1 - \left( \frac{T_{photo} - T_{critH}}{T_{limH} - T_{critH}} \right) \cdot r_F$$

$F_H$ Heat impact on ovules	$[d^{-1}]$<br>
$T_{photo}$	Temperature during the photo period	$[^{\circ}C]$<br>
$T_{critH}$	Critical temperature for heat stress effect	$[^{\circ}C]$<br>
$T_{limH}$ Maximum temperature for heat stress effect $[^{\circ}C]$<br>
$r_F$ Daily flower emergence rate $[d^{-1}]$<br>

The daily flower emergence rate during bloom is calculated following an idea of Moriondo et al. (2011):

$$r_F = p_{F,d} - p_{F, d-1}$$

$r_F$ Daily flower emergence rate $[d^{-1}]$<br>
$p_{F, d}$ Fraction of today opened flowers<br>
$p_{F, d-1}$ Fraction of yesterday opened flowers<br>

where

$$p_F =  \frac{1} { 1 + (\frac{1}{0.015} - 1) \cdot e^{-1.4 \cdot D_{BF}} }$$

$p_F$ Fraction of open flowers<br>
$D_{BF}$ Days after begin of bloom $[d]$<br>

The reduction factor is derived from the smallest value of $F_H$ during the sensitive phase:

$$\zeta_H = min(F_{H1},\, ... \,,F_{Hn} )$$

$\zeta_H$ Reduction factor heat stress<br>
$F_{H1}$ Heat impact at the first day of the sensitive phase $[d^{-1}]$<br>
$F_{Hn}$ Heat impact at the last day of the sensitive phase	$[d^{-1}]$<br>

Heat stress reduces the assimilate flow to the storage organ:

$$W_s = A_g \cdot a_s \cdot \zeta_H$$

$W_s$ Biomass of the storage organ $[kg \, TM \, ha^{-1}]$<br>
$A_g$ Gross CO<sub>2</sub> assimilation	$[kg \, CO_2 \, ha^{-1} \, d^{-1}]$<br>
$a_s$ Assimilate partitioning coefficient for the storage organ<br>
$\zeta_H$ Reduction factor heat stress <br>

---

#### References

* Challinor et al. (2005): Simulation of the impact of high temperature stress on annual crop yields. Agricultural and Forest Meteorology 135, 180 - 189.

* Mirschel, W. & Wenkel, K.-O., 2007. Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel and M. Wegehenkel (Editors), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, pp. 59- 74.

* Moriondo et al. (2011): Climate change impact assessment: the role of climate extremes in crop yield simulation. Climatic Change 104 (3-4), 679-701.