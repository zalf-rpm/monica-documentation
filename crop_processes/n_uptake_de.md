# N-Aufnahme

Die N-Aufnahme der Pflanze aus dem Boden wird nach der Idee von Kersebaum (1989) modelliert. Der tägliche Stickstoffbedarf der Pflanze wird berechnet durch

$`\small N_{pot} = \left( N_{target} \cdot W + N_{max \, root} \cdot W_{root} + N_{target} \cdot \frac{W_b}{p_N} - N_{crop}^{*} \right) \cdot \Delta t`$

$`\small  N_{pot}`$	Tägliches Stickstoffaufnahmevermögen	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small N_{target} `$	Maximale N-Konzentration in oberirdischen Pflanzenteilen $`\small [kg \, N \,kg \, TM^{-1}] `$<br>
$`\small W`$	Oberirdische Trockenmasse	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small N_{max \, root} `$	Maximale N-Konzentration in der Wurzel	$`\small [kg \, N \,kg \, TM^{-1}] `$<br>
$`\small W_{root}`$	Wurzel-Trockenmasse	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small W_b`$	Unterirdische Trockenmasse (nicht Wurzel)	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small p_N`$	N-Verteilungskoeffizient	 <br>
$`\small N_{crop}^{*}`$	Gesamt-N-Gehalt der Pflanze im vergangenen Zeitschritt	$`\small [kg \, N \, ha^{-1}] `$<br>
$`\small \Delta t`$	Zeitschritt	$`\small [d] `$<br>

Er beträgt maximal $`\small 6 \, kg \, N \, ha^{-1} \, d^{-1} `$. Die tägliche N-Aufnahme wird bestimmt durch die Wurzellänge und einen Begrenzungsfaktor, der mit der Ontogenese der Pflanze linear abnimmt und dem abnehmenden Anteil aktiver Wurzeloberfläche zu reinen Leitungswurzeln Rechnung trägt.

$`\small \Delta N_{lim} = L_{root} \cdot \left( N_{up \, max} - \frac{DD_{act}}{DD_{crop}} \right)`$

$`\small \Delta N_{lim}`$	Limit der täglichen N-Aufnahme	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small L_{root}`$	Gesamte Wurzellänge	$`\small [m \, m^{-2}]`$<br>
$`\small N_{up \, max}`$	Pflanzenspezifische maximale N-Aufnahme	$`\small [kg \, N \, m \, Wurzel^{-1}]`$<br>
$`\small DD_{act}`$	Aktuelle Wärmesumme	$`\small [^{\circ}C \, d]`$<br>
$`\small DD_{crop}`$	Pflanzenspezifische gesamte Wärmesumme	$`\small [^{\circ}C \, d]`$<br>

Es wird angenommen, dass N ausschließlich in der Nitratform aufgenommen wird. Der Transport erfolgt konvektiv mit dem Transpirationsstrom.

$`\small N_{konv \, max} = \sum_{z=1}^{Q \cdot R_z} T_z \cdot c_z \cdot \Delta z \cdot \Delta t `$

$`\small N_{konv \, max}`$	Maximale konvektive N-Aufnahme	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small R_z `$	Durchwurzelungstiefe	$`\small [m]`$<br>
$`\small q`$	Verhältnis absolute zu simulierter Durchwurzelungstiefe	 <br>
$`\small T_z`$	Transpiration	$`\small [mm]`$<br>
$`\small c_z`$	N-Konzentration in der Bodenlösung	$`\small [kg \, N \, m^{-3}]`$<br>
$`\small \Delta z`$	Schichtmächtigkeit	$`\small [m] `$<br>
$`\small \Delta t `$	Zeitschritt	$`\small [d]`$<br>

Übersteigt die konvektive Anlieferung das Aufnahmevermögen der Pflanze, berechnet sich die Aufnahme in den einzelnen Schichten

$`\small N_{konv_z} = T_z \cdot c_z \cdot \frac{N_{pot}}{N_{mas}} \cdot \Delta z \cdot \Delta t`$

$`\small N_{konv_z}`$	Tägliche konvektive N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small T_z`$	Transpiration aus Tiefe z	$`\small [mm] `$<br>
$`\small c_z`$	N-Konzentration in der Bodenlösung	$`\small [kg \, N \, m^{-3}]`$<br>
$`\small N_{pot}`$	Tägliches Stickstoffaufnahmevermögen	$`\small [kg \, N \, ha^{-1}]`$<br>
$`\small N_{mas}`$	Limit der täglichen N-Aufnahme	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small \Delta z`$	Schichtmächtigkeit	$`\small [m]`$<br>
$`\small \Delta t `$	Zeitschritt	$`\small [d]`$<br>

Reicht der konvektive N-Antransport jedoch nicht aus, um das Aufnahmepotenzial zu befriedigen, wird zusätzlich ein Antransport durch Diffusion berücksichtigt (Baldwin et al. 1973), dessen maximaler Betrag wie folgt berechnet wird:

$`\small N_{diff \, max} = \sum_{z=1}^{q \cdot R_z} N_{diff \, max_z} = \sum_{z=1}^{q \cdot R_z} 2 \cdot \pi \cdot r_w \cdot \Lambda_z \cdot D \cdot \frac{c_z - c_{min}} {r_z} \cdot \Delta z \cdot  \Delta t `$

