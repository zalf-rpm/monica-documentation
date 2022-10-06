# Bodentemperatur

Das Bodentemperaturmodell wurde dem Modell 4C (2002) entnommen. Dieser Ansatz verwendet eine empirische Funktion von Neusypina (1979) für die Beschreibung der Wärmeleitfähigkeit

$`\small \lambda_h = \frac{3.0 \cdot \rho_B - 1.7}{1.0 + (11.5 - 5.0 \cdot \rho_B) \cdot e^{-50 \cdot \left( \frac{ \theta}{\rho_B} \right) ^{1.5} } } \cdot 418.4`$

$`\small \lambda_h `$	Wärmeleitfähigkeit des Bodens	$`\small [J \, m^{-1} \, s^{-1} \, K^{-1}] `$<br>
$`\small \rho_B `$	Trockenrohdichte des Bodens	$`\small [Mg \, m^{-1}] `$<br>
$`\small \theta `$	Wassergehalt des Bodens	$`\small [m^3 \, m^{-3}] `$<br>

Die Wärmekapazität des Bodens wird nach Abrahamsen & Hansen (2000) kalkuliert.

$`\small c_s = \theta \cdot \rho_w \cdot c_w + \varepsilon \cdot \rho_a \cdot c_a + o \cdot \rho_h \cdot c_h + (1-\theta - \varepsilon - o) \cdot \rho_q \cdot c_q `$

$`\small c_s `$	Spezifische Wärmekapazität des Bodens	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_w `$	Spezifische Wärmekapazität des Wassers	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_a`$	Spezifische Wärmekapazität der Luft	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_h `$	Spezifische Wärmekapazität der organischen Substanz im Boden	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small c_q`$	Spezifische Wärmekapazität von Quarz	$`\small [J \, m^{-3} \, K^{-1}] `$<br>
$`\small \rho_w`$	Dichte des Wassers	$`\small [Mg \, m^{-3}] `$<br>
$`\small \rho_a`$	Dichte der Luft	$`\small [Mg \, m^{-3}] `$<br>
$`\small \rho_h`$	Dichte der organischen Bodensubstanz	$`\small [Mg \, m^{-3}] `$<br>
$`\small \rho_q`$	Dichte der mineralischen Bodensubstanz	$`\small [Mg \, m^{-3}] `$<br>
$`\small \theta`$	Volumetrischer Wassergehalt des Bodens	$`\small [m^{3} \, m^{-3}] `$<br>
$`\small \varepsilon`$	Volumetrischer Luftgehalt des Bodens	$`\small [m^{3} \, m^{-3}] `$<br>
$`\small o`$	Volumetrischer Gehalt der organischen Substanz im Boden	$`\small [m^{3} \, m^{-3}] `$<br>

Die Bodenoberflächentemperatur wird aus der Minimum- und der Maximumtemperatur der Luft und aus der Globalstrahlung nach Williams (1984) berechnet.

$`\small T_s(t) = (1-\alpha(t)) \cdot (T_{min}(t) + ( T_{max}(t) - T_{min}(t) ) \cdot \sqrt{ 0.03 \cdot R_g(t)}  ) + \alpha(t) \cdot T_s(t-\Delta t)  `$

$`\small T_s(t)`$	Bodenoberflächentemperatur zum Zeitpunkt t	$`\small [^{\circ}C] `$<br>
$`\small T_{min}`$	Minimum-Lufttemperatur in 2m Höhe	$`\small [^{\circ}C] `$<br>
$`\small T_{max}`$	Maximum-Lufttemperatur in 2m Höhe	$`\small [^{\circ}C] `$<br>
$`\small R_g`$	Globalstrahlung	$`\small [MJ \, m^{-2}] `$<br>
$`\small \alpha(t)`$	Albedo zum Zeitpunkt t	 <br>
$`\small T_s(t-\Delta t)`$	Bodenoberflächentemperatur zum vorangegangenen Zeitpunkt 	$`\small [^{\circ}C] `$<br>

Der Einfluss der Globalstrahlung Rg auf die Bodenoberflächentemperatur ist auf Werte oberhalb von 8.33 MJ m–2 d–1 begrenzt.
