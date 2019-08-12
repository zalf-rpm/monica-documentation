# Sauerstoffstress

Die Pflanze wird im Modell MONICA durch vier Stressfaktoren im Wachstum beeinträchtigt: Wassermangel, Stickstoffmangel, Sauerstoffmangel und Hitzestress.

Der Mangel an Sauerstoff im durchwurzelten Raum des Bodens wird über das Verhältnis des Luftgehalts im Boden zu einem kritischen Luftgehalt bestimmt. Dabei wird angenommen, dass die Abnahme der Sauerstoffkonzentration im verbleibenden Luftvolumen nach vier Tagen zu maximalen Mangelerscheinungen führt.

$`\small \zeta_O = 1- \left( \frac{t_{anox}} {4} \right)  \cdot (1 - \zeta_{O_{max}}) `$

$`\small \zeta_O`$	Reduktionsfaktor Sauerstoffstress	 <br>
$`\small t_{anox}`$	Zeit unter Luftmangel (max = 4)	$`\small [d] `$<br>
$`\small \zeta_{O_{max}} `$	Maximales Sauerstoffdefizit	 <br>

mit

$`\small \zeta_{O_{max}} = \left( \frac{\varepsilon}{\varepsilon_{crit}} \right)    \,\,\,\,\,\,\,\,\,\,\,\,\, \varepsilon < \varepsilon_{crit}`$

$`\small \zeta_{O_{max}} `$	Maximales Sauerstoffdefizit	 <br>
$`\small \varepsilon`$	Luftgehalt im Boden	$`\small [m^3 m^{-3}] `$<br>
$`\small \varepsilon_{crit}`$	Pflanzenspezifischer kritischer Luftgehalt im Boden	$`\small [m^3 m^{-3}] `$<br>

Der Stressfaktor Sauerstoffmangel verringert die Wasseraufnahme der Wurzel.
 
## Literatur
Challinor et al. (2005): Simulation of the impact of high temperature stress on annual crop yields. Agricultural and Forest Meteorology 135, 180 - 189.

Mirschel, W. & Wenkel, K.-O., 2007. Modelling soil-crop interactions with AGROSIM model family. In: K.C. Kersebaum, J.-M. Hecker, W. Mirschel and M. Wegehenkel (Editors), Modelling water and nutrient dynamics in soil crop systems. Springer, Stuttgart, pp. 59- 74.

Moriondo et al. (2011): Climate change impact assessment: the role of climate extremes in crop yield simulation. Climatic Change 104 (3-4), 679-701.