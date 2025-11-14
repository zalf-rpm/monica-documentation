# MONICA: The Model for Nitrogen and Carbon Dynamics in Agro-Ecosystems

## Introduction

This site is the official documentation of the MONICA model.

MONICA is a **dynamic, process-based simulation model** which describes transport and bio-chemical turn-over of carbon, nitrogen and water in agro-ecosystems. On **daily time steps** the most important processes in soil and plant are modelled mechanistically. They are linked in such way that feed-back relations of the single processes are reproduced as close to nature as possible. MONICA works **one-dimensional** and represents a space of 1 m<sup>2</sup> surface area and 2 m depth.

The acronym MONICA is derived from “**MO**del of **NI**trogen and **CA**rbon dynamics in agro-ecosystems”.

---

## What can MONICA do?

For daily time steps, MONICA calculates all processes that interact with the bio-chemical turn-over of carbon and nitrogen in soil and with their transport in soil, air and plant. A broad range of variables being of interest for the user can be accessed day-by-day. This could be easily measurable items, such as soil moisture, carbon and nitrogen contents, the crop’s biomass, or yield. These quantities are well suited to evaluate the performance of the MONICA simulations. However, MONICA also shows items that can only be quantified with great effort, such as nitrogen mineralisation from crop residues or organic fertilisers, NH<sub>3</sub>-, N<sub>2</sub>O-, and CO<sub>2</sub> emissions from soil, denitrification, potential groundwater recharge, crop evapotranspiration, or the leaching of nitrates into deeper soil layers.

---

## Which processes are described?

MONICA describes processes in the soil–plant system and the energy and matter exchange with the hydrosphere and atmosphere. Using daily weather data, it calculates the soil temperature for single discrete layers in the soil. The movement of water in the soil is modelled using a capacity approach. This approach assumes that water that cannot be stored in a soil layer will be passed on to the adjacent layer below. The layer’s storage capacity and its percolation rate are governed by its texture and soil organic matter content. Evaporation and water uptake by the root influence the water budget. If groundwater is accessible, capillary water can rise into the root zone.

Moving soil water carries nitrates. They originate from organic matter turn-over, appearing first as ammonium (ammonification) which is later turned into nitrates (nitrification). If oxygen is deficient, nitrate can be transformed into atmospheric nitrogen (denitrification), a process during which the greenhouse gas N<sub>2</sub>O is produced. The microorganisms facilitating these processes and producing CO<sub>2</sub> from their metabolism are also simulated. When organic fertilisers are applied, gaseous NH<sub>3</sub> is set free. Furthermore, also urea fertiliser hydrolyses in the soil and releases NH<sub>3</sub>.

The crop’s biomass increases according to the daily radiation and temperature. A target and a critical nitrogen concentration are calculated for the plant tissue. The first serves the calculation of nitrogen uptake from the soil, while the second is used to determine nitrogen deficiency. In the latter case, similar to drought stress, the crop’s daily growth increment will be reduced. In daily time steps the root grows into depth. The calculated root biomass is distributed to the discrete soil layers accordingly. The calculation of water and nitrogen uptake by the plant considers this distribution and consumes from the respective soil layers. 

At harvest, crop residues remain. They are decomposed and contribute to soil organic matter pools, nitrogen mineralisation, and CO<sub>2</sub> release. Every organic material has its own properties which determine its decay. That way a lettuce leaf will be mineralised much faster than wheat straw. Currently, MONICA is able to simulate eight different crops.

---

## MONICA’s crop growth concept

The crop growth concept of MONICA is based on the calculation of assimilate production from radiation. In this manner, the process of photosynthesis is simplified. The efficiency of carbohydrate production is dependent on temperature. The virtual crop’s development from seed to harvest maturity is determined using the accumulating temperature. For each developmental stage, the distribution of carbohydrates within the crop is continuously adjusted. In early stages root and leaf growth is promoted, while shoot and fruit growth will be increasingly supported at later stages. The distributed carbohydrates will be converted into dry matter biomass and – in the case of leaves – also in leaf area. In turn, leaf area, as the main location for photosynthesis, finds its way into the calculation of carbohydrate production.

The amount of dry matter assigned to the root is distributed down the soil profile according to an empirical formula. The current rooting depth is depending on soil texture, but also on the crop itself. Further on, root length density limits the root’s ability to take up water and nitrogen.  

