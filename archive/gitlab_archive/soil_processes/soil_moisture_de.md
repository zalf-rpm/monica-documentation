# Bodenfeuchte

Für die Beschreibung der Bodenwasserdynamik wurde ein Kapazitätsansatz gewählt (Wegehenkel, 2000). Die Kapazitätsparameter werden aus der Korngrößenzusammensetzung abgeleitet und durch den Humusgehalt und die Trockenrohdichte des Bodens modifiziert. Die entsprechenden Wassergehalte bei Sättigung, bei Feldkapazität und am Permanenten Welkepunkt für unterschiedliche Trockenrohdichten sowie die Korrekturwerte für die Humusklassen (AG Bodenkunde, 2005) finden sich bei Wessolek et al. (2009) und sind in der Datenbank hinterlegt.

Wenn ein Pflanzenbestand etabliert ist, wird ein Teil des Niederschlags an der Bestandesoberfläche zurückgehalten und verdunstet von dort. Die Interzeption I berechnet sich nach

$`\small I = (2.5 \cdot h_c \cdot \beta) - S_i`$

$`\small  I`$	Interzeption	$`\small  [mm]`$<br>
$`\small  h_c`$	Bestandeshöhe	$`\small  [m]`$<br>
$`\small  \beta`$	Bedeckungsgrad	$`\small  [m^2\,m^{-2}]`$<br>
$`\small  S_i`$	Interzeptionsspeicher	$`\small  [mm]`$<br>

Der übrige Niederschlag fällt zu Boden und wird einem Oberflächenspeicher zugeführt, von dem das Niederschlagswasser in den Boden infiltriert. Solange der Oberflächenspeicher Wasser enthält, ist er die einzige Quelle für die Evaporation. Perkolation von Wassermengen oberhalb der Feldkapazität wird bestimmt von den bodenhydraulischen Kennwerten und einem empirischen, bodenartabhängigen Ratenkoeffizienten  ($`\small \lambda`$). 

$`\small \lambda = 1.15 \cdot f^2_s + 0.1 \cdot f_c + 0.35 \cdot f_u`$

$`\small \lambda`$	Empirischer Perkolationsratenkoeffizient	 <br>
$`\small f_s`$	Sandgehalt des Bodens	$`\small [kg \, kg^{-1}] `$<br>
$`\small f_c`$	Tongehalt des Bodens	$`\small [kg \, kg^{-1}] `$<br>
$`\small f_u`$	Schluffgehalt des Bodens	$`\small [kg \, kg^{-1}] `$<br>

Für den Fall dass der Grundwasserspiegel innerhalb des Bodenprofils liegt, kann ein konstanter Grundwasserabfluss angepasst werden, um das Steigen und Fallen des Grundwasserspiegels in Abhängigkeit von der Wasserbilanz im Boden abzubilden. Virtuellen Bodenschichten mit einem Wassergehalt unterhalb 70% der Feldkapazität wird Wasser von der jeweils unten angrenzenden Schicht zugeführt, wenn diese einen Wassergehalt oberhalb der Feldkapazität aufweist (Kapillarer Aufstieg).

Die Referenz-Evapotranspiration für eine kurz geschnittene Grasfläche ET0 wird mit Hilfe der Penman-Monteith-Methode berechnet (1998).

$`\small ET_0 = \frac{0.408 \cdot \Delta \cdot (R_n - G) + \gamma \cdot \frac{900}{T + 273} \cdot u_2 \cdot (e_s - e_a)} {\Delta + \gamma \cdot (1 + \frac{r_a}{r_s} )} `$

$`\small \Delta `$	Gradient der Dampfdruckkurve	$`\small [kPa \, K^{-1}] `$<br>
$`\small R_n`$	Nettostrahlung an der Bestandesoberfläche	$`\small [MJ \, m^{-2} \, d^{-1}] `$<br>
$`\small G`$	Bodenwärmeflussdichte	$`\small [MJ \, m^{-2} \, d^{-1}] `$<br>
$`\small T`$	Tagesmitteltemperatur der Luft in 2 m Höhe	$`\small [^{\circ}C] `$<br>
$`\small u_2`$	Windgeschwindigkeit in 2 m Höhe	$`\small [m\,s^{-1}] `$<br>
$`\small e_s `$	Sättigungsdampfdruck	$`\small [kPa] `$<br>
$`\small e_a`$	aktueller Dampfdruck	$`\small [kPa] `$<br>
$`\small \gamma`$	Psychrometerkonstante	$`\small [kPa\, K^{-1}] `$<br>
$`\small r_s`$	Atmosphärischer Widerstand	$`\small [s \, m^{-1}] `$<br>
$`\small r_a`$	Oberflächenwiderstand	$`\small [s \, m^{-1}] `$<br>

mit

$`\small \gamma = 6.65 \cdot 10^{-4} \cdot P `$

$`\small P `$	Atmospherischer Druck	$`\small [Pa] `$<br>

Für die Berechnung der Referenz-Transpiration errechnet sich der Oberflächenwiderstand unter der Annnahme eines 12cm hohen Grasbestandes als:

$`\small r_s = \frac{r_1} {1.44}`$

$`\small r_1`$	Stomata-Widerstand; 100 s m-1	$`\small [s \, m^{-1}] `$<br>

Für den tatsächlichen Pflanzenbestand werden die Bestandeshöhe und der Blattflächenindex berücksichtigt.

$`\small  r_s = \frac{r_1}{LAI \cdot h} `$

$`\small r_1`$	Stomata-Widerstand	$`\small [s\,m^{-1}] `$<br>
$`\small LAI `$	Blattflächenindex	$`\small [m^2\,m^{-2}] `$<br>
$`\small h`$	Bestandeshöhe	$`\small [m] `$<br>

Der Stomata-Widerstand wird im Pflanzenwachstumsmodul berechnet. Für alle anderen Variablen in dieser Gleichung wird auf die Vorgaben der FAO verwiesen (Allen et al., 1998).

Die fruchtartspezifische potentielle Evapotranspiration wird berechnet mit Hilfe von ebenso fruchtartspezifischen Faktoren (Kc) während der Wachstumsperiode und Faktoren für den unbedeckten Boden in der Zeit zwischen Ernte und Auflaufen der neuen Frucht. Die Kc-Faktoren sind an die Entwicklungsstadien der Fruchtart gekoppelt. Die Anteile von Evapora­tion und Transpiration an der Gesamtverdunstung werden vom Bedeckungsrad abgeleitet. Die Berechnung der aktuellen Evaporation und den aktuellen Bodenwassergehalt.