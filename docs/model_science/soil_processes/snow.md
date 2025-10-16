# Snow

Snow layers are simulated following the idea of Riley and Bonesmo (2005). Below 1.8°C air temperature, an increasing fraction of snow is assumed in the falling rain, which adds to a snow layer.

$$n_l = \begin{cases}  1 & T_a \geq T_{as}  \\  \frac{T_a - T_{ls}}{T_{as} - T_{ls}} & T_{ls} < T_a < T_{as} \\ 0 & T_a \leq T_{ls}    \end{cases}$$

$n_l$ Fraction of liquid precipitation $[mm \, mm^{-1}]$<br>
$T_a$ Daily mean air temperature $[^{\circ} C]$<br>
$T_{ls}$ Threshold temperature for liquid water in snow	$[^{\circ} C]$<br>
$T_{as}$ Threshold temperature for snow accumulation $[^{\circ} C]$<br>

The amounts of liquid and snow precipitation are derived from:

$$N_l = n_l \cdot k_l$$

$N_l$ Liquid precipitation $[mm]$<br>
$n_l$ Fraction of liquid precipitation $[mm \, mm^{-1}]$<br>
$k_l$ Correction factor liquid precipitation $[mm]$<br>

and

$$N_s = (1-n_l) \cdot k_s$$

$N_s$ Snow precipitation $[mm]$<br>
$n_l$ Fraction of liquid precipitation $[mm \, mm^{-1}]$<br>
$k_s$ Correction factor snow precipitation $[mm]$<br>

The snow layer density is calculated as follows:

$$\rho_{sn} = \rho_{sn\,min} + \rho_{sn\, max} \cdot \frac{T_a - T_{ls}}{T_{as} - T_{ls}}$$

$\rho_{sn}$	Density of fresh snow $[kg \, dm^{-3}]$<br>
$\rho_{sn \, min}$ Minimum density of fresh snow $[kg \, dm^{-3}]$<br>
$\rho_{sn \, max}$ Maximum density of fresh snow $[kg \, dm^{-3}]$<br>
$T_a$ Daily mean air temperature $[^{\circ} C]$<br>
$T_{ls}$ Threshold temperature for liquid water in snow	$[^{\circ} C]$<br>
$T_{as}$ Threshold temperature for snow accumulation	$[^{\circ} C]$<br>

Snow starts thawing above 0.31 °C and increases its density:

$$W_{sm} = \begin{cases}  0 & T_a < T_{sm} \\ a_{sm} \cdot ( T_a - T_{sm}) & T_a \geq T_{sm}      \end{cases}$$

$W_{sm}$ Water from melting snow $[mm]$<br>
$a_{sm}$ Snow ageing (limited to 4.7)<br>
$T_a$ Daily mean air temperature $[^{\circ} C]$<br>
$T_{sm}$ Base temperature snow melt	$[^{\circ} C]$<br>

where

$$a_{sm} = 1.4 \cdot \frac{\rho_s}{0.1}$$

$a_{sm}$ Snow ageing (limited to 4.7) $[kg \, dm^{-3}]$<br>
$\rho_s$ Snow density $[kg \, dm^{-3}]$<br>

Liquid water in the snow layer re-freezes below –1.7 °C:

$$W_{sf} = 1.5 \cdot (T_{sm} - T_a)^{0.36}$$

$W_{sf}$ Re-freezing water in the snow layer $[mm]$<br>
$T_a$ Daily mean air temperature $[^{\circ} C]$<br>
$T_{sm}$ Base temperature snow melt	$[^{\circ} C]$<br>

The water-holding capacity of snow is calculated within given boundaries as:

$$C_s = \frac{0.1 \cdot C_{smax}}{\rho_s}$$

$C_s$ Water-holding capacity of snow $[mm]$<br>
$C_{smax}$	Maximum water-holding capacity of snow	$[mm]$<br>
$\rho_s$ Snow density $[kg \, dm^{-3}]$<br>

From this, the amount of water held back in the snow layer can be calculated:

$$W_{sr} = C_s \cdot W_s$$

$W_{sr}$ Liquid water in the snow layer	$[mm]$<br>
$C_s$ Water-holding capacity of snow $[mm]$<br>
$W_s$ Water equivalent in the snow layer $[mm]$<br>

where

$$W_s = W_f \cdot W_l$$

$W_s$ Water equivalent in the snow layer $[mm]$<br>
$W_f$ Water equivalent of the frozen water $[mm]$<br>
$W_l$ Liquid water in the snow layer $[mm]$<br>

$$W_f(t) = W_f(t-\Delta t) + N_s - W_{sm} + W_{sf}$$

$W_f(t)$ Water equivalent frozen water at time $t$ $[mm]$<br>
$W_f(t-\Delta t)$ Water equivalent frozen water at preceding time step $[mm]$<br>
$W_{sm}$ Water from melting snow $[mm]$<br>
$W_{sf}$ Re-freezing water in the snow layer $[mm]$<br>

and

$$W_l(t) = W_l(t-\Delta t) + N_l + W_{sm} - W_{sf}$$

$W_l(t)$ Liquid water in the snow layer at time $t$	$[mm]$<br>
$W_l(t-\Delta t)$ Liquid water in the snow layer at preceding time step	$[mm]$<br>
$W_{sm}$ Water from melting snow $[mm]$<br>
$W_{sf}$ Re-freezing water in the snow layer $[mm]$<br>

The amount of liquid water flowing from the snow layer onto the soil surface is calculated for the case $W_l$ > $W_{sr}$  $W_l$ > $W_{sr}$ as

$$W_i = W_l - W_{sr}$$

$W_i$ Liquid water flowing from the snow layer $[mm]$<br>
$W_l$ Liquid water in the snow layer $[mm]$<br>
$W_{sr}$ Liquid water in the snow layer	$[mm]$<br>

Finally, snow height can be calculated as:

$$S = \frac{W_s \cdot  \rho_W}{\rho_s}$$

$S$ Snow height	$[mm]$<br>
$W_s$ Water equivalent of snow $[mm]$<br>
$\rho_W$ Snow density $[kg \, dm^{-3}]$<br>
$\rho_s$ Density of water $[kg \, dm^{-3}]$<br>