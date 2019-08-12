# Wurzelwachstum

Die Simulation des Wurzelwachstums orientiert sich an dem Entwurf von Pedersen et al. (2010). Dabei wird die Wurzeltrockenmasse nach Gerwitz & Page (1974) über die Tiefe verteilt, wobei die Durchwurzelungstiefe exponentiell mit der modifizierten Thermalsumme zunimmt. Die pflanzenspezifische maximale Durchwurzelungstiefe wird mit Hilfe der bodenspezifischen maximalen Durchwurzelungstiefe modifiziert, welche aus dem Sandgehalt und der Lagerungsdichte des Bodens berechnet wird:

$`\small R_{max} = \frac{R_{B_{max}} + 2 \cdot R_{p_{max}}}{3} `$

$`\small R_{max}`$	Maximale Durchwurzelungstiefe	$`\small [m] `$<br>
$`\small R_{B_{max}} `$	Pflanzenspezifische maximale Durchwurzelungstiefe	$`\small [m] `$<br>
$`\small R_{p_{max}} `$	Bodenspezifische maximale Durchwurzelungstiefe	$`\small [m] `$<br>

und

$`\small R_{B_{max}} = f_s \cdot \left (   \frac{ ( 1.1-f_s) } {0.275} \right ) \cdot \left ( \frac{1.4}{\rho_B} + \frac{\rho^2_B}{40}  \right)  `$

$`\small R_{B_{max}}`$	Bodenspezifische maximale Durchwurzelungstiefe	$`\small [m] `$<br>
$`\small f_s `$	Sandgehalt des Bodens	$`\small [kg \, kg^{-1}] `$<br>
$`\small \rho_B `$	Lagerungsdichte des Bodens	$`\small [kg \, m^{-3}] `$<br>

wobei der Einfluss des Sandgehalts auf Gehalte < 0.55 kg kg-1 limitiert wird.

Die pflanzenspezifische Penetrationsrate der Wurzel in die Tiefe krzc wird bei niedrigen Tongehalten verringert.

![](monica_wurzel_abb.1.png)
Abbildung 1: Vertikale Penetrationsrate der Winterweizenwurzel in Abhängigkeit vom Tongehalt des Bodens.

Die Wärmesumme für das Wurzelwachstum wird mit Hilfe einer das Wachstum begrenzenden minimalen Temperatur gebildet:

$`\small DD_{root} = T_{av} - T_{r\,min}`$

$`\small DD_{root} `$	Wärmesumme Wurzelwachstum	$`\small [^{\circ}C \, d] `$<br>
$`\small T_{av} `$	Mittlere Tagestemperatur	$`\small [^{\circ}C] `$<br>
$`\small T_{r\,min} `$	Minimum-Temperatur für das Wurzelwachstum	$`\small [^{\circ}C] `$<br>

Der tägliche Wärmesummand ist auf 20°C beschränkt.

Damit ergibt sich die Durchwurzelungstiefe aus

$`\small R_z = R_{ini} + (DD_{root} - DD_{lag} ) \cdot k_{rz}`$

$`\small R_z`$	Aktuelle Durchwurzelungstiefe	$`\small [m] `$<br>
$`\small R_{ini} `$	Durchwurzelungstiefe bei Aussaat	$`\small [m] `$<br>
$`\small DD_{root} `$	Wärmesumme Wurzelwachstum	$`\small [^{\circ}C \, d] `$<br>
$`\small DD_{lag} `$	Verzögerung der Wurzelinitiierung	$`\small [^{\circ}C \, d] `$<br>

Die Wurzellänge in der jeweiligen Bodenschicht ergibt sich dann aus

$`\small L_{root} = W_{root} \cdot l_r `$

$`\small L_{root} `$	Gesamte Wurzellänge	$`\small [m \, m^{-2}] `$<br>
$`\small W_{root}`$	Biomasse im Kompartiment Wurzel	$`\small [kg \, m^{-2}] `$<br>
$`\small l_r `$	Pflanzenspezifische Wurzellänge	$`\small [m \, kg^{-1}] `$<br>

Der Verteilungsfaktor für die Wurzeldichte wird für jede Bodenschicht wie folgt berechnet:

$`\small \lambda_z = \begin{cases}   \lambda_0 e^{-a_z\cdot z} & z < R_z \\ \lambda_0 e^{-a_z \cdot z} \cdot \left ( 1- \frac{z-R_z}{q \cdot R_z - R_z }  \right ) & q \cdot R_z < z < R_z  \\ 0 & z > q \cdot R_z  \end{cases} `$

$`\small \lambda_z `$	Verteilungsfaktor der Wurzellängendichte in Tiefe z	$`\small [m \, m^{-3}] `$<br>
$`\small \lambda_0`$	Verteilungsfaktor der Wurzellängendichte in Tiefe 0	$`\small [kg \, m^{-3}] `$<br>
$`\small a_z`$	Formfaktor	 <br>
$`\small z`$	Tiefe	$`\small [m] `$<br>
$`\small R_z `$	Durchwurzelungstiefe	$`\small [m] `$<br>
$`\small q`$	Verhältnis absolute zu simulierter Durchwurzelungstiefe	 <br>

Unter Verwendung der auf den Bereich [0;1] normierten Faktoren wird die Wurzellängendichte für jede Bodenschicht errechnet:

$`\small \Lambda_z = \frac{\lambda_z}{\sum^{z_{max}}_{z=1} \lambda_z} \cdot L_{root} `$

$`\small \Lambda_z `$	Wurzellängendichte in Tiefe z	$`\small [m \, m^{-3}] `$<br>
$`\small \lambda_z`$	Verteilungsfaktor der Wurzellängendichte in Tiefe z	$`\small [m \, m^{-3}] `$<br>
$`\small z_{max} `$	Maximale Profiltiefe	$`\small [m] `$<br>
$`\small L_{root}`$	Gesamte Wurzellänge	$`\small [m \, m^{-2}] `$<br>

Der Wurzeldurchmesser wird schließlich für Pflanzen mit oberirdischem Speicherorgan (Getreide, Mais, Raps, etc) als mit der Tiefe abnehmend, für Pflanzen mit unterirdischem Speicherorgan (Zuckerrübe, Kartoffel, Möhre, etc.) als konstant angenommen:

![](monica_wurzel_abb.2.png)
Abbildung 2: Wurzeldurchmesser in Abhängigkeit von der Bodentiefe für Pflanzen mit oberirdischem (‑‑‑) und unterirdischem (—) Speicherorgan.

# Literatur:
Gerwitz, A., Page, E.R. (1974): An empirical mathematical model to describe plant root systems. J. Apl. Ecol. 11, 773-781.

Pedersen, A., Zhang, K.F., Thorup-Kristensen, K., Jensen, L.S. (2010): Modelling diverse root density dynamics and deep nitrogen uptake - A simple approach. Plant Soil 326 (1-2), 493-510.

 