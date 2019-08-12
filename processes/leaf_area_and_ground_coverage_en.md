# Leaf area and ground coverage

The daily change in the crop’s leaf area index is calculated in MONICA as follows
 
$`\small \frac{d}{dt} LAI = \frac{d}{dt} W_{L+} \cdot \left( \Phi_{L_s} + \frac{DD_{ads}}{DD_{crops}}  \cdot (\Phi_{L_e} - \Phi_{L_s}) \cdot dt \right)  -\frac{d}{dt}W_{L-} \cdot \Phi_{L_0} \cdot dt `$

$`\small LAI `$	Leaf area index	$`\small [m^2 \, m^{-2}] `$<br>
$`\small W_{L+}`$	Leaf biomass increment	$`\small [kg \, TM \, m^{-2} \, d^{-1}] `$<br>
$`\small W_{L-}`$	Leaf biomass decrement	$`\small [kg \, TM \, m^{-2} \, d^{-1}] `$<br>
$`\small \Phi_{L_s}`$	Specific leaf area at begin of stage	$`\small [m^{2} \, kg \, TM^{-1}] `$<br>
$`\small \Phi_{L_e}`$	Specific leaf area at end of stage	$`\small [m^{2} \, kg \, TM^{-1}] `$<br>
$`\small \Phi_{L_0}`$	Initial specific leaf area	$`\small [m^{2} \, kg \, TM^{-1}] `$<br>
$`\small DD_{acts} `$	Actual temperature sum of developmental stage	$`\small [^{\circ}C \, d] `$<br>
$`\small DD_{crops}`$	Plant-specific temperature sum in developmental stage	$`\small [^{\circ}C \, d] `$<br>
$`\small t `$	Time	$`\small [d] `$<br>
 
Subsequently, the ground coverage is calculated from leaf area index.

$`\small \beta = 1 - e^{-0.5 \cdot LAI}`$

$`\small \beta `$	Degree of ground coverage	$`\small [m^{2} \, m^{-2}] `$<br>
$`\small LAI`$	Leaf area index<br>