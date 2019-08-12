# Matter transport

In soil, nutrients are transported with the moving water. So far, MONICA only describes the translocation of dissolved nitrate ions. However, using the convection-dispersion equation, modelling of ammonium, sulphate and dissolved organic matter transport are also imaginable. The basic version of the convection-dispersion equation is:

$`\small \frac{\partial \theta c}{\partial t}  = \frac{\partial}{\partial z} \cdot \left ( \theta \cdot D \cdot \frac{\partial c}{\partial z}\right ) - \frac{\partial q c}{\partial z} `$

$`\small \theta `$	Soil moisture content	$`\small [m^{3} \, m^{-3}] `$<br>
$`\small c`$	Solute concentration	$`\small [kg \, m^{-3}] `$<br>
$`\small D`$	Effective dispersion coefficient	$`\small [m^{2} \, m^{-1}] `$<br>
$`\small z`$	Soil depth	$`\small [m] `$<br>
$`\small t`$	Time	$`\small [d] `$<br>

which includes water flow

$`\small q = -K (\Psi_m) \cdot \left ( \frac{\partial \Psi_m}{\partial z} - 1   \right)`$

$`\small q`$	Water flux density	$`\small [m \, d^{-1}] `$<br>
$`\small \Psi_m`$	Soil matrix potential	$`\small [cm \, WS] `$<br>
$`\small K(\Psi_m)`$	Hydraulic conductivity	$`\small [m \, d^{-1}] `$<br>
$`\small \theta`$	Soil moisture content	$`\small [m^3 \, m^{-3}] `$<br>
$`\small z`$	Soil depth	$`\small [m] `$<br>

the effective dispersion coefficient

$`\small D = D_0 \cdot \frac{1}{\tau} + \alpha_v \cdot \left |  \frac{q}{\theta} \right | `$

$`\small D`$	Effective dispersion coefficient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small D_0`$	Diffusion coefficient in solution (0.000214)	$`\small [m^2 \, d^{-1}] `$<br>
$`\small \tau`$	Tortuosity	$`\small [m^2 \, m^{-2}] `$<br>
$`\small \alpha_v `$	Dispersion factor (0.05)	$`\small [m] `$<br>
$`\small q`$	Water flux density	$`\small [m \, d^{-1}] `$<br>
$`\small \theta `$	Volumetric water content	$`\small [m^3 \, m^{-3}] `$<br>

in which tortuosity is used as its inverse, pore space continuity.

$`\small \tau = \frac{\theta}{a \cdot e^{10 \cdot \theta}}`$

$`\small \tau `$	Tortuosity	$`\small [m^2 \, m^{-2}] `$<br>
$`\small \theta`$	Volumetric water content	$`\small [m^3 \, m^{-3}] `$<br>
$`\small a`$	Factor (0.002)	 <br>

The convection-dispersion equation is solved using the explicit finite difference method.

For q1/2 > 0

$`\small \frac{ \theta_{i , t+\Delta t} \cdot c_{i, t+\Delta t} - \theta_{i, t} \cdot c_{i, t}}   {\Delta t} = `$

$`\small \left (   \theta_{i - \frac{1}{2}, t} \cdot D_{i-\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i-\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i-\frac{1}{2}, t}} {2 \cdot \theta_{i-\frac{1}{2}, t}} \right )   \cdot \frac {c_{i-1, t} - c_{i, t}} {(\Delta z)^2}  `$

$`\small - \left ( \theta_{i+\frac{1}{2}, t} \cdot D_{i+\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i+\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i+\frac{1}{2},t}}  {2 \cdot \theta_{i+\frac{1}{2},t}}   \right ) \cdot \frac{c_{i,t} - c_{i+1,t}}{(\Delta z)^2}  `$

$`\small -  \frac{q_{i+\frac{1}{2}, t} \cdot c_{i,t} - q_{i-\frac{1}{2}, t} \cdot c_{i-1,t} }{\Delta z}  `$

and for q1/2 < 0

$`\small \frac{ \theta_{i , t+\Delta t} \cdot c_{i, t+\Delta t} - \theta_{i, t} \cdot c_{i, t}}   {\Delta t} = `$

$`\small \left (   \theta_{i - \frac{1}{2}, t} \cdot D_{i-\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i-\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i-\frac{1}{2}, t}} {2 \cdot \theta_{i-\frac{1}{2}, t}} \right )   \cdot \frac {c_{i-1, t} - c_{i, t}} {(\Delta z)^2}  `$

$`\small - \left ( \theta_{i+\frac{1}{2}, t} \cdot D_{i+\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i+\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i+\frac{1}{2},t}}  {2 \cdot \theta_{i+\frac{1}{2},t}}   \right ) \cdot \frac{c_{i,t} - c_{i+1,t}}{(\Delta z)^2}  `$

$`\small -  \frac{q_{i+\frac{1}{2}, t} \cdot c_{i+1,t} - q_{i-\frac{1}{2}, t} \cdot c_{i,t} }{\Delta z}  `$

$`\small \theta `$	Volumetric water content	$`\small [m^3 \, m^{-3}] `$<br>
$`\small c `$	Solute concentration	$`\small [kg \, m^{-3}] `$<br>
$`\small D`$	Effective dispersion coefficient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small q `$	Water flux density	$`\small [m \, d^{-1}] `$<br>
$`\small i`$	Soil layer	 <br>
$`\small z`$	Soil depth	$`\small [m] `$<br>
$`\small t `$	Time	$`\small [d] `$<br>

In case of high water fluxes, the time step is reduced for numerical stability reasons.

$`\small t=1.0`$	for q < 5.0 mm d–1	$`\small [d] `$<br>
$`\small t=0.5`$	for q > 5.0 mm d–1 und q < 10.0 mm d–1	$`\small [d] `$<br>
$`\small t=0.25`$	for q > 10.0 mm d––1 und q < 15.0 mm d–1	$`\small [d] `$<br>
$`\small t=0.125`$	for q > 15.0 mm d–1	$`\small [d] `$<br>
 