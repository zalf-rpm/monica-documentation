# Assimilatverteilung

Die Assimilate, die aus dem Photosynthesemodul bereitgestellt werden, müssen auf die einzelnen Organe der Pflanze verteilt werden. Die Verteilungskoeffizienten werden einer Matrix entnommen, die über die Entwicklungsstadien der Pflanze gespannt wird.

Tabelle 1: Matrix der Assimilatverteilung am Beispiel Winterweizen.

&nbsp; | Wurzel | Blatt	Spross | Frucht | Dauerstruktur
------ | ------ | ------------ | ------ | -------------
Aussaat bis Auflaufen | 0.5 | 0.5 | 0 | 0 | 0
Auflaufen bis Doppelring | 0.2 | 0.2 | 0.6 | 0 | 0
Doppelring bis Blühbeginn | 0.13 | 0.2 | 0.67 | 0 | 0
Blüte | 0 | 0 | 0.03 | 0.97 | 0
Kornfüllung | 0 | 0 | 0 | 1.0 | 0
Seneszenz | 0 | 0 | 0 | 0 | 0

Die tagesaktuelle Verteilung wird mittels linearer Regression zwischen den Elementen der Matrix und dem relativen Entwicklungsstadium berechnet

$`\small a_i = a_{i,j} + ((a_{i, j+1} - a_{i,j}) \cdot DD_{rels})`$

und

$`\small DD_{rels} = \frac{DD_{acts}}{DD_{crops}}`$

$`\small a_i`$	Aktueller Assimilatverteilungskoeffizient für das Organ i<br>
$`\small a_{i,j}`$	Verteilungskoeffizient zu Beginn des Stadiums j<br>
$`\small a_{i,j+1}`$	Verteilungskoeffizient zu Beginn des folgenden Stadiums j+1<br>
$`\small DD_{rels}`$	relative phänologische Entwicklung innerhalb des Stadiums<br>
$`\small DD_{acts}`$	Aktuelle Temperatursumme im Entwicklungsstadium	$`\small [^{\circ}C \, d] `$<br>
$`\small DD_{crops}`$	Pflanzenspezifische Temperatursumme im Entwicklungsstadium	$`\small [^{\circ}C \, d] `$<br>
 
Die Anzahl der Organe und der Entwicklungsstadien sowie deren Benennung kann für jede Pflanze frei gewählt werden.

Der tägliche Assimilatzuwachs des jeweiligen Organs ergibt sich dann aus

$`\small \frac{d}{dt}W_i = A_g \cdot a_i \cdot dt `$

$`\small W_i`$	Biomasse des Organs i	$`\small [kg \, TM \, ha^{-1}] `$<br>
$`\small A_g`$	Brutto-CO2-Assimilation	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small a_i`$	Verteilungskoeffizient zu Beginn des Stadiums j<br>
$`\small t`$	Zeit	$`\small [d] `$<br>
 
Tägliche Massenabnahme aufgrund der Seneszenz des einzelnen Organs wird entsprechend berechnet.