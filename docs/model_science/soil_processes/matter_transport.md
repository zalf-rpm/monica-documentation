# Matter transport

In soil, nutrients are transported with the moving water. So far, MONICA only describes the translocation of dissolved nitrate ions. However, using the convection-dispersion equation, modelling of ammonium, sulphate, and dissolved organic matter transport is also imaginable. The basic version of the convection-dispersion equation is:

$$\frac{\partial \theta c}{\partial t}  = \frac{\partial}{\partial z} \cdot \left ( \theta \cdot D \cdot \frac{\partial c}{\partial z}\right ) - \frac{\partial q c}{\partial z}$$

$\theta$ Soil moisture content $[m^{3} \, m^{-3}]$<br>
$c$	Solute concentration $[kg \, m^{-3}]$<br>
$D$	Effective dispersion coefficient $[m^{2} \, m^{-1}]$<br>
$z$	Soil depth $[m]$<br>
$t$	Time $[d]$<br>

which includes water flow:

$$q = -K (\Psi_m) \cdot \left ( \frac{\partial \Psi_m}{\partial z} - 1   \right)$$

$q$	Water flux density $[m \, d^{-1}]$<br>
$\Psi_m$ Soil matrix potential $[cm \, WS]$<br>
$K(\Psi_m)$	Hydraulic conductivity $[m \, d^{-1}]$<br>
$\theta$ Soil moisture content $[m^3 \, m^{-3}]$<br>
$z$	Soil depth $[m]$<br>

the effective dispersion coefficient:

$$D = D_0 \cdot \frac{1}{\tau} + \alpha_v \cdot \left |  \frac{q}{\theta} \right |$$

$D$	Effective dispersion coefficient $[m^2 \, d^{-1}]$<br>
$D_0$ Diffusion coefficient in solution (0.000214) $[m^2 \, d^{-1}]$<br>
$\tau$ Tortuosity $[m^2 \, m^{-2}]$<br>
$\alpha_v$ Dispersion factor (0.05) $[m]$<br>
$q$ Water flux density $[m \, d^{-1}]$<br>
$\theta$ Volumetric water content $[m^3 \, m^{-3}]$<br>

in which tortuosity is used as its inverse, pore space continuity.

$$\tau = \frac{\theta}{a \cdot e^{10 \cdot \theta}}$$

$\tau$ Tortuosity $[m^2 \, m^{-2}]$<br>
$\theta$ Volumetric water content $[m^3 \, m^{-3}]$<br>
$a$ Factor (0.002)<br>

The convection-dispersion equation is solved using the explicit finite difference method.

For $q1/2 > 0$:

$$\frac{ \theta_{i , t+\Delta t} \cdot c_{i, t+\Delta t} - \theta_{i, t} \cdot c_{i, t}}   {\Delta t} =$$

$$\left (   \theta_{i - \frac{1}{2}, t} \cdot D_{i-\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i-\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i-\frac{1}{2}, t}} {2 \cdot \theta_{i-\frac{1}{2}, t}} \right )   \cdot \frac {c_{i-1, t} - c_{i, t}} {(\Delta z)^2}$$

$$- \left ( \theta_{i+\frac{1}{2}, t} \cdot D_{i+\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i+\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i+\frac{1}{2},t}}  {2 \cdot \theta_{i+\frac{1}{2},t}}   \right ) \cdot \frac{c_{i,t} - c_{i+1,t}}{(\Delta z)^2}$$

$$-  \frac{q_{i+\frac{1}{2}, t} \cdot c_{i,t} - q_{i-\frac{1}{2}, t} \cdot c_{i-1,t} }{\Delta z}$$

and for $q1/2 < 0$:

$$\frac{ \theta_{i , t+\Delta t} \cdot c_{i, t+\Delta t} - \theta_{i, t} \cdot c_{i, t}}   {\Delta t} =$$

$$\left (   \theta_{i - \frac{1}{2}, t} \cdot D_{i-\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i-\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i-\frac{1}{2}, t}} {2 \cdot \theta_{i-\frac{1}{2}, t}} \right )   \cdot \frac {c_{i-1, t} - c_{i, t}} {(\Delta z)^2}$$

$$- \left ( \theta_{i+\frac{1}{2}, t} \cdot D_{i+\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i+\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i+\frac{1}{2},t}}  {2 \cdot \theta_{i+\frac{1}{2},t}}   \right ) \cdot \frac{c_{i,t} - c_{i+1,t}}{(\Delta z)^2}$$

$$-  \frac{q_{i+\frac{1}{2}, t} \cdot c_{i+1,t} - q_{i-\frac{1}{2}, t} \cdot c_{i,t} }{\Delta z}$$

$\theta$ Volumetric water content $[m^3 \, m^{-3}]$<br>
$c$	Solute concentration $[kg \, m^{-3}]$<br>
$D$	Effective dispersion coefficient $[m^2 \, d^{-1}]$<br>
$q$	Water flux density $[m \, d^{-1}]$<br>
$i$	Soil layer<br>
$z$	Soil depth $[m]$<br>
$t$ Time $[d]$<br>

In case of high water fluxes, the time step is reduced for numerical stability reasons.

$t=1.0$	for q < 5.0 mm d–1	$[d]$<br>
$t=0.5$	for q > 5.0 mm d–1 und q < 10.0 mm d–1 $[d]$<br>
$t=0.25$ for q > 10.0 mm d––1 und q < 15.0 mm d–1 $[d]$<br>
$t=0.125$ for q > 15.0 mm d–1 $[d]$<br>