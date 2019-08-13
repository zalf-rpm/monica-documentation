# Atmung

Die Erhaltungsatmung wird für die Photoperiode und für Dunkelheit mit Hilfe von AGROSIM-Algorithmen (Mirschel und Wenkel, 2007) berechnet:

$`\small M_{photo} = \sum (W_i \cdot m_i) \cdot 2^{a\cdot(T_{photo} - b)} \cdot (2-L_N)`$

$`\small M_{dark} = \sum (W_i \cdot m_i) \cdot 2^{a\cdot(T_{dark} - b)} \cdot L_N`$

$`\small M_{photo} `$	Erhaltungsatmung in der Photoperiode	$`\small [Pa] `$<br>
$`\small M_{dark}`$	Erhaltungsatmung in der Dunkelheit	$`\small [Pa] `$<br>
$`\small W_i `$	Trockenmasse von Organkompartiment i	$`\small [kg \, m^{-2}] `$<br>
$`\small m_i `$	Spezifische Erhaltungsatmung von Organkompartiment i	$`\small [kg \, m^{-2}] `$<br>
$`\small T_{photo} `$	Mittlere Temperatur in der Photoperiode	$`\small [^{\circ} C] `$<br>
$`\small T_{dark} `$	Mittlere Temperatur in der Dunkelperiode	$`\small [^{\circ} C] `$<br>
$`\small a, b `$	Parameter	 <br>
$`\small L_N`$	Normierte Tageslänge	$`\small [d] `$<br>

mit

$`\small L_N = 2 - \left ( \frac{L_p}{12} \right ) `$

$`\small L_N `$	Normierte Tageslänge	$`\small [d] `$<br>
$`\small L_P`$	Photoaktive Tageslänge	$`\small [d] `$<br>

und

$`\small T_{photo} = T_{max} - \left ( \frac{T_{max} - T_{min}} {4}  \right ) `$

$`\small T_{dark} = T_{min} + \left ( \frac{T_{max} - T_{min}} {4}  \right ) `$

$`\small T_{photo}`$	Mittlere Temperatur in der Photoperiode	$`\small [^{\circ} C] `$<br>
$`\small T_{max}`$	Maximum-Tagestemperatur	$`\small [^{\circ} C] `$<br>
$`\small T_{min}`$	Minimum-Tagestemperatur	$`\small [^{\circ} C] `$<br>

Entsprechend wird die Wachstumsatmung berechnet.

Mit den aus der Photosynthese gebildeten Assimilaten wird zunächst prioritär die Erhaltungsatmung bedient, dann erst die Wachstumsatmung. Die Assimilate werden den Organ-Kompartimenten nach einer Partitionierungs-Matrix zugewiesen, welche nach den Ontogenesestadien der Pflanze gegliedert ist. Die Ontogenese der Pflanze wiederum wird aus der Summe der Gradtage berechnet und, wenn notwendig, für jedes Stadium durch den Tageslängen- und Vernalisationsanspruch modifiziert.

# Literatur

* Mirschel, W. und K.-O. Wenkel (2007): Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel und M. Wegehenkel (Hrsg.), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, 59-74.