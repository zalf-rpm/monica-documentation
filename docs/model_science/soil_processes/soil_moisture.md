# Soil moisture

A capacity approach was used to describe soil water dynamics (Wegehenkel, 2000). The capacity parameters are derived from the soil texture and modified by soil organic matter content and bulk density. Water contents at saturation, field capacity, and permanent wilting point for different bulk densities and correction values for different soil organic matter classes (Ad-hoc-AG Boden, 2005) are provided by Wessolek et al. (2009) and stored in the database.

If a crop is present, precipitation is partly intercepted and evaporates from the crop surface. Interception $I$ is calculated as:

$$I = (2.5 \cdot h_c \cdot \beta) - S_i$$

$I$	Interception $[mm]$<br>
$h_c$ Crop height $[m]$<br>
$\beta$	Canopy closure $[m^2\,m^{-2}]$<br>
$S_i$ Interception storage $[mm]$<br>

The remainder falls on the ground and is stored in a surface pond, from which water infiltrates into the soil. As long as the surface pond contains water, it is the only source of evaporation. The percolation of water volumes above field capacity is governed by an empirical, texture-dependent rate coefficient ($\lambda$):

$$\lambda = 1.15 \cdot f^2_s + 0.1 \cdot f_c + 0.35 \cdot f_u$$

$\lambda$ Empirical percolation rate coefficient<br>
$f_s$ Soil sand content	$[kg \, kg^{-1}]$<br>
$f_c$ Soil clay content	$[kg \, kg^{-1}]$<br>
$f_u$ Soil silt content	$[kg \, kg^{-1}]$<br>

If the groundwater level is located within the simulated soil profile, constant groundwater discharge can be adjusted to allow for the rising and falling groundwater level, depending on the soil water balance. The capillary rise from groundwater is considered according to empirical ascent rates (Ad-hoc-AG Boden, 2005). The groundwater level oscillates between given maximum and minimum levels with a period of one year.

Reference evapotranspiration $ET0$ is calculated using the Penman-Monteith method, according to Allen et al. (1998). This method requires the diurnal minimum and maximum temperature, the water vapour pressure deficit, wind velocity, and total global radiation.

$$ET_0 = \frac{0.408 \cdot \Delta \cdot (R_n - G) + \gamma \cdot \frac{900}{T + 273} \cdot u_2 \cdot (e_s - e_a)} {\Delta + \gamma \cdot (1 + \frac{r_a}{r_s} )}$$

$\Delta$ Slope of the vapour pressure curve	$[kPa \, K^{-1}]$<br>
$R_n$ Net radiation at the crop surface	$[MJ \, m^{-2} \, d^{-1}]$<br>
$G$ Soil heat flux density $[MJ \, m^{-2} \, d^{-1}]$<br>
$T$	Mean daily air temperature at 2 m height $[^{\circ}C]$<br>
$u_2$ Wind speed at 2 m height $[m\,s^{-1}]$<br>
$e_s$ Saturation vapour pressure $[kPa]$<br>
$e_a$ Actual vapour pressure $[kPa]$<br>
$\gamma$ Psychrometric constant	$[kPa\, K^{-1}]$<br>
$r_s$ Atmospheric resistance $[s \, m^{-1}]$<br>
$r_a$ Surface resistance $[s \, m^{-1}]$<br>

where

$$\gamma = 6.65 \cdot 10^{-4} \cdot P$$

$P$	Atmospheric pressure $[Pa]$<br>

The surface resistance for the reference evapotranspiration assumes a 12 cm cut grass crop and is calculated using:

$$r_s = \frac{r_1} {1.44}$$

$r_1$ Stomata resistance; 100 s m-1	$[s \, m^{-1}]$<br>

The surface resistance for the actual crop is calculated in the crop growth module.

---

## Pedo-Transfer Functions

