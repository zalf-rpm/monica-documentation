# Blattfläche und Bedeckungsgrad

Die tägliche Änderung des Blattflächenindexes einer Pflanze wird in MONICA wie folgt berechnet

$`\small \frac{d}{dt} LAI = \frac{d}{dt} W_{L+} \cdot \left( \Phi_{L_s} + \frac{DD_{ads}}{DD_{crops}}  \cdot (\Phi_{L_e} - \Phi_{L_s}) \cdot dt \right)  -\frac{d}{dt}W_{L-} \cdot \Phi_{L_0} \cdot dt `$

$`\small LAI `$	Blattflächenindex	$`\small [m^2 \, m^{-2}] `$<br>
$`\small W_{L+}`$	Blattmasseninkrement	$`\small [kg \, TM \, m^{-2} \, d^{-1}] `$<br>
$`\small W_{L-}`$	Blattmassendekrement	$`\small [kg \, TM \, m^{-2} \, d^{-1}] `$<br>
$`\small \Phi_{L_s}`$	spezifische Blattfläche zu Beginn des Stadiums	$`\small [m^{2} \, kg \, TM^{-1}] `$<br>
$`\small \Phi_{L_e}`$	spezifische Blattfläche zum Ende des Stadiums	$`\small [m^{2} \, kg \, TM^{-1}] `$<br>
$`\small \Phi_{L_0}`$	Initiale spezifische Blattfläche	$`\small [m^{2} \, kg \, TM^{-1}] `$<br>
$`\small DD_{acts} `$	Aktuelle Temperatursumme im Entwicklungsstadium	$`\small [^{\circ}C \, d] `$<br>
$`\small DD_{crops}`$	Pflanzenspez. Temperatursumme im Entwicklungsstadium	$`\small [^{\circ}C \, d] `$<br>
$`\small t `$	Zeit	$`\small [d] `$<br>
 
Anschließend wird der Bedeckungsrad aus dem Blattflächenindex ermittelt.

$`\small \beta = 1 - e^{-0.5 \cdot LAI}`$

$`\small \beta `$	Bedeckungsgrad	$`\small [m^{2} \, m^{-2}] `$<br>
$`\small LAI`$	Blattflächenindex	$`\small [m^{2} \, m^{-2}] `$<br>