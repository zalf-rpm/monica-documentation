# Respiration

Maintenance respiration is calculated for the photo period and for darkness using AGROSIM algorithms (Mirschel und Wenkel, 2007):

$` \small M_{photo} = \sum (W_i \cdot m_i) \cdot 2^{a\cdot(T_{photo} - b)} \cdot (2-L_N)`$

$` \small M_{dark} = \sum (W_i \cdot m_i) \cdot 2^{a\cdot(T_{dark} - b)} \cdot L_N`$

$` \small M_{photo} `$	Maintenance respiration during the photo period	$` \small [Pa] `$<br>
$` \small M_{dark}`$	Maintenance respiration in darkness	$` \small [Pa] `$<br>
$` \small W_i `$	Dry mass of organ i	$` \small [kg \, m^{-2}] `$<br>
$` \small m_i `$	Specific maintenance respiration of organ i	$` \small [kg \, m^{-2}] `$<br>
$` \small T_{photo} `$	Mean temperature during the photo period	$` \small [^{\circ} C] `$<br>
$` \small T_{dark} `$	Mean temperature during the dark period	$` \small [^{\circ} C] `$<br>
$` \small a, b `$	Parameters	 <br>
$` \small L_N`$	Normalised day length	$` \small [d] `$<br>

where

$` \small L_N = 2 - \left ( \frac{L_p}{12} \right ) `$

$` \small L_N `$	Normalised day length	$` \small [d] `$<br>
$` \small L_P`$	Photoactive day length	$` \small [d] `$<br>

und

$` \small T_{photo} = T_{max} - \left ( \frac{T_{max} - T_{min}} {4}  \right ) `$

$` \small T_{dark} = T_{min} + \left ( \frac{T_{max} - T_{min}} {4}  \right ) `$

$` \small T_{photo}`$	Mean temperature during the photo period	$` \small [^{\circ} C] `$<br>
$` \small T_{max}`$	Daily maximum air temperature	$` \small [^{\circ} C] `$<br>
$` \small T_{min}`$	Daily minimum air temperature	$` \small [^{\circ} C] `$<br>

Growth respiration is calculated accordingly.

Maintenance respiration is served with priority using assimilates produced by photosynthesis; growth respiration is second in line. Assimilates are assigned to the crop organs according to a partitioning matrix which is organised by the crop’s developmental stages. The crops development in turn is calculated from the sum of degree days and, if applicable, modified for each stage by day length and vernalisation requirement.

## Reference
Mirschel, W. und K.-O. Wenkel (2007): Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel und M. Wegehenkel (Hrsg.), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, 59-74.