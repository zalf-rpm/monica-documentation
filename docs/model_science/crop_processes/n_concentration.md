# N concentration

To determine the effect of insufficient N supply to the crop, the concept of critical N concentration is applied.

$$N_{crit} = a \cdot (1.0 + b \cdot e^{-0.26 \cdot W})$$

$N_{crit}$ Critical N concentration in the plant tissue	$[kg \, N \, kg DM^{-1}]$<br>
$W$	Above-ground dry matter biomass	$[t]$<br>
$a, b$ Crop-specific parameters<br>

The concept was developed by Greenwood (1990). The optimum N concentration is obtained by multiplying the critical N concentration with a luxury-N supply factor, which is taken from the EU-Rotate_N model (Rahn et al., 2010).

$$N_{target} = N_{crit} \cdot k_{lux}$$

$N_{target}$ Optimum N concentration in the plant tissue $[kg \, N \, kg DM^{-1}]$<br>
$N_{crit}$ Critical N concentration in the plant tissue	$[kg \, N \, kg DM^{-1}]$<br>
$k_{lux}$ Luxury-N supply factor <br>

The N concentration in the plant tissue is calculated for every daily time step. The difference between the actual and the newly calculated optimum N concentration is acquired for uptake from the soil. Soil N supply is calculated under consideration of root architecture and transport parameters (N uptake). In case the supply from soil is sufficient, the difference will be replaced in the crop. Otherwise, the N concentration in the plant tissue will drop. Once it falls below the critical N concentration, photosynthesis is reduced.

---

#### References

* Greenwood, D.J. et al. (1990): Weather, nitrogen supply and growth rate of field vegetables. Plant Soil 124 (2), 297-301.

* Rahn, C.R. et al. (2010): EU-Rotate_N - a European decision support system - to predict environmental and economic consequences of the management of nitrogen fertiliser in crop rotations. European Journal of Horticultural Science 75 (1), 20-32