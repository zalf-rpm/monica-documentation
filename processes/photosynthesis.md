# Photosynthesis
Modelling of crop growth follows a generic approach used with the SUCROS model (van Keulen et al. 1982). Daily net dry matter production by photosynthesis and respiration is driven by radiation and temperature. Gross CO2 assimilation is calculated by estimating the sky cover duration.

 

$` \small A_g = O_r \cdot A_0 + (1-O_r) \cdot A_c `$

 

$` \small A_g`$	Gross CO2 assimilation rate	$` \small [kg\ , CO_2 \, ha^{-1} \, h^{-1}] `$
$` \small O_r`$	Relative sky cover duration	$` \small [d \, d^{-1}] `$
$` \small A_0`$	CO2 assimilation under clouded sky	$` \small [kg\ , CO_2 \, ha^{-1} \, h^{-1}] `$
$` \small A_c`$	CO2 assimilation under clear sky	$` \small [kg\ , CO_2 \, ha^{-1} \, h^{-1}] `$
 

where

 

$` \small O_r = \frac{R_c - (0.5 \cdot R_s \cdot 10^9)} {0.8 \cdot R_c}`$

 

$` \small O_r`$	Relative sky cover duration	$` \small [d \, d^{-1}] `$
$` \small R_s`$	Global radiation	$` \small [MJ \, m^2 \, d^{-1}] `$
$` \small R_c`$	Irradiation under clear sky	$` \small [J \, m^2] `$
 

Gross CO2 assimilation includes assimilation under clouded and under clear sky conditions.

 

$` \small A_c = \begin{cases}  PHCL & LAI < 5  \\ PHCH & LAI \geq 5 \end{cases}`$

 

$` \small A_O = \begin{cases}  PHOL & LAI < 5 \\ PHOH & LAI \geq 5 \end{cases}`$

 

$` \small A_O`$	CO2 assimilation under clouded sky	$` \small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$
$` \small A_C`$	CO2 assimilation under clear sky	$` \small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$
$` \small LAI`$	Leaf area index	$` \small [m^2 \, m^{-2}] `$
 

The following auxiliary algorithms were used:

 

$` \small PHCL = \begin{cases} PHC3 \cdot \left(  1-e^{\frac{PHC4}{PHC3}}  \right) & PHC3 < PHC4 \\ PHC4 \cdot \left(  1-e^{\frac{PHC3}{PHC4}}\right) & PHC3 \geq PHC4 \end{cases} `$

 

$` \small PHCH = 0.95 \cdot (PHCH1 + PHCH2) + 20.5 `$

 

$` \small PHOL = \begin{cases} PHO3 \cdot \left(  1-e^{\frac{PHC4}{PHO3}}  \right) & PHO3 < PHC4 \\ PHC4 \cdot \left(  1-e^{\frac{PHO3}{PHC4}}\right) & PHO3 \geq PHC4 \end{cases} `$

 

$` \small PHOH = 0.9935 \cdot PHOH1 + 1.1`$

 

where

 

$` \small PHC3 = PHCH \cdot (1-e^{-0.8 \cdot LAI})`$

 

$` \small PHC4 = N_{atmo} \cdot LAI \cdot A `$

 

 

$` \small N_{atmo} `$	Atmospheric day length	$` \small [h] `$
$` \small LAI `$	Leaf area index	$` \small [m^2 \, m^{-2}] `$
$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$
 

$` \small PHO3 = PHOH \cdot (1-e{-0.8 \cdot LAI}) `$

 

$` \small PHCH1 = h_p \cdot A \cdot N_{eff} \cdot \frac{X}{1+X}`$

 

 

$` \small LAI `$	Leaf area index	$` \small [m^2 \, m^{-2}] `$
$` \small h_p`$	Sun’s culmination (vertical projection)	$` \small [^{\circ}] `$
$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$
$` \small N_{eff}`$	Effective day length	$` \small [h] `$
 

$` \small X = log \left( \frac{1+0.45 \cdot R_c} {N_{eff} \cdot 3600} \right)  \cdot \frac{\varepsilon_N}{h_p \cdot A}  `$

 

$` \small R_c`$	Irradiation under clear sky	$` \small [J \, m^{-2}] `$
$` \small \varepsilon_N`$	Net radiation use efficiency of CO2´assimilation	$` \small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$
$` \small N_{eff}`$	Effective day length	$` \small [h] `$
$` \small h_p`$	Sun’s culmination (vertical projection)	$` \small [^{\circ}] `$
$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, h^{1}] `$
 

