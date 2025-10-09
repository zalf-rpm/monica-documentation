# Photosynthese

Die Modellierung des Pflanzenwachstums folgt einem generischen Ansatz, der auf dem SUCROS- Modell (van Keulen et al. 1982) basiert. Die tägliche Netto-Produktion der Trockenmasse durch Photosynthese und Atmung wird durch die Globalstrahlung und Temperatur angetrieben.

Die Berechnung der Brutto-CO2-Assimilation erfolgt mittels der Abschätzung der Bewölkungszeit

$`\small A_g = O_r \cdot A_0 + (1-O_r) \cdot A_c `$

$`\small A_g`$	Brutto-CO2-Assimilationsrate	$`\small [kg\ , CO_2 \, ha^{-1} \, h^{-1}] `$<br>
$`\small O_r`$	Relative Bewölkungszeit	$`\small [d \, d^{-1}] `$<br>
$`\small A_0`$	CO2-Assimilation bei Bewölkung	$`\small [kg\ , CO_2 \, ha^{-1} \, h^{-1}] `$<br>
$`\small A_c`$	CO2-Assimilation bei klarem Himmel	$`\small [kg\ , CO_2 \, ha^{-1} \, h^{-1}] `$<br>

mit

$`\small O_r = \frac{R_c - (0.5 \cdot R_s \cdot 10^9)} {0.8 \cdot R_c}`$

$`\small O_r`$	relative Bewölkungszeit	$`\small [d \, d^{-1}] `$<br>
$`\small R_s`$	Globalstrahlung	$`\small [MJ \, m^2 \, d^{-1}] `$<br>
$`\small R_c`$	Einstrahlung bei klarem Himmel	$`\small [J \, m^2] `$<br>

Die Brutto-CO2-Assimilation setzt sich dabei zusammen aus der Assimilation bei Bewölkung und bei klarem Himmel.

$`\small A_c = \begin{cases}  PHCL & LAI < 5  \\ PHCH & LAI \geq 5 \end{cases}`$

$`\small A_O = \begin{cases}  PHOL & LAI < 5 \\ PHOH & LAI \geq 5 \end{cases}`$

$`\small A_O`$	CO2-Assimilation bei Bewölkung	$`\small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$<br>
$`\small A_C`$	CO2-Assimilation bei klarem Himmel	$`\small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$<br>
$`\small LAI`$	Blattflächenindex	$`\small [m^2 \, m^{-2}] `$<br>

Dazu werden folgende Hilfsalgorithmen verwendet:

$`\small PHCL = \begin{cases} PHC3 \cdot \left(  1-e^{\frac{PHC4}{PHC3}}  \right) & PHC3 < PHC4 \\ PHC4 \cdot \left(  1-e^{\frac{PHC3}{PHC4}}\right) & PHC3 \geq PHC4 \end{cases} `$

$`\small PHCH = 0.95 \cdot (PHCH1 + PHCH2) + 20.5 `$

$`\small PHOL = \begin{cases} PHO3 \cdot \left(  1-e^{\frac{PHC4}{PHO3}}  \right) & PHO3 < PHC4 \\ PHC4 \cdot \left(  1-e^{\frac{PHO3}{PHC4}}\right) & PHO3 \geq PHC4 \end{cases} `$

$`\small PHOH = 0.9935 \cdot PHOH1 + 1.1`$

mit

$`\small PHC3 = PHCH \cdot (1-e^{-0.8 \cdot LAI})`$

$`\small PHC4 = N_{atmo} \cdot LAI \cdot A `$

$`\small N_{atmo} `$	Atmosphärische Tageslänge	$`\small [h] `$<br>
$`\small LAI `$	Blattflächenindex	$`\small [m^2 \, m^{-2}] `$<br>
$`\small A`$	CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$<br>

$`\small PHO3 = PHOH \cdot (1-e{-0.8 \cdot LAI}) `$

$`\small PHCH1 = h_p \cdot A \cdot N_{eff} \cdot \frac{X}{1+X}`$

$`\small LAI `$	Blattflächenindex	$`\small [m^2 \, m^{-2}] `$<br>
$`\small h_p`$	Sonnenhöchststand (senkrechte Projektion)	$`\small [^{\circ}] `$<br>
$`\small A`$	CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$<br>
$`\small N_{eff}`$	Effektive Tageslänge	$`\small [h] `$<br>

