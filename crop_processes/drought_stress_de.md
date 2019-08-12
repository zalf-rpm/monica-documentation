# Trockenstress
Die Pflanze wird im Modell MONICA durch vier Stressfaktoren im Wachstum beeinträchtigt: Wassermangel, Stickstoffmangel, Sauerstoffmangel und Hitzestress.

Wassermangel zeichnet sich dadurch aus, dass die potenzielle Transpiration eine Größe annimmt, die durch das im Boden vorhandene Wasser nicht gedeckt werden kann. Das Verhältnis der aktuellen zur potenziellen Transpiration bildet den Reduktionsfaktor für Trockenstress.

$`\small \zeta_W = \frac{T_a}{T_p}`$

$`\small \zeta_W`$	Reduktionsfaktor Trockenstress	

$`\small T_a`$	Aktuelle Transpiration	$`\small [mm] `$<br>
$`\small T_p`$	Potenzielle Transpiration	$`\small [mm] `$<br>

Der Wasserstress greift im Modell an der Bereitstellung der Assimilate aus der Photosynthese, sobald der Reduktionsfaktor einen für jede Pflanzenart und deren Entwicklungsstadien spezifischen Schwellenwert unterschreitet.

$`\small A_{gm} = A_g \cdot \zeta_w \,\,\,\,\,\,\,\,\,\,\,\,  \zeta_w < \bar{\zeta}_w`$

$`\small A_{gm}`$	Assimilatmenge	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small A_g`$	Brutto-CO2-Assimilation	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small \zeta_w`$	Reduktionsfaktor Trockenstress	 <br>
$`\small \bar{\zeta}_w`$	Pflanzen- und stadien-spezifischer Schwellenwert für Trockenstress	 <br>

Im Falle von Trockenstress wird für bestimmte Entwicklungsstadien außerdem eine Beschleunigung der Ontogenese angenommen.

## Literatur
Challinor et al. (2005): Simulation of the impact of high temperature stress on annual crop yields. Agricultural and Forest Meteorology 135, 180 - 189.

Mirschel, W. & Wenkel, K.-O., 2007. Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel and M. Wegehenkel (Editors), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, pp. 59- 74.

Moriondo et al. (2011): Climate change impact assessment: the role of climate extremes in crop yield simulation. Climatic Change 104 (3-4), 679-701.