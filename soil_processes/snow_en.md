# Snow

Snow layers are simulated following the idea of Riley and Bonesmo (2005). Below 1.8°C air temperature, an increasing fraction of snow is assumed in the falling rain, which adds to a snow layer.

$`\small n_l = \begin{cases}  1 & T_a \geq T_{as}  \\  \frac{T_a - T_{ls}}{T_{as} - T_{ls}} & T_{ls} < T_a < T_{as} \\ 0 & T_a \leq T_ls    \end{cases}`$

$`\small n_l `$	Fraction of liquid precipitation	$`\small [mm \, mm^{-1}] `$<br>
$`\small T_a`$	Daily mean air temperature	$`\small [^{\circ} C] `$<br>
$`\small T_{ls}`$	Threshold temperature for liquid water in snow	$`\small [^{\circ} C] `$<br>
$`\small T_{as}`$	Threshold temperature for snow accumulation	$`\small [^{\circ} C] `$<br>

The amounts of liquid and snow precipitation are derived from:

$`\small N_l = n_l \cdot k_l `$

$`\small N_l`$	Liquid precipitation	$`\small [mm] `$<br>
$`\small n_l`$	Fraction of liquid precipitation	$`\small [mm \, mm^{-1}] `$<br>
$`\small k_l`$	Correction factor liquid precipitation	$`\small [mm] `$<br>

and

$`\small N_s = (1-n_l) \cdot k_s `$

$`\small N_s`$	Snow precipitation	$`\small [mm] `$<br>
$`\small n_l `$	Fraction of liquid precipitation	$`\small [mm \, mm^{-1}] `$<br>
$`\small k_s`$	Correction factor snow precipitation	$`\small [mm] `$<br>

The snow layer density is calculated as follows:

$`\small \rho_{sn} = \rho_{sn\,min} + \rho_{sn\, max} \cdot \frac{T_a - T_{ls}}{T_{as} - T_{ls}} `$

$`\small \rho_{sn}`$	Density of fresh snow	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_{sn \, min}`$	Minimum density of fresh snow	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_{sn \, max}`$	Maximum density of fresh snow	$`\small [kg \, dm^{-3}] `$<br>
$`\small T_a`$	Daily mean air temperature	$`\small [^{\circ} C] `$<br>
$`\small T_{ls}`$	Threshold temperature for liquid water in snow	$`\small [^{\circ} C] `$<br>
$`\small T_{as}`$	Threshold temperature for snow accumulati	$`\small [^{\circ} C] `$<br>

Snow starts thawing above 0.31 °C and increases its density:

$`\small W_{sm} = \begin{cases}  0 & T_a < T_{sm} \\ a_{sm} \cdot ( T_a - T_{sm}) & T_a \geq T_{sm}      \end{cases} `$

$`\small W_{sm}`$	Water from melting snow	$`\small [mm] `$<br>
$`\small a_{sm}`$	Snow ageing (limited to 4.7)	 <br>
$`\small T_a`$	Daily mean air temperature	$`\small [^{\circ} C] `$<br>
$`\small T_{sm}`$	Base temperature snow melt	$`\small [^{\circ} C] `$<br>

where

$`\small a_{sm} = 1.4 \cdot \frac{\rho_s}{0.1}`$

$`\small a_{sm}`$	Snow ageing (limited to 4.7)	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_s`$	Snow density	$`\small [kg \, dm^{-3}] `$<br>

Liquid water in the snow layer re-freezes below –1.7 °C:

$`\small W_{sf} = 1.5 \cdot (T_{sm} - T_a)^{0.36} `$

$`\small W_{sf}`$	Re-freezing water in the snow layer	$`\small [mm] `$<br>
$`\small T_a`$	Daily mean air temperature	$`\small [^{\circ} C] `$<br>
$`\small T_{sm}`$	Base temperature snow melt	$`\small [^{\circ} C] `$<br>

The water holding capacity of snow is calculated within given boundaries as

$`\small C_s = \frac{0.1 \cdot C_{smax}}{\rho_s} `$

$`\small C_s `$	Water holding capacity of snow	$`\small [mm `$<br>
$`\small C_{smax}`$	Maximum Water holding capacity of snow	$`\small [mm `$<br>
$`\small \rho_s`$	Snow density	$`\small [kg \, dm^{-3}] `$<br>

From this, the amount of water held back in the snow layer can be calculated:

$`\small W_{sr} = C_s \cdot W_s `$

$`\small W_{sr}`$	Liquid water in the snow layer	$`\small [mm] `$<br>
$`\small C_s`$	Water holding capacity of snow	$`\small [mm] `$<br>
$`\small W_s`$	Water equivalent in the snow layer	$`\small [mm] `$<br>

where

$`\small W_s = W_f \cdot W_l`$

$`\small W_s`$	Water equivalent in the snow layer	$`\small [mm] `$<br>
$`\small W_f`$	Water equivalent of the frozen water	$`\small [mm] `$<br>
$`\small W_l`$	Liquid water in the snow layer	$`\small [mm] `$<br>

$`\small W_f(t) = W_f(t-\Delta t) + N_s - W_{sm} + W_{sf} `$

$`\small W_f(t)`$	Water equivalent frozen water at time t	$`\small [mm] `$<br>
$`\small W_f(t-\Delta t)`$	Water equivalent frozen water at preceding time step	$`\small [mm] `$<br>
$`\small W_{sm}`$	Water from melting snow	$`\small [mm] `$<br>
$`\small W_{sf}`$	Re-freezing water in the snow layer	$`\small [mm] `$<br>

and

$`\small W_l(t) = W_l(t-\Delta t) + N_l + W_{sm} - W_{sf}`$

$`\small W_l(t)`$	Liquid water in the snow layer at time t	$`\small [mm] `$<br>
$`\small W_l(t-\Delta t)`$	Liquid water in the snow layer at preceding time step	$`\small [mm] `$<br>
$`\small W_{sm} `$	Water from melting snow	$`\small [mm] `$<br>
$`\small W_{sf}`$	Re-freezing water in the snow layer	$`\small [mm] `$<br>

The amount of liquid water flowing from the snow layer onto the soil surface is calculated for the case Wl > Wsr  Wl > Wsr as

$`\small W_i = W_l - W_{sr} `$

$`\small W_i`$	Liquid water flowing from the snow layer	$`\small [mm] `$<br>
$`\small W_l`$	Liquid water in the snow layer	$`\small [mm] `$<br>
$`\small W_{sr}`$	Liquid water in the snow layer	$`\small [mm] `$<br>

Finally, snow height can be caluclated as:

$`\small S = \frac{W_s \cdot  \rho_W}{\rho_s} `$

$`\small S`$	Snow height	$`\small [mm] `$<br>
$`\small W_s`$	Water equivalent of snow	$`\small [mm] `$<br>
$`\small\rho_W `$	Snow density	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_s`$	Density of water	$`\small [kg \, dm^{-3}] `$<br>