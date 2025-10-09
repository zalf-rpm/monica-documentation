# Schnee

Schneedecken werden nach einer Idee von Riley und Bonesmo (2005) simuliert. Im fallenden Niederschlag unterhalb einer Temperatur von 1.8°C wird von MONICA ein zunehmender Anteil Schnee angenommen und einer Schneedecke zugeführt.

$`\small n_l = \begin{cases}  1 & T_a \geq T_{as}  \\  \frac{T_a - T_{ls}}{T_{as} - T_{ls}} & T_{ls} < T_a < T_{as} \\ 0 & T_a \leq T_ls    \end{cases}`$

$`\small n_l `$	Anteil des Niederschlags in Form von Wasser	$`\small [mm \, mm^{-1}] `$<br>
$`\small T_a`$	Tagesmitteltemperatur	$`\small [^{\circ} C] `$<br>
$`\small T_{ls}`$	Grenztemperatur für flüssiges Wasser im Schnee	$`\small [^{\circ} C] `$<br>
$`\small T_{as}`$	Grenztemperatur für Schneeakkumulation	$`\small [^{\circ} C] `$<br>

Die entsprechenden Mengen Niederschlags in flüssiger und in Schneeform ergeben sich aus:

$`\small N_l = n_l \cdot k_l `$

$`\small N_l`$	Niederschlag in Form von Wasser	$`\small [mm] `$<br>
$`\small n_l`$	Anteil des Niederschlags in Form von Wasser	$`\small [mm \, mm^{-1}] `$<br>
$`\small k_l`$	Korrekturfaktor Niederschlag als Wasser	$`\small [mm] `$<br>

und

$`\small N_s = (1-n_l) \cdot k_s `$

$`\small N_s`$	Niederschlag in Form von Schnee	$`\small [mm] `$<br>
$`\small n_l `$	Anteil des Niederschlags in Form von Wasser	$`\small [mm \, mm^{-1}] `$<br>
$`\small k_s`$	Korrekturfaktor Niederschlag als Schnee	$`\small [mm] `$<br>

Die Dichte der Schneedecke wird wie folgt kalkuliert:

$`\small \rho_{sn} = \rho_{sn\,min} + \rho_{sn\, max} \cdot \frac{T_a - T_{ls}}{T_{as} - T_{ls}} `$

$`\small \rho_{sn}`$	Neuschneedichte	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_{sn \, min}`$	minimale Neuschneedichte	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_{sn \, max}`$	maximale Neuschneedichte	$`\small [kg \, dm^{-3}] `$<br>
$`\small T_a`$	Tagesmitteltemperatur	$`\small [^{\circ} C] `$<br>
$`\small T_{ls}`$	Grenztemperatur für flüssiges Wasser im Schnee	$`\small [^{\circ} C] `$<br>
$`\small T_{as}`$	Grenztemperatur für Schneeakkumulation	$`\small [^{\circ} C] `$<br>

Der Schnee beginnt oberhalb einer Temperatur von 0.31 °C zu schmelzen und sich zu verdichten:

$`\small W_{sm} = \begin{cases}  0 & T_a < T_{sm} \\ a_{sm} \cdot ( T_a - T_{sm}) & T_a \geq T_{sm}      \end{cases} `$

$`\small W_{sm}`$	Wasser aus Schneeschmelze	$`\small [mm] `$<br>
$`\small a_{sm}`$	Schneealterung (limitiert bei 4.7)	 <br>
$`\small T_a`$	Tagesmitteltemperatur	$`\small [^{\circ} C] `$<br>
$`\small T_{sm}`$	Basistemperatur Schneeschmelze	$`\small [^{\circ} C] `$<br>

mit

$`\small a_{sm} = 1.4 \cdot \frac{\rho_s}{0.1}`$

