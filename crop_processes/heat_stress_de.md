# Hitzestress

Die Pflanze wird im Modell MONICA durch vier Stressfaktoren im Wachstum beeinträchtigt: Wassermangel, Stickstoffmangel, Sauerstoffmangel und Hitzestress.

Hitzestress wirkt auf die Ausbildung von Fruchtanlagen während der Blüte. Hitze im Sinne des Modells wird definiert als das Überschreiten eines pflanzenspezifischen Schwellwertes während der Tageslichtperiode. Die Temperatur für diesen Tagesabschnitt wird nach Mirschel & Wenkel (2007) angenommen als

$`\small T_{photo} = T_{max} - \frac{T_{max}-T_{min}}{4} `$

$`\small T_{photo} `$	Temperatur während der Photoperiode	$`\small [^{\circ}C] `$<br>
$`\small T_{max}`$	Tagesmaximumtemperatur	$`\small [^{\circ}C] `$<br>
$`\small T_{min}`$	Tagesminimumtemperatur	$`\small [^{\circ}C] `$<br>

Während einer definierten pflanzenspezifischen Phase besonderer Sensitivität wird der Effekt der Extremtemperaturen auf die Pflanze nach Challinor et al. (2005) abgebildet

$`\small F_H = 1 - \left( \frac{T_{photo} - T_{critH}}{T_{limH} - T_{critH}} \right) \cdot r_F  `$

$`\small F_H`$	Hitzeeinwirkung auf Fruchtanlagen	$`\small [d^{-1}] `$<br>
$`\small T_{photo}`$	Temperatur während der Photoperiode	$`\small [^{\circ}C] `$<br>
$`\small T_{critH} `$	kritische Temperatur für Hitzestresseffekt	$`\small [^{\circ}C] `$<br>
$`\small T_{limH} `$	Maximale Temperatur für Hitzestresseffekt	$`\small [^{\circ}C] `$<br>
$`\small r_F `$	tägliche Öffnungsrate der Blüten	$`\small [d^{-1}] `$<br>

Die tägliche Öffnungsrate der Blüten während der Blühperiode wird dafür berechnet nach der Idee von Moriondo et al. (2011).

$`\small r_F = p_{F,d} - p_{F, d-1} `$

$`\small r_F`$	tägliche Öffnungsrate der Blüten	$`\small [d^{-1}] `$<br>
$`\small p_{F, d}`$	Anteil heute geöffneter Blüten	 <br>
$`\small p_{F, d-1} `$	Anteil gestern geöffneter Blüten	 <br>

mit

$`\small p_F =  \frac{1} { 1 + (\frac{1}{0.015} - 1) \cdot e^{-1.4 \cdot D_{BF}} } `$

$`\small p_F `$	Anteil geöffneter Blüten	 <br>
$`\small D_{BF}`$	Tage nach Blühbeginn	$`\small [d] `$<br>

Der Reduktionsfaktor ergibt sich aus dem kleinsten Wert für FH der während der sensitiven Phase auftritt.

$`\small \zeta_H = min(F_{H1},\, ... \,,F_{Hn} ) `$

$`\small \zeta_H `$	Reduktionsfaktor Hitzestress	 <br>
$`\small F_{H1}`$	Hitzeeinwirkung am ersten Tag der sensitiven Phase	$`\small [d^{-1}] `$<br>
$`\small F_{Hn}`$	Hitzeeinwirkung am letzten Tag der sensitiven Phase	$`\small [d^{-1}] `$<br>

Er reduziert die Assimilatzuweisung an das Speicherorgan.

$`\small W_s = A_g \cdot a_s \cdot \zeta_H`$

$`\small W_s`$	Biomasse des Speicherorgans	$`\small [kg \, TM \, ha^{-1}] `$<br>
$`\small A_g`$	Brutto-CO2-Assimilation	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small a_s`$	Assimilatverteilungskoeffizient für das Speicherorgan	 <br>
$`\zeta_H`$	Reduktionsfaktor Hitzestress	 <br>

## Literatur
Challinor et al. (2005): Simulation of the impact of high temperature stress on annual crop yields. Agricultural and Forest Meteorology 135, 180 - 189.

Mirschel, W. & Wenkel, K.-O., 2007. Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel and M. Wegehenkel (Editors), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, pp. 59- 74.

Moriondo et al. (2011): Climate change impact assessment: the role of climate extremes in crop yield simulation. Climatic Change 104 (3-4), 679-701.