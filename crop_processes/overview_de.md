# PFLANZENPROZESSE

The crop growth concept followed with the development of MONICA is based on the calculation of carbohydrates production from radiation input, mimicking photosynthesis. Its efficiency is temperature-dependent. The development of the virtual plant from sowing to harvest maturity is determined from accumulation of heat units. For each developmental stage the distribution of the produced carbohydrates to the plant’s organs is adjusted. First, growth of root and leaves is promoted, later that of shoots and fruits. The distributed carbohydrates are then converted into dry matter biomass and, in the case of leaves, in leaf area. In turn, leaf area, being the primary site for photosynthesis going on, determines the production of carbohydrates.

The dry matter biomass assigned to the root is distributed to the discrete soil layers using an empirical algorithm. The actual rooting depth is governed by soil texture on the one hand, on the other by the crop itself. Root length density limits the uptake of water and nitrogen.

Soil coverage is calculated from leaf area, which, in turn, determines the contribution of crop transpiration to the total water vapour loss from crop and soil together (evapotranspiration) Actual evapotranspiration is calculated relative to short-cut grass, using crop-specific coefficients. The water loss is replaced by taking it up from the soil layers according to the root distribution, if supply is sufficient. If water supply is not sufficient and falls below a threshold, crop growth will be reduced. This is also the case when soil nitrogen supply is not enough to prevent the crop’s nitrogen concentration falling below the critical value.

At harvest, yield will be removed from the system. The remaining crop residues are returned to the soil. The nitrogen content of the plant is partitioned between marketable parts and residues.

1. [Photosynthese](photosynthesis_de.md)
2. [Atmung](respiration_de.md)
3. [Assimilatverteilung](assimilate_partitioning_de.md)
4. [Ontogenese](ontogenesis_de.md)
5. [Wurzelwachstum](root_growth_de.md)
6. [Blattfläche und Bedeckungsgrad](leaf_area_and_ground_coverage_de.md)
7. [Transpiration](transpiration_de.md)
8. [N-Aufnahme](n_uptake_de.md)
9. [N-Konzentration](n_concentration_de.md)
10. [Trockenstress](drought_stress_de.md)
11. [Stickstoffmangel](nitrogen_deficiency_de.md)
12. [Sauerstoffstress](oxygen_deficiency_de.md)
13. [Hitzestress](heat_stress_de.md)
14. [Ertrag](yield_de.md)