$` \small PHCH_2 = (5-h_p) \cdot A \cdot N_{eff} \cdot \frac{Y}{1+Y}`$

 

$` \small h_p`$	Sun’s culmination (vertical projection)	$` \small [^{\circ}] `$
$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$
$` \small N_{eff}`$	Effective day length	$` \small [h] `$
 

$` \small Y=log \left(  \frac{1+0.55 \cdot R_c}{N_{eff} \cdot 3600} \right) \cdot \frac{\varepsilon_N}{(5-h_p) \cdot A}`$

 

$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$
$` \small \varepsilon_N`$	Net radiation use efficiency of CO2 assimilation	$` \small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$
$` \small R_c `$	Irradiation under clear sky
 

$` \small [J \, m^{-2}] `$
$` \small h_p `$	Sun’s culmination (vertical projection)	$` \small [^{\circ}] `$
 

$` \small PHOH1 = 5 \cdot A \cdot \varepsilon_N \cdot \frac{Z}{1+Z}`$

 

$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, h^{-1}] `$
$` \small \varepsilon_N`$	Net radiation use efficiency of CO2 assimilation	$` \small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$
 

$` \small Z = \frac{R_0}{N_{eff} \cdot 3600} \cdot \frac{\varepsilon_N}{5\cdot A}`$

 

$` \small Z`$	 	 
$` \small N_{eff}`$	Effective day length	$` \small [h] `$
$` \small \varepsilon_N `$	Net radiation use efficiency of CO2 assimilation	$` \small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$
$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$
 

$` \small  h_p = \sin(90 + \delta + \varphi) \cdot \left( \frac{\pi}{180}\right)`$

 

$` \small h_p`$	Sun’s culmination (vertical projection)	$` \small [rad] `$
$` \small \delta`$	Solar declination	$` \small [^{\circ}] `$
$` \small \varphi`$	Latitude	$` \small [^{\circ}] `$
 

$` \small \varepsilon_N = (1-\alpha_c) \cdot \varepsilon_L`$

 

$` \small \varepsilon_N `$	Net radiation use efficiency of CO2 assimilation	$` \small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$
$` \small \alpha_c`$	Crop albedo	 
$` \small \varepsilon_L `$	Radiation use efficiency of CO2 assimilation	$` \small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$
 

$` \small N_atmo = 12 \cdot \frac{ \left( \pi + 2 \cdot \arcsin \left( \frac{\delta_{sin}}{\delta{cos}} \right) \right)}{\pi} `$

 

$` \small N_{atmo}`$	Atmospheric day length	$` \small [h] `$
 

$` \small N_{eff} = 12 \cdot \frac{\left(\pi + 2 \cdot \arcsin \left( \frac{(-\sin(\frac{8 \cdot \pi}{180}) + \delta{sin})}{\delta_{cos}}   \right)  \right)}{\pi}`$

 

$` \small N_{eff}`$	Effective day length	$` \small [h] `$
 

$` \small N_{photo} = 12 \cdot \frac{\left(\pi + 2 \cdot \arcsin \left( \frac{(-\sin(\frac{-6 \cdot \pi}{180}) + \delta_{sin})} {\delta_{cos}} \right)\right)}{\pi}`$


$` \small N_{photo} `$	Photoperiodic day length	$` \small [h] `$
 

$` \small R_c = 0.5 \cdot 1300 \cdot \bar{R}_{photo} \cdot e^{\frac{-0.14}{\bar{R}_{photo}}}`$

 

$` \small R_c`$	Irradiation under clear sky	$` \small [J \, m^{-2}] `$
 

$` \small R_o = 0.2 \cdot R_c`$

 

$` \small R_o`$	Irradiation under clouded sky	$` \small [J \, m^{-2}] `$
 

$` \small \bar{R}_{photo}  = 3600 \cdot \left(  \delta_{sin} \cdot \N_{astro} + \frac{24}{\pi} \cdot \delta_{cos}  \cdot \sqrt{\left(1-\left(\frac{\delta_{sin}}{\delta_{cos}}\right)^2\right)} \,\,\,\right)`$

 

