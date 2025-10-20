# Soil temperature

The model for soil temperature was taken from the 4C simulation model (Lasch et al., 2002). It uses an empirical function for the description of heat conductivity which was presented by Neusypina (1979).

$$\lambda_h = \frac{3.0 \cdot \rho_B - 1.7}{1.0 + (11.5 - 5.0 \cdot \rho_B) \cdot e^{ -50 \cdot \left( \frac{\theta}{\rho_B} \right) ^{1.5} } } \cdot 418.4$$

$\lambda_h$ Soil heat conductivity $[J \, m^{-1} \, s^{-1} \, K^{-1}]$<br>
$\rho_B$ Soil raw density $[Mg \, m^{-1}]$<br>
$\theta$ Soil moisture content $[m^3 \, m^{-3}]$<br>

Soil heat capacity is calculated according to Abrahamsen & Hansen (2000):

$$c_s = \theta \cdot \rho_w \cdot c_w + \varepsilon \cdot \rho_a \cdot c_a + o \cdot \rho_h \cdot c_h + (1-\theta - \varepsilon - o) \cdot \rho_q \cdot c_q$$

$c_s$ Specific soil heat capacity $[J \, m^{-3} \, K^{-1}]$<br>
$c_w$ Specific heat capacity of water $[J \, m^{-3} \, K^{-1}]$<br>
$c_a$ Specific heat capacity of air	$[J \, m^{-3} \, K^{-1}]$<br>
$c_h$ Specific heat capacity of soil organic matter	$[J \, m^{-3} \, K^{-1}]$<br>
$c_q$ Specific heat capacity of quarz $[J \, m^{-3} \, K^{-1}]$<br>
$\rho_w$ Density of water $[Mg \, m^{-3}]$<br>
$\rho_a$ Density of air	$[Mg \, m^{-3}]$<br>
$\rho_h$ Density of soil organic matter	$[Mg \, m^{-3}]$<br>
$\rho_q$ Density of soil minerals $[Mg \, m^{-3}]$<br>
$\theta$ Volumetric soil moisture content $[m^{3} \, m^{-3}]$<br>
$\varepsilon$ Volumetric soil air content $[m^{3} \, m^{-3}]$<br>
$o$	Volumetric soil organic matter content $[m^{3} \, m^{-3}]$<br>

The soil surface temperature is calculated using minimum and maximum air temperature and global radiation according to Williams (1984):

$$T_s(t) = (1-\alpha(t)) \cdot (T_{min}(t) + ( T_{max}(t) - T_{min}(t) ) \cdot \sqrt{ 0.03 \cdot R_g(t)}  ) + \alpha(t) \cdot T_s(t_\Delta t)$$

$T_s(t)$ Soil surface temperature at time $t$ $[^{\circ}C]$<br>
$T_{min}$ Minimum air temperature in 2 m height $[^{\circ}C]$<br>
$T_{max}$ Maximum air temperature in 2 m height $[^{\circ}C]$<br>
$R_g$ Global radiation $[MJ \, m^{-2}]$<br>
$\alpha(t)$ Albedo at time $t$<br>
$T_s(t-\Delta t)$ Soil surface temperature at previous time step $[^{\circ}C]$<br>

The impact of global radiation $R_g$ on soil surface temperature is limited to values above 8.33 MJ m–2 d–1.

___

#### References

* Abrahamsen, P., Hansen, S., 2000. Daisy: an open soil-crop-atmosphere system