From leaf area, the degree of soil coverage is estimated, which in turn determines the fraction of transpiration from total evapotranspiration. Actual evapotranspiration is calculated relative to a cut grass crop, using crop-specific coefficients. The resulting water loss is then – a sufficient supply assumed – taken from the respective soil layers according to the root distribution. In case water supply is insufficient and falls below a specific threshold value, crop growth will be reduced. This also applies in the case of nitrogen supply from soil being insufficient for maintaining the N concentration in the crop tissue above a critical level.

At harvest, marketable parts of the crop are removed and the remaining residues are allocated to conceptual organic matter pools in the soil. Nitrogen in the crop is then divided between marketable yield and crop residues.

---

## Applications

MONICA has been applied so far in three major research projects:

* The **LandCaRe 2020** project focused on the development of a knowledge platform on which stakeholders can access information on the expected impact of climate change on agriculture in Germany (BMBF Funding Focus *klimazwei*).
* In the **CarBioCial** project, three regions along a land use gradient in Southern Amazonia (Brazil) are currently analysed to derive recommendations for a carbon-neutral land management under climate change conditions in Brazil (BMBF Funding Focus *Sustainable Land Management*).
* Within the **EVA2**, project crop rotations for energy production are currently grown on different sites across Germany. MONICA is used to evaluate the impact of energy cropping on the environment, especially on groundwater recharge and nitrogen leaching (FNR).

---

## Technical details

MONICA was programmed in C++. This makes it easier to connect with other programs that work together with MONICA and ensures high performance. Simulations covering long periods of time can therefore be run in seconds.

MONICA’s construction is modular. A central control module manages crop management actions (sowing, fertilisation, irrigation, tillage, and harvest). These actions are sorted and assigned to the respective topical modules. The control module is also responsible for providing and outputting results. The topical modules calculate soil temperature, soil water dynamics, soil matter transport, soil organic matter turn-over, and crop growth. An additional central module saves relevant state variables for the respective soil layers.

---

## History

### The development of MONICA

MONICA is a successor of the HERMES model. HERMES is a pure nitrogen model. It had to be modified to also support research questions on carbon dynamics under climate change. The simple algorithm for the calculation of nitrogen mineralisation from soil organic matter was replaced by a more comprehensive one, which was taken from the Danish DAISY model. It calculates the carbon turn-over and derives nitrogen dynamics from it. This approach also considers soil microbial biomass dynamics. Furthermore, the impact of atmospheric CO<sub>2</sub> concentration on the crop’s photosynthesis and transpiration was introduced.

Using these algorithms, the increased photosynthesis and the improved water use efficiency of crops grown under elevated CO<sub>2</sub> concentrations can be simulated according to observations made from free air carbon enrichment experiments. Finally, the new model, which was given the name MONICA, was equipped with automatically triggered fertilising and irrigation routines. They enable MONICA to run in long-term scenarios without having to adjust the virtual crop’s management to gradually changing conditions.

### MONICA’s predecessor: the HERMES model

The HERMES model (Kersebaum 1989) was the first simulation model for the year-round simulation of N dynamics in agricultural sites developed in Germany. It uses a generic, photosynthesis-driven crop model, built on the archetype SUCROS (Van Keulen et al. 1982). This enables HERMES to consider a broad range of crops. In general, the process model comprises rather simple algorithms and requires easy-to-access input data only.

However, HERMES used some input information that was specific for Germany, such as the German soil classification system and Haude’s evapotranspiration factors. For this reason, it was not much used by international colleagues. In Germany, however, HERMES was applied in many case studies, including regional applications in combination with a geo-information system. More recent releases include alternative evapotranspiration algorithms and enable a less restrictive parameterisation of the soil. This version was also applied outside Europe (Kersebaum et al. 2008). The HERMES model is considered the heart of MONICA.

#### References

* Kersebaum, K.C. (1989): Die Simulation der Stickstoff-Dynamik von Ackerböden. Dissertation Universität Hannover.

* Van Keulen, H., Penning de Vries, F.W.T., Drees, E.M. (1982): A summary model for crop growth. In: Penning de Vries, F.W.T. & Van Laar, H.H. (Hrsg.): Simulation of plant growth and crop production. PUDOC, Wageningen, 87 - 97.

* Kersebaum, K.C., Wurbs, A., De Jong, R., Campbell, C.A., Yang, J., Zentner, R.P. (2008): Long term simulation of soil-crop interactions in semiarid southwestern Saskatchewan, Canada. European Journal of Agronomy 29, 1-12

