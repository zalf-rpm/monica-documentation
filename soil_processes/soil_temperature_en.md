# Soil temperature

The model for soil temperature was taken from the 4C simulation model (Lasch et al., 2002). It uses an empirical function for the description of heat conductivity which was presented by Neusypina (1979).

$`\small \lambda_h = \frac{3.0 \cdot \rho_B - 1.7}{1.0 + (11.5 - 5.0 \cdot \rho_B) \cdot e^{ -50 \cdot \left( \frac{\theta}{\rho_B} \right) ^{1.5} } } \cdot 418.4`$

$`\small \lambda_h `$	Soil heat conductivity	$`\small [J \, m^{-1} \, s^{-1} \, K^{-1}] `$<br>
$`\small \rho_B `$	Soil raw density	$`\small [Mg \, m^{-1}] `$<br>
$`\small \theta `$	Soil moisture conten	$`\small [m^3 \, m^{-3}] `$<br>

Soil heat capacity is calculated according to Abrahamsen & Hansen (2000)

$`\small c_s = \theta \cdot \rho_w \cdot c_w + \varepsilon \cdot \rho_a \cdot c_a + o \cdot \rho_h \cdot c_h + (1-\theta - \varepsilon - o) \cdot \rho_q \cdot c_q `$

$`\small c_s `$	Specific soil heat capacity	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_w `$	Specific heat capacity of water	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_a`$	Specific heat capacity of air	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_h `$	Specific heat capacity of soil organic matter	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_q`$	Specific heat capacity of quarz	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small \rho_w`$	Density of water	$`\small [Mg \, m^{-3}] `$<br>
$`\small \rho_a`$	Density of air	$`\small [Mg \, m^{-3}] `$<br>
$`\small \rho_h`$	Density of soil organic matter	$`\small [Mg \, m^{-3}] `$<br>
$`\small \rho_q`$	Density of soil minerals	$`\small [Mg \, m^{-3}] `$<br>
$`\small \theta`$	Volumetric soil moisture content	$`\small [m^{3} \, m^{-3}] `$<br>
$`\small \varepsilon`$	Volumetric soil air content	$`\small [m^{3} \, m^{-3}] `$<br>
$`\small o`$	Volumetric soil organic mater content	$`\small [m^{3} \, m^{-3}] `$<br>

The soil surface temperature is calculated using minimum and maximum air temperature and global radiation according to Williams (1984).

$`\small T_s(t) = (1-\alpha(t)) \cdot (T_{min}(t) + ( T_{max}(t) - T_{min}(t) ) \cdot \sqrt{ 0.03 \cdot R_g(t)}  ) + \alpha(t) \cdot T_s(t_\Delta t))  `$

$`\small T_s(t)`$	Soil surface temperature at time t	$`\small [^{\circ}C] `$<br>
$`\small T_{min}`$	Minimum air temperature in 2m height	$`\small [^{\circ}C] `$<br>
$`\small T_{max}`$	Maximum air temperature in 2m height	$`\small [^{\circ}C] `$<br>
$`\small R_g`$	Global radiation	$`\small [MJ \, m^{-2}] `$<br>
$`\small \alpha(t)`$	Albedo at time t	 <br>
$`\small T_s(t-\Delta t)`$	Soil surface temperature at previous time step	$`\small [^{\circ}C] `$<br>

The impact of global radiation Rg on soil surface temperature is limited to values above 8.33  MJ m–2 d–1 .

## References

* Abrahamsen, P., Hansen, S., 2000. Daisy: an open soil-crop-atmosphere system