$`\small X = log \left( \frac{1+0.45 \cdot R_c} {N_{eff} \cdot 3600} \right)  \cdot \frac{\varepsilon_N}{h_p \cdot A}  `$

$`\small R_c`$	Einstrahlung bei klarem Himmel	$`\small [J \, m^{-2}] `$<br>
$`\small \varepsilon_N`$	Netto-Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$<br>
$`\small N_{eff}`$	Effektive Tageslänge	$`\small [h] `$<br>
$`\small h_p`$	Sonnenhöchststand (senkrechte Projektion)	$`\small [^{\circ}] `$<br>
$`\small A`$	CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$<br>

$`\small PHCH_2 = (5-h_p) \cdot A \cdot N_{eff} \cdot \frac{Y}{1+Y}`$

$`\small h_p`$	Sonnenhöchststand (senkrechte Projektion)	$`\small [^{\circ}] `$<br>
$`\small A`$	CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$<br>
$`\small N_{eff}`$	Effektive Tageslänge	$`\small [h] `$<br>

$`\small Y=log \left(  \frac{1+0.55 \cdot R_c}{N_{eff} \cdot 3600} \right) \cdot \frac{\varepsilon_N}{(5-h_p) \cdot A}`$

$`\small A`$	CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$<br>
$`\small \varepsilon_N`$	Netto-Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$<br>
$`\small R_c `$	Einstrahlung bei klarem Himmel $`\small [J \, m^{-2}] `$<br>
$`\small h_p `$	Sonnenhöchststand (senkrechte Projektion)	$`\small [^{\circ}] `$<br>

$`\small PHOH1 = 5 \cdot A \cdot \varepsilon_N \cdot \frac{Z}{1+Z}`$

$`\small A`$	CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$<br>
$`\small \varepsilon_N`$	Netto-Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$<br>

$`\small Z = \frac{R_0}{N_{eff} \cdot 3600} \cdot \frac{\varepsilon_N}{5\cdot A}`$

$`\small Z`$<br>	
$`\small N_{eff}`$	Effektive Tageslänge	$`\small [h] `$<br>
$`\small \varepsilon_N `$	Netto-Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$<br>
$`\small A`$	Netto-Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$<br>

$`\small  h_p = \sin(90 + \delta + \varphi) \cdot \left( \frac{\pi}{180}\right)`$

$`\small h_p`$	Sonnenhöchststand (senkrechte Projektion)	$`\small [rad] `$<br>
$`\small \delta`$	Solare Deklination	$`\small [^{\circ}] `$<br>
$`\small \varphi`$	Breitengrad	$`\small [^{\circ}] `$<br>

$`\small \varepsilon_N = (1-\alpha_c) \cdot \varepsilon_L`$

$`\small \varepsilon_N `$	Netto-Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$<br>
$`\small \alpha_c`$	Albedo des Pflanzenbestandes<br>
$`\small \varepsilon_L `$	Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$<br>

$`\small N_atmo = 12 \cdot \frac{ \left( \pi + 2 \cdot \arcsin \left( \frac{\delta_{sin}}{\delta{cos}} \right) \right)}{\pi} `$

$`\small N_{atmo}`$	Atmosphärische Tageslänge	$`\small [h] `$<br>

$`\small N_{eff} = 12 \cdot \frac{\left(\pi + 2 \cdot \arcsin \left( \frac{(-\sin(\frac{8 \cdot \pi}{180}) + \delta{sin})}{\delta_{cos}}   \right)  \right)}{\pi}`$

$`\small N_{eff}`$	Effektive Tageslänge	$`\small [h] `$<br>

$`\small N_{photo} = 12 \cdot \frac{\left(  \pi + 2 \cdot \arcsin \left( \frac{(-\sin(\frac{-6 \cdot \pi}{180}) + \delta_{sin})} {\delta_{cos}} \right)\right)}{\pi}`$

$`\small N_{photo} `$	Photoperiodische Tageslänge	$`\small [h] `$<br>

$`\small R_c = 0.5 \cdot 1300 \cdot \bar{R}_{photo} \cdot e^{\frac{-0.14}{\bar{R}_{photo}}}`$

$`\small R_c`$	Einstrahlung bei klarem Himmel	$`\small [J \, m^{-2}] `$<br>

$`\small R_o = 0.2 \cdot R_c`$

$`\small R_o`$	Einstrahlung bei bewölktem Himmel	$`\small [J \, m^{-2}] `$<br>