$` \small \bar{R}_{photo}`$	Mittlere photosynthesewirksame Strahlung	$` \small [J \, m^{-2}] `$
 

[CO2] has an impact on the crop’s photosynthesis rate and stomata resistance, which in turn influences transpiration (Nendel et al. 2009). Mitchell et al. (1995) presented a set of algorithms for the calculation of the maximum photosynthesis rate, based on ideas of Farquhar and von Caemmerer (1982) and Long (1991)

 

$` \small A =  \frac{( C_i - \Gamma^{*}) \cdot V_{c_{max}}}{C_i + K_c \cdot \left(  1 + \frac{O_i}{K_o} \right)} `$

 

$` \small A`$	CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$
$` \small C_i `$	Inter-cellular CO2 concentration	$` \small [\mu mol \, mol^{-1}] `$
$` \small \Gamma^{*}`$	Compensation point of photosynthesis, related to Ci in absence of dark respiration	$` \small [\mu mol \, mol^{-1}] `$
$` \small O_i`$	Inter-cellular O2 concentration	$` \small [\mu mol \, mol^{-1}] `$
$` \small v_{c_{max}}`$	Maximum saturated Rubisco carboxylation rate	$` \small [\mu mol \, m^{-2} \, s^{-1}] `$
$` \small K_c`$	Michaelis-Menten constant for CO2	$` \small [\mu mol \, mol^{-1}] `$
$` \small K_o`$	Michaelis-Menten constant for O2	$` \small [\mu mol \, mol^{-1}] `$
 

Temperature dependencies of Ci, Oi, Kc, K0 and Vcmax and ist parameters were described by Long (1991). Accordingly, Ci is calculated from atmospheric CO2 concentration Ca as

 

$` \tiny C_i = C_a \cdot 0.7 \cdot \frac{(1.674 - 6.1294 \cdot 10^{-2} \cdot T + 1.1688 \cdot 10^{-3} \cdot T^2 - 8.8741 \cdot 10^{-7} \cdot T^3 )}{0.73547}`$

 

$` \small C_i`$	Inter-cellular CO2 concentration	$` \small [\mu mol \, mol^{-1}] `$
$` \small T`$	Daily mean air temperature in 2m height	$` \small [^{\circ} C] `$
 

Respectively Oi is calculated by

 

$` \tiny O_i = 210 + \frac {( 0.047 - 1.3087 \cdot 10^{-4} \cdot T + 2.5603 \cdot 10^{-6} \cdot T^2 - 2.1441 \cdot 10^{-8} \cdot T^3 )}  {2.6934 \cdot 10^{-2}}`$

 

$` \small O_i`$	Inter-cellular O2 concentration	$` \small [\mu mol \, mol^{-1}] `$
$` \small T `$	Daily mean air temperature in 2m height	$` \small [^{\circ} C] `$
 

The seasonal dynamic of the atmosphericn CO2 concentration from 1958 until today is described using

 

$` \small C_a = 222 + e ^{0.0119 \cdot (t_{dec} - 1580)} + 2.5 \cdot \sin \left( \frac{t_{dec} - 0.5} {0.1592} \right)`$

 

$` \small C_a`$	Atmospheric CO2 concentration	$` \small [\mu mol \, mol^{-1}] `$
$` \small t_{dec} `$	Date in decimal form	 
 

The algorithm used for light intensities below saturation which Mitchell et al. (1995) presented, was not applied. Instead, Amax is adapted to light interception according to Goudriaan and van Laar (1978). Mitchell et al. (1995) proposed the following algorithm for the transition between photosynthetic quantum use efficiency and light-saturated photosynthesis:

 

 

$` \small \varepsilon_L = \frac{0.37 \cdot (C_i - \Gamma^{*})} {4.5 \cdot C_i + 10.5 \cdot \Gamma^{*}}`$

 

$` \small \varepsilon_L`$	Radiation use efficiency of CO2 assimilation	$` \small [kg \, CO_2 \, J^{-1} \, ha^{-1} \, h^{-1}] `$
$` \small C_i`$	Inter-cellular CO2 concentration	$` \small [\mu mol \, mol^{-1}] `$
$` \small \Gamma^{*} `$	Compensation point of photosynthesis, related to Ci in absence of dark respiration	$` \small [\mu mol \, mol^{-1}] `$
 

The compensation point of photosynthesis is obtained from

 

 

