# Leaf area and ground coverage

The daily change in the crop’s leaf area index is calculated in MONICA as follows:
 
$$\frac{d}{dt} LAI = \frac{d}{dt} W_{L+} \cdot \left( \Phi_{L_s} + \frac{DD_{ads}}{DD_{crops}}  \cdot (\Phi_{L_e} - \Phi_{L_s}) \cdot dt \right)  -\frac{d}{dt}W_{L-} \cdot \Phi_{L_0} \cdot dt$$

$LAI$ Leaf area index $[m^2 \, m^{-2}]$<br>
$W_{L+}$ Leaf biomass increment	$[kg \, TM \, m^{-2} \, d^{-1}]$<br>
$W_{L-}$ Leaf biomass decrement	$[kg \, TM \, m^{-2} \, d^{-1}]$<br>
$\Phi_{L_s}$ Specific leaf area at begin of stage $[m^{2} \, kg \, TM^{-1}]$<br>
$\Phi_{L_e}$ Specific leaf area at end of stage	$[m^{2} \, kg \, TM^{-1}]$<br>
$\Phi_{L_0}$ Initial specific leaf area	$[m^{2} \, kg \, TM^{-1}]$<br>
$DD_{acts}$	Actual temperature sum of developmental stage $[^{\circ}C \, d]$<br>
$DD_{crops}$ Plant-specific temperature sum in developmental stage $[^{\circ}C \, d]$<br>
$t$	Time $[d]$<br>
 
Subsequently, the ground coverage is calculated from leaf area index:

$\beta = 1 - e^{-0.5 \cdot LAI}$

$\beta$ Degree of ground coverage $[m^{2} \, m^{-2}]$<br>
$LAI$ Leaf area index<br>