$`\small \bar{R}_{photo}  = 3600 \cdot \left(  \delta_{sin} \cdot \N_{astro} + \frac{24}{\pi} \cdot \delta_{cos}  \cdot \sqrt{\left(1-\left(\frac{\delta_{sin}}{\delta_{cos}}\right)^2\right)} \,\,\,\right)`$

$`\small \bar{R}_{photo}`$	Mittlere photosynthesewirksame Strahlung	$`\small [J \, m^{-2}] `$<br>

[CO2] beeinflusst die Photosynthese-Rate und den Stomata-Widerstand der Pflanze, der wiederum auf die Transpiration wirkt (Nendel et al. 2009). Zunächst präsentierten Mitchell et al. (1995) einen Algorithmensatz  für die Berechnung der maximalen Photosynthese-Rate basierend auf den Ideen von Farquhar und von Caemmerer (1982) und Long (1991)

$`\small A =  \frac{( C_i - \Gamma^{*}) \cdot V_{c_{max}}}{C_i + K_c \cdot \left(  1 + \frac{O_i}{K_o} \right)} `$

$`\small A`$	CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small C_i `$	Interzellulare CO2-Konzentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small \Gamma^{*}`$	Kompensationspunkt der Photosynthese, bezogen auf Ci in Abwesenheit der Dunkelatmung	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small O_i`$	Interzellulare O2-Konzentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small v_{c_{max}}`$	Maximale gesättigte Carboxylierungsrate der Rubisco	$`\small [\mu mol \, m^{-2} \, s^{-1}] `$<br>
$`\small K_c`$	Michaelis-Menten-Konstante für CO2	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small K_o`$	Michaelis-Menten-Konstante für O2	$`\small [\mu mol \, mol^{-1}] `$<br>

Die Abhängigkeiten von Ci, Oi, Kc, K0 und Vcmax und ihrer Parameter von der Temperatur wurden von Long (1991) beschrieben. Ci wird demnach aus der atmosphärischen  CO2-Konzentration Ca berechnet nach

$`\tiny C_i = C_a \cdot 0.7 \cdot \frac{(1.674 - 6.1294 \cdot 10^{-2} \cdot T + 1.1688 \cdot 10^{-3} \cdot T^2 - 8.8741 \cdot 10^{-7} \cdot T^3 )}{0.73547}`$

$`\small C_i`$	Interzellulare CO2-Konzentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small T`$	Mittlere Tageslufttemperatur in 2m Höhe	$`\small [^{\circ} C] `$<br>

Entsprechend wird Oi berechnet nach

$`\tiny O_i = 210 + \frac {( 0.047 - 1.3087 \cdot 10^{-4} \cdot T + 2.5603 \cdot 10^{-6} \cdot T^2 - 2.1441 \cdot 10^{-8} \cdot T^3 )}  {2.6934 \cdot 10^{-2}}`$

$`\small O_i`$	Interzellulare O2-Konzentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small T `$	Mittlere Tageslufttemperatur in 2m Höhe	$`\small [^{\circ} C] `$<br>

Die saisonale Dynamik der atmosphärischen CO2-Konzentration von 1958 bis heute wird mit folgender Funktion beschrieben

$`\small C_a = 222 + e ^{0.0119 \cdot (t_{dec} - 1580)} + 2.5 \cdot \sin \left( \frac{t_{dec} - 0.5} {0.1592} \right)`$

$`\small C_a`$	Atmosphärische CO2-Konzentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small t_{dec} `$	Datum in Dezimalform<br>

Der Algorithmus für Lichtintensitäten unterhalb der Sättigung, den Mitchell et al. (1995) vorstellten, wurden nicht angewendet. Stattdessen wird Amax an die  Licht-Interzeption nach dem Vorbild von Goudriaan und van Laar (1978) angepasst. Für die Transition zwischen photosynthetischer Quanten-Nutzungseffizienz und lichtgesättigter Photosynthese schlugen Mitchell et al. (1995) folgenden Algorithmus vor:

$`\small \varepsilon_L = \frac{0.37 \cdot (C_i - \Gamma^{*})} {4.5 \cdot C_i + 10.5 \cdot \Gamma^{*}}`$