$`\small a_{sm}`$	Schneealterung (limitiert bei 4.7)	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_s`$	Schneedichte	$`\small [kg \, dm^{-3}] `$<br>

Flüssiges Wasser in der Schneedecke wiedergefriert unterhalb –1.7 °C:

$`\small W_{sf} = 1.5 \cdot (T_{sm} - T_a)^{0.36} `$

$`\small W_{sf}`$	Wiedergefrierendes Wasser in der Schneedecke	$`\small [mm] `$<br>
$`\small T_a`$	Tagesmitteltemperatur	$`\small [^{\circ} C] `$<br>
$`\small T_{sm}`$	Basistemperatur Schneeschmelze	$`\small [^{\circ} C] `$<br>

Die Wasserhaltekapazität des Schnees innerhalb vorgegebner Kapazitätsgrenzen wird berechnet als

$`\small C_s = \frac{0.1 \cdot C_{smax}}{\rho_s} `$

$`\small C_s `$	Wasserhaltekapazität des Schnees	$`\small [mm `$<br>
$`\small C_{smax}`$	Maximale Wasserhaltekapazität des Schnees	$`\small [mm `$<br>
$`\small \rho_s`$	Schneedichte	$`\small [kg \, dm^{-3}] `$<br>

Daraus ergibt sich die Menge Wassers, das in der Schneedecke gehalten wird:

$`\small W_{sr} = C_s \cdot W_s `$

$`\small W_{sr}`$	Zurückgehaltenes  Wasser in der Schneedecke	$`\small [mm] `$<br>
$`\small C_s`$	Wasserhaltekapazität des Schnees	$`\small [mm] `$<br>
$`\small W_s`$	Wasseräquivalent der Schneedecke	$`\small [mm] `$<br>

mit

$`\small W_s = W_f \cdot W_l`$

$`\small W_s`$	Wasseräquivalent der Schneedecke	$`\small [mm] `$<br>
$`\small W_f`$	Wasseräquivalent des gefrorenen Wassers	$`\small [mm] `$<br>
$`\small W_l`$	Flüssiges Wasser in der Schneedecke	$`\small [mm] `$<br>

$`\small W_f(t) = W_f(t-\Delta t) + N_s - W_{sm} + W_{sf} `$

$`\small W_f(t)`$	Wasseräquivalent gefrorenes Wasser zum Zeitpunkt t	$`\small [mm] `$<br>
$`\small W_f(t-\Delta t)`$	Wasseräquivalent gefrorenes Wasser zum vorangegangenen Zeitpunkt	$`\small [mm] `$<br>
$`\small W_{sm}`$	Wasser aus Schneeschmelze	$`\small [mm] `$<br>
$`\small W_{sf}`$	Wiedergefrierendes Wasser in der Schneedecke	$`\small [mm] `$<br>

und

$`\small W_l(t) = W_l(t-\Delta t) + N_l + W_{sm} - W_{sf}`$

$`\small W_l(t)`$	Flüssiges Wasser in der Schneedecke zum Zeitpunkt t	$`\small [mm] `$<br>
$`\small W_l(t-\Delta t)`$	Flüssiges Wasser in der Schneedecke zum vorangegangenen Zeitpunkt	$`\small [mm] `$<br>
$`\small W_{sm} `$	Wasser aus Schneeschmelze	$`\small [mm] `$<br>
$`\small W_{sf}`$	Wiedergefrierendes Wasser in der Schneedecke	$`\small [mm] `$<br>

Der Zufluss flüssigen Wassers aus der Schneedecke an die Bodenoberfläche ergibt sich für den Fall Wl > Wsr als

$`\small W_i = W_l - W_{sr} `$

$`\small W_i`$	Aus der Schneedecke fließendes Wasser	$`\small [mm] `$<br>
$`\small W_l`$	Flüssiges Wasser in der Schneedecke	$`\small [mm] `$<br>
$`\small W_{sr}`$	Zurückgehaltenes  Wasser in der Schneedecke	$`\small [mm] `$<br>

Schließlich lässt sich die Schneehöhe errechnen:

$`\small S = \frac{W_s \cdot  \rho_W}{\rho_s} `$

$`\small S`$	Schneehöhe	$`\small [mm] `$<br>
$`\small W_s`$	Wasseräquivalent des Schnees	$`\small [mm] `$<br>
$`\small\rho_W `$	Schneedichte	$`\small [kg \, dm^{-3}] `$<br>
$`\small \rho_s`$	Dichte von Wasser	$`\small [kg \, dm^{-3}] `$<br>
