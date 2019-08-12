# Soil frost

Soil frost algorithms were taken from Olsen and Haugen (1997) who assumed that the same thermal properties apply across the whole soil profile. The temperatures simulated in the soil temperature module are not considered, because the phase transition between water and ice is not considered. Parameter values were taken from the SOIL model (Jansson, 1991). The frost module requires the soil surface temperature–modified by a possible snow layer–as an input. The ECOMAG model (Motovilov et al., 1999) delivered the approach to describe thawing.

For the heat conductivity calculation of frozen soil a fixed moisture content at field capacity is assumed. According to Hanson et al. (2004) this moisture content is corrected by

$`\small \theta_{FCmf} = \theta_{FCm} + (1 + \theta_{FCm} \cdot (13.05 \cdot \theta^{1.06}_{FCm}))  `$

$`\small \theta_{FCmf}`$	Fixed moisture content at field capacity (frozen)	$`\small [m^3 \, m^{-3}] `$<br>
$`\small \theta_{FCm}`$	Fixed moisture content at field capacity	$`\small [m^3 \, m^{-3}] `$<br>

The contribution of heat supply from the lower soil to latent heat is determined by

$`\small q_Q = \frac{0.3 \cdot d_F}{Q_I} `$

$`\small Q_I = 335 \cdot \theta_{FCm}`$

$`\small q_Q`$	Fraction of heat flux from lower soil to latent heat	 <br>
$`\small Q_I `$	latent heat	$`\small [MJ] `$<br>
$`\small d_F`$	Number of frost days	$`\small [d] `$<br>
$`\small \theta_{FCm}`$	Fixed moisture content at field capacity	$`\small [m^3 \, m^{-3}] `$<br>

Frost depth is then derived from

$`\small z_F = \sqrt{\left( \frac{q_Q}{2}  \right )^2 + 2 \cdot \theta_{FCmf} \ cdot \frac{dd_n}{Q_I} - \frac{q_Q}{2}  } `$

$`\small z_F`$	Frost depth	$`\small [m] `$<br>
$`\small q_Q `$	Fraction of heat flux from lower soil to latent heat	 <br>
$`\small \theta_{FCmf}`$	Fixed moisture content at field capacity (frozen)	$`\small [m^3 \, m^{-3}] `$<br>
$`\small Q_I`$	latent heat	$`\small [MJ] `$<br>
$`\small dd_n`$	Sum of negative degree days since begin of frost	$`\small [^{\circ} C \, d] `$<br>

In the case of soil frost the daily progression of the thaw front into depth is calculated:

$`\small \Delta z_T = \begin{cases}  \sqrt{\frac{2 \cdot \lambda_h \cdot | T_s | } {79000 \cdot \theta_{FC}}} & T_s \geq 0^{\circ}C    \\ -  \sqrt{\frac{2 \cdot \lambda_h \cdot | T_s | } {79000 \cdot \theta_{FC}}}  & T_s < 0^{\circ}C  \end{cases}`$

$`\small \Delta z_T`$	Depth increment of the thaw front	$`\small [m] `$<br>
$`\small \lambda_h`$	Soil heat conductivity	$`\small [J \, m^{-1} \, s^{-1} \, K^{-1}] `$<br>
$`\small T_s`$	Temperature beneath the snow layer	$`\small [^{\circ} C] `$<br>
$`\small \theta_{FC}`$	Fixed water content at field capacity	$`\small [m^3 \, m^{-3}] `$<br>

where

$`\small T_s = \frac{T_a} {1+\frac{(10 \cdot S)}{z_F}} `$

$`\small T_s`$	Temperature beneath the snow layer	$`\small [^{\circ} C] `$<br>
$`\small S`$	Snow height	$`\small [m] `$<br>
$`\small T_a`$	Daily mean air temperature	$`\small [^{\circ} C] `$<br>
$`\small z_F`$	Frost depth	$`\small [m] `$<br>

For the case of soil frost it is assumed that no water moves in the frozen layers ($`\small\lambda=0`$). Water from rainfall or melting snow infiltrates into soil until saturation is reached. The remainder is stored in the surface pond or adds to surface run-off. Once the soil has thawn–i.e. the thaw front has reached the frost front (zT > zF)–gravitational water can pass through the profile and water from the surface pond can infiltrate. The variables zT, zF, ddn and dF are then re-set to zero.

## References

* Hansson, K., Simunek, J., Mizoguchi, M., Lundin, L.C., van Genuchten, M.T. (2004): Water flow and heat transport in frozen soil: Numerical solution and freeze-thaw applications. Vadose Zone Journal 3, 693-704.

* Jansson, P.E. (1991): The SOIL model - Users manual. 91-6. Swedish University of Agricultural Sciences, Uppsala.

* Motovilov, Y. G., Gottschalk, L., Engeland, K., Belokurov, A. (1999): ECOMAG: Regional model of hydrological cycle - Application to the NOPEX region.  1051. Dept. Geophysics, University of Oslo, Oslo, pp. -88.

* Olsen, P. A., Haugen, L. E. (1997): Jordas termiske egenskaper.  8. Dept. Soil and Water Science, Agricultural University of Norway, Ås, pp. -14.