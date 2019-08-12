# N-Konzentration
Um die Wirkung mangelnder N-Versorgung auf die Pflanze ermitteln zu können, wird die kritische N-Konzentration in der Pflanze als Funktion ihrer relativen Entwicklung verwendet.

$`\small N_{crit} = a \cdot (1.0 + b \cdot e^{-0.26 \cdot W})`$

$`\small N_{crit}`$	Kritische N-Konzentration in der Pflanze	$`\small [kg \, N \, kg DM^{-1}] `$<br>
$`\small W`$	Oberirdische Frischmasse	$`\small [t] `$<br>
$`\small a, b`$	Pflanzenspezifische Parameter	 <br>

W ist die oberirdische Frischmasse der Pflanze, a und b sind pflanzenspezifische Parameter (Rahn et al., 2010). Das Konzept baut auf den Ideen von Greenwood auf (1990). Die optimale N-Konzentration wird durch Multiplikation der kritischen N-Konzentration mit einem Luxus-N-Versorgungsfaktor erhalten, welcher dem EU-Rotate_N-Modell (Rahn et al., 2010) entnommen wurde.

$`\small N_{target} = N_{crit} \cdot k_{lux}`$

$`\small N_{target}`$	Optimale N-Konzentration in der Pflanze	$`\small [kg \, N \, kg DM^{-1}] `$<br>
$`\small N_{crit}`$	Kritische N-Konzentration in der Pflanze	$`\small [kg \, N \, kg DM^{-1}] `$<br>
$`\small k_{lux}`$	Luxus-N-Versorgungsfaktor	 <br>

Die N-Konzentration der Pflanze wird in jedem Tagesschritt neu berechnet. Die Differenz zwischen der aktuellen und der neuen optimalen N-Konzentration wird zur Aufnahme aus dem Boden angefragt. Ist das Dargebot im Boden ausreichend, wird die Differenz ergänzt. Ansonsten fällt die N-Konzentration in der Pflanze.

Unterschreitet sie die kritische N-Konzentration, wird die Photosyntheseleistung reduziert.

## Literatur
Greenwood, D.J. et al. (1990): Weather, nitrogen supply and growth rate of field vegetables. Plant Soil 124 (2), 297-301.

Rahn, C.R. et al. (2010): EU-Rotate_N - a European decision support system - to predict environmental and economic consequences of the management of nitrogen fertiliser in crop rotations. European Journal of Horticultural Science 75 (1), 20-32