$` \small \Gamma^{*} = \frac{0.5 \cdot 0.21 \cdot V_{c_{max}} \cdot O_i} {V_{c_{max}} \cdot K_o} `$

 

$` \small C_i`$	Inter-cellular CO2 concentration	$` \small [\mu mol \, mol^{-1}] `$
$` \small \Gamma^{*}`$	Compensation point of photosynthesis, related to Ci in absence of dark respiration	$` \small [\mu mol \, mol^{-1}] `$
$` \small V_{c_{max}} `$	Maximum saturated Rubisco carboxylation rate	$` \small [\mu mol \, m^{-2} \, s^{-1}] `$
$` \small K_o `$	Michaelis-Menten constant for O2	$` \small [\mu mol \, mol^{-1}] `$
 

The maximum saturated Rubisco carboxylation rate Vcmax is calculated from

 

$` \small V_{c_{max}} = 98 \cdot \frac{A_{max}} {34.668} \cdot k(T)_{v_{cmax}}`$

 

$` \small V_{c_{max}}`$	Maximum saturated Rubisco carboxylation rate	$` \small [\mu mol \, m^{-2} \, s^{-1}] `$
$` \small A_{max} `$	Plant-specific maximum CO2 assimilation rate	$` \small [kg \, CO_2 \, ha^{-1} \, d^{-1}] `$
$` \small k(T)_{v_{cmax}} `$	Temperature function for Vcmax	 
 

Temperaturfunktion für Vcmax (Long 1991).

![Temperature function for Vcmax (Long 1991).][https://gitlab.com/zalf-rpm/monica-docs/blob/master/processes/MONICA_Photosynthese_Temperature_KTvmax.png "Temperature function for Vcmax (Long 1991)."]
Figure 1: Temperature function for Vcmax (Long 1991).

For crops using the C4 metabolism, no direct impact of the atmospheric CO2 concentration on photosynthesis is assumed. The crop-specific maximum CO2 assimilation rate is merely modified by a simple temperature function.

![Temperature function for the CO2 assimilation rate of C4 crops (Sage & Kubien 2007).][MONICA_Photosynthese_Temperature_C4.png "Temperature function for the CO2 assimilation rate of C4 crops (Sage & Kubien 2007)."]

Figure 2: Temperature function for the CO2 assimilation rate of C4 crops (Sage & Kubien 2007).

 

 

## References
Farquhar, G. D. & von Caemmerer, S. (1982). Modelling of photosynthetic response to environmental conditions. In: Encyclopedia of plant physiology. New series. Volume 12B. Physiological plant ecology. II. Water relations and carbon assimilation. (Eds O. L. Lange, P. S. Nobel, C. B. Osmond, & H. Ziegler), pp. 549-587. Berlin: Springer.

Goudriaan, J. & van Laar, H. H. (1978). Relations between leaf resistance, CO2 concentration and CO2 assimilation in maize, beans, lalang grass and sunflower. Photosynthetica 12 (3), 241-249.

Long, S. P. (1991). Modification of the response of photosynthetic productivity to rising temperature by atmospheric CO2 concentrations - Has its importance been underestimated. Plant Cell and Environment 14 (8), 729-739.

Mitchell, R. A. C., Lawlor, D. W., Mitchell, V. J., Gibbard, C. L., White, E. M., & Porter, J. R. (1995). Effects of elevated CO2 concentration and increased temperature on winter-wheat - Test of ARCWHEAT1 simulation model. Plant Cell and Environment 18 (7), 736-748.

Nendel, C., Kersebaum, K. C., Mirschel, W., Manderscheid, R., Weigel, H. J., & Wenkel, K.-O. (2009). Testing different CO2 response algorithms against a FACE crop rotation experiment. NJAS - Wageningen Journal of Life Sciences 57 (1), 17-25.

Sage, R. F. & Kubien, D. S. (2007). The temperature response of C3 and C4 photosynthesis. Plant Cell and Environment 30 (9), 1086-1106.

van Keulen, H., Penning de Vries, F. W. T., & Drees, E. M. (1982). A summary model for crop growth. In: Simulation of plant growth and crop production (Eds F. W. T. Penning de Vries & H. H. van Laar), pp. 87-97. Wageningen: PUDOC.

http://monica.agrosystem-models.com/images/phocagallery/prozesse/MONICA%20Photosynthese%20Temperature%20KTvmax.png