$`\small N_{diff \, max}`$	Maximale diffusive N-Aufnahme	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small R_z`$	Durchwurzelungstiefe	$`\small [m] `$<br>
$`\small q `$	Verhältnis absolute zu simulierter Durchwurzelungstiefe	 <br>
$`\small N_{diff \, max_z}`$	Maximale diffusive N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small r_w`$	Wurzelradius	$`\small [m] `$<br>
$`\small \Lambda_z `$	Wurzellängendichte in Tiefe z	$`\small [m \, m^{-3}] `$<br>
$`\small D`$	Effektiver Dispersionskoeffizient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small c_z`$	N-Konzentration in der Bodenlösung	$`\small [kg \, N \, m^{-3}] `$<br>
$`\small c_{min}`$	Min. N-Konzentration an der Wurzeloberfläche (1.4·10–6)	$`\small [kg \, N \, m^{-3}] `$<br>
$`\small r_z`$	Halbe Wurzeldistanz	$`\small [m] `$<br>
$`\small \Delta z`$	Schichtmächtigkeit	$`\small [m] `$<br>
$`\small \Delta t `$	Zeitschritt	$`\small [d] `$<br>

mit der halben Distanz zwischen benachbarten Wurzeln unter Annahme einer Gleichverteilung

$`\small r_z = (\pi \cdot \Lambda_z)^{-0.5}`$

$`\small r_z`$	Halbe Wurzeldistanz	$`\small [m]`$<br>
$`\small \Lambda_z `$	Wurzellängendichte in Tiefe z	$`\small [m \, m^{-3}]`$<br>

und dem effektiven Dispersionskoeffizienten

$`\small D= \frac{1}{\tau} \cdot D_0 + D_v \cdot \vert \frac{q}{\theta}\vert`$

$`\small D`$	Effektiver Dispersionskoeffizient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small \tau `$	Tortuosität	 <br>
$`\small D_0`$	Diffusionskoeffizient in Lösung (2.14·10–5)	$`\small [m^2 \, d^{-1}]`$<br>
$`\small d_v`$	Dispersionsfaktor (0.05)	$`\small [m] `$<br>
$`\small q`$	Wasserflussdichte	$`\small [m^2 \, d^{-1}]`$<br>
$`\small \theta`$	Vol. Wassergehalt	$`\small [m^3 \, m^{-3}]`$<br>

mit

$`\small \tau = \frac {\theta}{a \cdot e^{b \cdot \theta}}`$

$`\small \tau`$	Tortuosität	 <br>
$`\small \theta`$	Vol. Wassergehalt	$`\small [m^3 \, m^{-3}]`$<br>
$`\small a, b`$	Faktoren	 <br>

Übersteigt die zusätzliche diffusive Anlieferung das Aufnahmevermögen der Pflanze, wird sie, begrenzt durch das Aufnahmevermögen, über die Bodenschichten verteilt.

$`\small N_{diff_z} = (N_{pot} - N_{konv \, max}) \cdot \frac{N_{diff \, max_z}}{N_{diff \, max}}`$

$`\small N_{diff_z} `$	Tägliche diffusive N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{pot}  `$	Tägliches Stickstoffaufnahmevermögen	$`\small[kg \, N \, m^{-2}] `$<br>
$`\small N_{konv \, max}  `$	Maximale konvektive N-Aufnahme	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{diff \, max_z} `$	Maximale diffusive N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small N_{diff \, max} `$	Maximale diffusive N-Aufnahme	$`\small [kg \, N \, m^{-2}]`$<br>

Die tatsächliche N-Aufnahme berechnet sich schließlich aus

$`\small N_{up_z} = N_{konv_z} + N_{diff_z} \,\,\,\,\,\,\,\,\,\,\, N_{up_z} \leq N_z - N_{min\,av}`$

$`\small N_{up_z} `$	Tägliche N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{konv_z} `$	Tägliche konvektive N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{diff_z} `$	Tägliche diffusive N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_z`$	N-Gehalt der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small N_{min\,av} `$	Minimaler N-Gehalt in der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>

und

$`\small N_{up} = \sum_{z=1}^{q \cdot R_z} N_{up_z} `$

$`\small N_{up} `$	Tägliche N-Aufnahme	$`\small [kg \, N \, m^{-2}]`$<br>
$`\small R_z`$	Durchwurzelungstiefe	$`\small [m]`$<br>
$`\small q`$	Verhältnis absolute zu simulierter Durchwurzelungstiefe	 <br>
$`\small N_{up_z}`$	Tägliche N-Aufnahme aus der Bodenschicht z	$`\small [kg \, N \, m^{-2}]`$<br>

## Literatur
Baldwin, J.P. et al. (1973): Uptake of solutes by multiple root systems from soil. III. A model for calculating the solute uptake by a randomly dispersed root system developing in a finite volume of soil. Plant Soil 38, 621 – 635.

Groot, J.J.R. (1987): Simulation of nitrogen balance in a system of winter wheat and soil. Simulation Report CABO-TT 13. Centre for agrobiological research and Department of Theoretical Production Ecology, Landwirtschaftliche Universität Wageningen, Niederlande.

Kersebaum, K.C. (1989): Die Simulation der Stickstoff-Dynamik von Ackerböden. Dissertation, Universität Hannover, p. 143.

 