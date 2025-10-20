# Ontogenesis

The plant development is simulated using the principle of heat summation. The effective temperature is limited by a minimum temperature, which is referred to as base temperature. Suitable soil moisture is required for the emergence of the seeds, which is at least 30% of the available water. Ponding at the soil surface would hinder seed emergence. If soil moisture is ideal, the topsoil layer’s temperature will be used for heat summation.

$$DD_{0, t} = DD_{0,t-1} + (T_{S10} - T_{B0}) \cdot \Delta t$$

$DD_{0, t}$	Actual temperature sum in developmental stage 0	$[^{\circ} C \, d]$<br>
$DD_{0, t-1}$ Yesterday’s temperature sum in developmental stage 0 $[^{\circ} C \, d]$<br>
$T_{S10}$ Soil temperature in 0–10 cm depth	$[^{\circ} C]$<br>
$T_{B0}$ Base temperature developmental stage 0	$[^{\circ} C]$<br>
$\Delta t$ Time step $[d]$<br>

As soon as the crop-specific temperature sum for seed emergence is reached, the following developmental stage is initiated. From this time on the daily mean temperature will be summed up. Stress factors drought and N deficiency accelerate the summation, while vernalisation and day length factors decelerate it:

$$DD_{n,t} =  DD_{n,t-1} + (T_{av} - T_{Bn}) \cdot b_s \cdot b_v \cdot b_D \cdot \Delta t$$

$DD_{n,t}$ Actual temperature sum in developmental stage $n$ $[^{\circ} C \, d]$<br>
$DD_{n,t-1}$ Yesterday’s temperature sum in developmental stage $n$	$[^{\circ} C \, d]$<br>
$T_{av}$ Daily mean air temperature in 2 m height $[^{\circ} C]$<br>
$T_{Bn}$ Base temperature developmental stage n	$[^{\circ} C]$<br>
$b_s$ Acceleration factor environmental stress<br>
$b_v$ Vernalisation factor<br>
$b_D$ Day length factor<br>
$\Delta t$ Time step $[d]$<br>

where

$$b_s = max( 1 + ( 1 - \zeta_W)^2, 1 + (1-\zeta_N)^2  )$$

$b_s$ Acceleration factor environmental stress<br>
$\zeta_w$ Stress factor drought<br>
$\zeta_N$ Stress factor N deficiency<br>

Satisfaction of the crop’s vernalisation requirement is considered as follows:

$$b_V = \begin{cases}  \frac{(d_V - d_{VT})}{(d_{VR} - d_{VT})} & d_{VT} \geq 1 \\ 1 & d_{VT}<1  \end{cases}$$

![](../../images/model_science/crop_processes/ontogenesis_fig1.png){ width="50%"}

*Figure 1: Effective vernalisation in relation to the daily mean air temperature.*

$b_V$ Vernalisation factor<br>
$d_V$ Current number of vernalisation days $[d]$<br>
$d_{VT}$ Vernalisation threshold $[^{\circ} C \, d]$<br>
$d_{VR}$ Crop-specific vernalisation requirement $[^{\circ} C \, d]$<br>

where

$$d_V = d_{V-1} + b_{V_{eff}} \cdot \Delta t$$

$d_V$ Current number of vernalisation days $[d]$<br>
$d_{V-1}$ Number of vernalisation days until yesterday $[d]$<br>
$b_{V_{eff}}$ Effective vernalisation<br>
$\Delta t$ Time step $[d]$<br>

and

$$d_{VT} = min( d_{VR}, 9 ) - 1$$

$d_{VT}$ Vernalisation threshold $[^{\circ} C \, d]$<br>
$d_{VR}$ Crop-specific vernalisation requirement $[^{\circ} C \, d]$<br>

The vernalisation factor is always positive.

Day length is considered in relation to a crop-specific base day length and to the crop’s day length requirement:

$$b_D = \frac{N_{photo} - N_{basis}}{N_{req} - N_{basis}}$$

$b_D$ Day length factor<br>
$N_{photo}$	Photoperiodic day length $[h]$<br>
$N_{basis}$	Crop-specific base day length $[h]$<br>
$N_{req}$ Crop-specific day length requirement $[h]$<br>