---

## Publication & How to Cite MONICA

### Official Reference

!!! note ""
    **Nendel, C., M. Berg, K.C. Kersebaum, W. Mirschel, X. Specka, M. Wegehenkel, K.O. Wenkel and R. Wieland (2011)**: *The MONICA model: Testing predictability for crop growth, soil moisture and nitrogen dynamics.* Ecological Modelling, 222(9), 1614–1625. [https://doi.org/10.1016/j.ecolmodel.2011.02.018](https://doi.org/10.1016/j.ecolmodel.2011.02.018)

### How to Cite MONICA

When using MONICA in publications, please cite the main model description paper.

#### In-text citation example

> “Simulations were performed using the MONICA agro-ecosystem model (Nendel et al., 2011).”

#### Full reference example

> Nendel, C., M. Berg, K.C. Kersebaum, W. Mirschel, X. Specka, M. Wegehenkel, K.O. Wenkel and R. Wieland (2011): *The MONICA model: Testing predictability for crop growth, soil moisture and nitrogen dynamics.* Ecological Modelling, 222(9), 1614–1625. https://doi.org/10.1016/j.ecolmodel.2011.02.018

### BibTeX entry

```bibtex
@article{NENDEL20111614,
    title = {The MONICA model: Testing predictability for crop growth, soil moisture and nitrogen dynamics},
    journal = {Ecological Modelling},
    volume = {222},
    number = {9},
    pages = {1614-1625},
    year = {2011},
    issn = {0304-3800},
    doi = {https://doi.org/10.1016/j.ecolmodel.2011.02.018},
    url = {https://www.sciencedirect.com/science/article/pii/S0304380011000998},
    author = {C. Nendel and M. Berg and K.C. Kersebaum and W. Mirschel and X. Specka and M. Wegehenkel and K.O. Wenkel and R. Wieland},
    keywords = {Simulation model, Climate change, Validation, Crop rotation, Yield prediction},
    abstract = {A fundamentally revised version of the HERMES agro-ecosystem model, released under the name of MONICA, was calibrated and tested to predict crop growth, soil moisture and nitrogen dynamics for various experimental crop rotations across Germany, including major cereals, sugar beet and maize. The calibration procedure also included crops grown experimentally under elevated atmospheric CO2 concentration. The calibrated MONICA simulations yielded a median normalised mean absolute error (nMAE) of 0.20 across all observed target variables (n=42) and a median Willmott's Index of Agreement (d) of 0.91 (median modelling efficiency (ME): 0.75). Although the crop biomass, habitus and soil moisture variables were all within an acceptable range, the model often underperformed for variables related to nitrogen. Uncalibrated MONICA simulations yielded a median nMAE of 0.27 across all observed target variables (n=85) and a median d of 0.76 (median ME: 0.30), also showing predominantly acceptable results for the crop biomass, habitus and soil moisture variables. Based on the convincing performance of the model under uncalibrated conditions, MONICA can be regarded as a suitable simulation model for use in regional applications. Furthermore, its ability to reproduce the observed crop growth results in free-air carbon enrichment experiments makes it suited to predict agro-ecosystem behaviour under expected future climate conditions.}
}
```

### Model Version & Software Archive

A DOI for the MONICA source code is not yet available. Until further notice, please cite the model using the official MONICA reference listed above and include the version used in your study.

---

## **Quick Start**

### **1. Download and Extract**

- Get the latest **`monica-windows-x86_64.zip`** from the [GitHub releases page](https://github.com/zalf-rpm/monica/releases).
- Extract it to a simple location, e.g.: `C:\monica\`
- The folder structure should look like this:

```
C:\monica
├── bin
├── monica-parameters
├── projects
└── documentation\
```

### **2. Run the Example Simulation**

- Open **Command Prompt** and navigate to the example project directory:

```
C:\monica\projects\Hohenfinow2-single-run
```

- Run the simulation by typing:

```
run_monica.cmd
```

This automatically calls `monica-run.exe` from the `bin` folder and creates an output file named `sim-out.csv`.

### **3. Verify and Use**

-   If `sim-out.csv` appears, MONICA is working fine.
-	Users can now edit the `sim.json`, `crop.json`, `site.json`, and `climate.csv` files to run their own simulations.
-	(Optional) Add `C:\monica\bin` to the **PATH** and set **`MONICA_PARAMETERS = C:\monica\monica-parameters`**  to use MONICA system-wide.