A Pedo-Transfer Function (PTF) mechanism that transforms soil physical properties (mainly texture) into water retention capacity parameters. These parameters, known as pore space (PS or saturation water capacity), field capacity (FC) and permanent wilting point (WP). These parameters are required by the tipping-bucket water balance approach used in MONICA to simulate soil water content dynamics.

MONICA currently supports four PTFs, divided into two types:

### Empirical PTFs

These methods use lookup tables or regression equations to directly relate soil properties to PS, FC, and WP.

**Wessolek2009**
Uses a lookup table that relates categorical soil types to water retention parameters. The table is stored in [`SoilCharacteristicData.json`](https://github.com/zalf-rpm/monica-parameters/blob/master/soil/SoilCharacteristicData.json), where `airCapacity` = PS and `nFieldCapacity` = WP. Based on Wessolek et al. (2009).

**Toth**
Based on regression equations reported in Tóth et al. (2015), specifically equations 5, 9, and 12 of Table S1 (Supplementary Material). Implemented as `fcSatPwpFromToth` in [`soil.cpp`](https://github.com/zalf-rpm/mas_cpp_misc/blob/5f0c31b0dc01ef3984f5eca21a067ecf84e70af4/soil/soil.cpp), line 911.

### Van Genuchten PTFs

These methods build a soil water retention curve using the Van Genuchten formula. Five parameters are estimated from soil texture and organic carbon, and PS, FC, and WP are then derived from this curve at defined matric head values.

The Van Genuchten retention curve relates soil moisture $\theta$ to matric head $h$:

$$\theta(h) = \theta_r + \frac{\theta_s - \theta_r}{\left[1 + (\alpha |h|)^n\right]^m}$$

In MONICA, because the current soil moisture $\theta$ is already known at each time step, the equation is inverted to compute the matric head $h$, which is then converted to a pF value:

$$pF = \log_{10}(|h|)$$

The workflow in the code is therefore:

```
soil moisture (θ)
      ↓
inverse Van Genuchten
      ↓
matric head (h)
      ↓
pF value
      ↓
water retention parameters (PS, FC, WP)
```

The pF values corresponding to FC and WP are then used to derive the water retention parameters.

**VanGenuchtenVereecken**
Based on Vereecken et al. (1989). This method assumes $\theta_r$ = WP (i.e. permanent wilting point equals residual water content) and fixes $m = 1$. Because $\theta_s$ = PS and $\theta_r$ = WP are already known, only FC needs to be calculated. FC is not calculated at a single pF value but varies between pF 1.9 and 2.4 depending on clay and sand content. Implemented as `fcSatPwpFromVanGenuchtenVereecken` in [`soil.cpp`](https://github.com/zalf-rpm/mas_cpp_misc/blob/5f0c31b0dc01ef3984f5eca21a067ecf84e70af4/soil/soil.cpp), line 847.

!!! note
    Setting $m = 1$ is a simplification and is physically inaccurate. It removes one degree of freedom from the retention curve, causing the drying tail to become flatter meaning the soil appears to retain more water in the dry range than it should. This affects plant water availability, drying dynamics, and hydraulic conductivity calculations. The assumption $\theta_r$ = WP is also physically questionable. Because $m = 1$, the shape of the retention curve depends almost entirely on the pedotransfer function for $n$.

**VanGenuchtenToth**
Based on Tóth et al. (2015), equation set 21 of the Supplementary Material. WP is estimated at a matric head of −15,000 cm, PS equals $\theta_s$, and FC is calculated in the same way as in VanGenuchtenVereecken. Unlike VanGenuchtenVereecken, the parameter $m$ is not fixed at 1 but is derived from $n$ using its own formula, making the retention curve more physically realistic. Implemented as `fcSatPwpFromVanGenuchtenToth` in [`soil.cpp`](https://github.com/zalf-rpm/mas_cpp_misc/blob/5f0c31b0dc01ef3984f5eca21a067ecf84e70af4/soil/soil.cpp), line 881.

### Summary of available PTFs

| PTF | Type | Based on | Key assumption |
|---|---|---|---|
| `Wessolek2009` | Empirical | Wessolek et al. (2009) | Lookup table by soil type |
| `Toth` | Empirical | Tóth et al. (2015) | Regression equations |
| `VanGenuchtenVereecken` | Van Genuchten curve | Vereecken et al. (1989) | $\theta_r$ = WP, $m$ = 1 |
| `VanGenuchtenToth` | Van Genuchten curve | Tóth et al. (2015) | WP at pF 4.2, $m$ from $n$ |

### Configuration

The PTF to use is specified in `site.json` via the `pwpFcSatFunction` key:

```json
"pwpFcSatFunction": "Wessolek2009"
```

Accepted values: `"Wessolek2009"`, `"Toth"`, `"VanGenuchtenVereecken"`, `"VanGenuchtenToth"`

---

## Lambda Factor

Lambda ($\lambda$) is an empirical, texture-dependent percolation rate coefficient that governs how much water exceeding field capacity at a given soil layer is transferred to the layer below. A higher lambda value results in faster percolation.

### Default formula in MONICA

The default lambda is calculated from sand, clay, and silt fractions. In the MONICA source code (`soil_io3.py` and `mas_cpp_misc/soil/conversion.cpp`, line 78), it is implemented as:

```python
lambda = (2.0 * (sand * sand * 0.575)) + (clay * 0.1) + ((1.0 - sand - clay) * 0.35)
```

where `sand` and `clay` are given as fractions (kg kg⁻¹).

!!! note
    The code formula and the equation shown in the soil moisture description above are mathematically equivalent. Expanding the code: `2.0 * sand² * 0.575` = `1.15 * sand²`, `clay * 0.1` = `0.1 * fc`, and `(1.0 - sand - clay) * 0.35` = `0.35 * silt`. The code simply uses a different but equivalent notation.

### Alternative lambda methods

Alternative methods for calculating lambda have been described in the literature, including those by Rawls et al. (1982) and Carsel & Parrish (1988). These methods generally produce lower lambda values and therefore lower percolation rates compared to the MONICA default.

### Overriding the default lambda

A custom lambda value can be provided directly in the soil input file by including a `Lambda` column in `Soil.csv` and reading it in the run-producer script:

```python
"Lambda": [float(row['lambda']), "-"]
```

To use the MONICA default lambda instead, this line can be commented out.

!!! note
    A known issue exists in the source code regarding low water percolation in loam soils. A temporary override (`lambda = 1.0`) is noted in `conversion.cpp` and is currently under investigation.

---

## References

- Ad-hoc-AG Boden (2005). *Bodenkundliche Kartieranleitung* (5th ed.). Hannover.
- Allen, R. G., Pereira, L. S., Raes, D., & Smith, M. (1998). *Crop evapotranspiration: Guidelines for computing crop water requirements*. FAO Irrigation and Drainage Paper 56.
- Carsel, R. F., & Parrish, R. S. (1988). Developing joint probability distributions of soil water retention characteristics. *Water Resources Research*, 24(5), 755–769.
- Rawls, W. J., Brakensiek, D. L., & Saxton, K. (1982). Estimation of soil water properties. *Transactions of the ASAE*, 25(5), 1316–1320.
- Tóth, B., Weynants, M., Nemes, A., Makó, A., Bilas, G., & Tóth, G. (2015). New generation of hydraulic pedotransfer functions for Europe. *European Journal of Soil Science*, 66(1), 226–238.
- Vereecken, H., Maes, J., Feyen, J., & Darius, P. (1989). Estimating the soil moisture retention characteristic from texture, bulk density, and carbon content. *Soil Science*, 148(6), 389–403.
- Wegehenkel, M. (2000). Test of a modelling system for simulating water balances and plant growth using various different complex approaches. *Ecological Modelling*, 129(1), 39–64.
- Wessolek, G., Kaupenjohann, M., & Renger, M. (2009). *Bodenökologie und Bodengenese: Bodenphysikalische Kennwerte und Berechnungsverfahren für die Praxis* (40).