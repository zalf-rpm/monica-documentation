# Ontogenesis

The plant development is simulated using the principle of heat summation. The effective temperature is limited by a minimum temperature, which is referred to as base temperature. Suitable soil moisture is required for the emergence of the seeds, which is at least 30% of the available water. Ponding at the soil surface would hinder seed emergence. If soil moisture is ideal, the top soil layer’s temperature will be used for heat summation.

$`\small DD_{0, t} = DD_{0,t-1} + (T_{S10} - T_{B0}) \cdot \Delta t `$

$`\small DD_{0, t}`$	Actual temperature sum in developmental stage 0	$`\small [^{\circ} C \, d] `$<br>
$`\small DD_{0, t-1}`$	Yesterday’s temperature sum in developmental stage 0	$`\small [^{\circ} C \, d] `$<br>
$`\small T_{S10}`$	Soil temperature in 0–10 cm depth	$`\small [^{\circ} C] `$<br>
$`\small T_{B0} `$	Base temperature developmental stage 0	$`\small [^{\circ} C] `$<br>
$`\small \Delta t`$	Time step	$`\small [d] `$<br>

As soon as the crop-specific temperature sum for seed emergence is reached, the following developmental stage is initiated. From this time on the daily mean temperature will be summed up. Stress factors drought and N deficiency accelerate the summation, while vernalisation and day length factors decelerate it.

$`\small DD_{n,t} =  DD_{n,t-1} + (T_{av} - T_{Bn}) \cdot b_s \cdot b_v \cdot b_D \cdot \Delta t `$

$`\small DD_{n,t}`$	Actual temperature sum in developmental stage n	$`\small [^{\circ} C \, d] `$<br>
$`\small DD_{n,t-1}`$	Yesterday’s temperature sum in developmental stage n	$`\small [^{\circ} C \, d] `$<br>
$`\small T_{av}`$	Daily mean air temperature in 2m height	$`\small [^{\circ} C] `$<br>
$`\small T_{Bn}`$	Base temperature developmental stage n	$`\small [^{\circ} C] `$<br>
$`\small b_s`$	Acceleration factor environmental stress<br>
$`\small b_v`$	Vernalisation factor<br>
$`\small b_D`$	Day length factor<br>
$`\small \Delta t`$	Time step	$`\small [d] `$<br>

where

$`\small b_s = max( 1 + ( 1 - \zeta_W)^2, 1 + (1-\zeta_N)^2  ) `$

$`\small b_s `$	Acceleration factor environmental stress<br>
$`\small \zeta_w`$	Stress factor drought<br>
$`\small \zeta_N`$	Stress factor N deficiency<br>

Satisfaction of the crop’s vernalisation requirement is considered as follows:

$`\small b_V = \begin{cases}  \frac{(d_V - d_{VT})}{(d_{VR} - d_{VT})} & d_{VT} \geq 1 \\ 1 & d_{VT}<1  \end{cases}`$

![](../images/crop_processes/monica_ontogenesis_fig.1.png)<br>
Figure 1: Effective vernalisation in relation to the daily mean air temperature.

$`\small b_V`$	Vernalisation factor<br>
$`\small d_V`$	Current number of vernalisation days	$`\small [d] `$<br>
$`\small d_{VT}`$	Vernalisation threshold	$`\small [^{\circ} C \, d] `$<br>
$`\small d_{VR}`$	Crop-specific vernalisation requirement	$`\small [^{\circ} C \, d] `$<br>

where

$`\small d_V = d_{V-1} + b_{V_{eff}} \cdot \Delta t`$

$`\small d_V`$	Current number of vernalisation days	$`\small [d] `$<br>
$`\small d_{V-1}`$	Number of vernalisation days until yesterday	$`\small [d] `$<br>
$`\small b_{V_{eff}}`$	Effective vernalisation<br>
$`\small \Delta t`$	Time step	$`\small [d] `$<br>

and

$`\small d_{VT} = min( d_{VR}, 9 ) - 1`$

$`\small d_{VT} `$	Vernalisation threshold	$`\small [^{\circ} C \, d] `$<br>
$`\small d_{VR}`$	Crop-specific vernalisation requirement	$`\small [^{\circ} C \, d] `$<br>

The vernalisation factor is always positive.

Day length is considered in relation to a crop-specific base day length and to the crop’s day length requirement

$`\small b_D = \frac{N_{photo} - N_{basis}}{N_{req} - N_{basis}}`$

$`\small b_D`$	Day length factor<br>
$`\small N_{photo}`$	Photoperiodic day length	$`\small [h] `$<br>
$`\small N_{basis}`$	Crop-specific base day length	$`\small [h] `$<br>
$`\small N_{req}`$	Crop-specific day length requirement	$`\small [h] `$<br>
 