$`\small \varepsilon_L`$	Strahlungsnutzungseffizienz der CO2-Assimilation	$`\small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$<br>
$`\small C_i`$	Interzellulare CO2-Konzentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small \Gamma^{*} `$	Kompensationspunkt der Photosynthese, bezogen auf Ci in Abwesenheit der Dunkelatmung	$`\small [\mu mol \, mol^{-1}] `$<br>

Der Kompensationspunkt der Photosynthese ergibt sich aus

$`\small \Gamma^{*} = \frac{0.5 \cdot 0.21 \cdot V_{c_{max}} \cdot O_i} {V_{c_{max}} \cdot K_o} `$

$`\small C_i`$	Interzellulare CO2-Konzentration	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small \Gamma^{*}`$	Kompensationspunkt der Photosynthese, bezogen auf Ci in Abwesenheit der Dunkelatmung	$`\small [\mu mol \, mol^{-1}] `$<br>
$`\small V_{c_{max}} `$	Maximale gesättigte Carboxylierungsrate der Rubisco	$`\small [\mu mol \, m^{-2} \, s^{-1}] `$<br>
$`\small K_o `$	Michaelis-Menten-Konstante für O2	$`\small [\mu mol \, mol^{-1}] `$<br>

Die maximale gesättigte Carboxylierungsrate der Rubisco Vcmax wird berechnet aus

$`\small V_{c_{max}} = 98 \cdot \frac{A_{max}} {34.668} \cdot k(T)_{v_{cmax}}`$

$`\small V_{c_{max}}`$	Maximale gesättigte Carboxylierungsrate der Rubisco	$`\small [\mu mol \, m^{-2} \, s^{-1}] `$<br>
$`\small A_{max} `$	Pflanzenspezifische maximale CO2-Assimilationsrate	$`\small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$<br>
$`\small k(T)_{v_{cmax}} `$	Temperaturfunktion für Vcmax<br>

![](../images/crop_processes/MONICA_Photosynthese_Temperature_KTvmax.png)<br>
Abbildung 1: Temperaturfunktion für Vcmax (Long 1991).

Für Pflanzen, die einen C4-Stoffwechsel betreiben, wird kein direkter Einfluss der atmosphärischen CO2-Konzentration auf die Photosynthese angenommen. Die pflanzenspezifische maximale CO2-Assimilationsrate wird lediglich durch eine einfache Temperaturfunktion modifiziert.
 
![](../images/crop_processes/MONICA_Photosynthese_Temperature_C4.png)<br>
Abbildung 2: Temperaturfunktion der CO2-Assimilationsrate für C4-Pflanzen (Sage & Kubien 2007).

# Literatur

* Farquhar, G. D. & von Caemmerer, S. (1982). Modelling of photosynthetic response to environmental conditions. In: Encyclopedia of plant physiology. New series. Volume 12B. Physiological plant ecology. II. Water relations and carbon assimilation. (Eds O. L. Lange, P. S. Nobel, C. B. Osmond, & H. Ziegler), pp. 549-587. Berlin: Springer.

* Goudriaan, J. & van Laar, H. H. (1978). Relations between leaf resistance, CO2 concentration and CO2 assimilation in maize, beans, lalang grass and sunflower. Photosynthetica 12 (3), 241-249.

* Long, S. P. (1991). Modification of the response of photosynthetic productivity to rising temperature by atmospheric CO2 concentrations - Has its importance been underestimated. Plant Cell and Environment 14 (8), 729-739.

* Mitchell, R. A. C., Lawlor, D. W., Mitchell, V. J., Gibbard, C. L., White, E. M., & Porter, J. R. (1995). Effects of elevated CO2 concentration and increased temperature on winter-wheat - Test of ARCWHEAT1 simulation model. Plant Cell and Environment 18 (7), 736-748.

* Nendel, C., Kersebaum, K. C., Mirschel, W., Manderscheid, R., Weigel, H. J., & Wenkel, K.-O. (2009). Testing different CO2 response algorithms against a FACE crop rotation experiment. NJAS - Wageningen Journal of Life Sciences 57 (1), 17-25.

* Sage, R. F. & Kubien, D. S. (2007). The temperature response of C3 and C4 photosynthesis. Plant Cell and Environment 30 (9), 1086-1106.

* van Keulen, H., Penning de Vries, F. W. T., & Drees, E. M. (1982). A summary model for crop growth. In: Simulation of plant growth and crop production (Eds F. W. T. Penning de Vries & H. H. van Laar), pp. 87-97. Wageningen: PUDOC.