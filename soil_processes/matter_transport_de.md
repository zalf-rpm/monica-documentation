# Stofftransport

Mit dem sich im Boden bewegenden Wasser werden auch Nährstoffe transportiert. MONICA beschreibt derzeit ausschließlich den Transport von gelösten Nitrat-Ionen, jedoch sind Erweiterungen für Ammonium und Sulfat sowie gelöste organische Verbindungen denkbar. Stofftransport wird in MONICA mit Hilfe der Konvektions-Dispersion-Gleichung beschrieben:

$`\small \frac{\partial \theta c}{\partial t}  = \frac{\partial}{\partial z} \cdot \left ( \theta \cdot D \cdot \frac{\partial c}{\partial z}\right ) - \frac{\partial q c}{\partial z} `$

$`\small \theta `$	Wassergehalt des Bodens	$`\small [m^{3} \, m^{-3}] `$<br>
$`\small c`$	Stoffkonzentration in Lösung	$`\small [kg \, m^{-3}] `$<br>
$`\small D`$	Dispersionskoeffizient	$`\small [m^{2} \, m^{-1}] `$<br>
$`\small z`$	Bodentiefe	$`\small [m] `$<br>
$`\small t`$	Zeit	$`\small [d] `$<br>

mit dem Wasserfluss

$`\small q = -K (\Psi_m) \cdot \left ( \frac{\partial \Psi_m}{\partial z} - 1   \right)`$

$`\small q`$	Wasserflussdichte	$`\small [m \, d^{-1}] `$<br>
$`\small \Psi_m`$	Matrixpotenzial des Bodens	$`\small [cm \, WS] `$<br>
$`\small K(\Psi_m)`$	Hydraulische Leitfähigkeit	$`\small [m \, d^{-1}] `$<br>
$`\small \theta`$	Wassergehalt des Bodens	$`\small [m^3 \, m^{-3}] `$<br>
$`\small z`$	Bodentiefe	$`\small [m] `$<br>

dem Dispersionskoeffizienten

$`\small D = D_0 \cdot \frac{1}{\tau} + \alpha_v \cdot \left |  \frac{q}{\theta} \right | `$

$`\small D`$	Dispersionskoeffizient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small D_0`$	Diffusionskoeffizient (0.000214)	$`\small [m^2 \, d^{-1}] `$<br>
$`\small \tau`$	Tortuosität	$`\small [m^2 \, m^{-2}] `$<br>
$`\small \alpha_v `$	Dispersionslänge (0.0049)	$`\small [m] `$<br>
$`\small q`$	Wasserflussdichte	$`\small [m \, d^{-1}] `$<br>
$`\small \theta `$	Wassergehalt des Bodens	$`\small [m^3 \, m^{-3}] `$<br>

und der Porenkontinuität, die als Kehrwert der Tortuosität beschrieben wird.

$`\small \tau = \frac{\theta}{a \cdot e^{10 \cdot \theta}}`$

$`\small \tau `$	Tortuosität	$`\small [m^2 \, m^{-2}] `$<br>
$`\small \theta`$	Wassergehalt des Bodens	$`\small [m^3 \, m^{-3}] `$<br>
$`\small a`$	Faktor (0.002)	 <br>

Zur Lösung der Konvektions-Dispersions-Gleichung wird das explizite "Finite Differenzen-Verfahren" angewendet.

Für q1/2 > 0

$`\small \frac{ \theta_{i , t+\Delta t} \cdot c_{i, t+\Delta t} - \theta_{i, t} \cdot c_{i, t}}   {\Delta t} = `$

$`\small \left (   \theta_{i - \frac{1}{2}, t} \cdot D_{i-\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i-\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i-\frac{1}{2}, t}} {2 \cdot \theta_{i-\frac{1}{2}, t}} \right )   \cdot \frac {c_{i-1, t} - c_{i, t}} {(\Delta z)^2}  `$

$`\small - \left ( \theta_{i+\frac{1}{2}, t} \cdot D_{i+\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i+\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i+\frac{1}{2},t}}  {2 \cdot \theta_{i+\frac{1}{2},t}}   \right ) \cdot \frac{c_{i,t} - c_{i+1,t}}{(\Delta z)^2}  `$

$`\small -  \frac{q_{i+\frac{1}{2}, t} \cdot c_{i,t} - q_{i-\frac{1}{2}, t} \cdot c_{i-1,t} }{\Delta z}  `$

und für q1/2 < 0

$`\small \frac{ \theta_{i , t+\Delta t} \cdot c_{i, t+\Delta t} - \theta_{i, t} \cdot c_{i, t}}   {\Delta t} = `$

$`\small \left (   \theta_{i - \frac{1}{2}, t} \cdot D_{i-\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i-\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i-\frac{1}{2}, t}} {2 \cdot \theta_{i-\frac{1}{2}, t}} \right )   \cdot \frac {c_{i-1, t} - c_{i, t}} {(\Delta z)^2}  `$

$`\small - \left ( \theta_{i+\frac{1}{2}, t} \cdot D_{i+\frac{1}{2}, t} - \frac{\Delta z}{2} \cdot q_{i+\frac{1}{2}, t} + \frac{\Delta t \cdot q_{i,t} \cdot q_{i+\frac{1}{2},t}}  {2 \cdot \theta_{i+\frac{1}{2},t}}   \right ) \cdot \frac{c_{i,t} - c_{i+1,t}}{(\Delta z)^2}  `$

$`\small -  \frac{q_{i+\frac{1}{2}, t} \cdot c_{i+1,t} - q_{i-\frac{1}{2}, t} \cdot c_{i,t} }{\Delta z}  `$

$`\small \theta `$	Wassergehalt des Bodens	$`\small [m^3 \, m^{-3}] `$<br>
$`\small c `$	Stoffkonzentration in Lösung	$`\small [kg \, m^{-3}] `$<br>
$`\small D`$	Dispersionskoeffizient	$`\small [m^2 \, d^{-1}] `$<br>
$`\small q `$	Wasserflussdichte	$`\small [m \, d^{-1}] `$<br>
$`\small i`$	Bodenschicht	 <br>
$`\small z`$	Bodentiefe	$`\small [m] `$<br>
$`\small t `$	Zeit	$`\small [d] `$<br>

Wenn hohe Wasserflüsse auftreten, wird aus Gründen der numerischen Stabilität der Zeitschritt verkleinert.

$`\small t=1.0`$	für q < 5.0 mm d–1	$`\small [d] `$<br>
$`\small t=0.5`$	für q > 5.0 mm d–1 und q < 10.0 mm d–1	$`\small [d] `$<br>
$`\small t=0.25`$	für q > 10.0 mm d––1 und q < 15.0 mm d–1	$`\small [d] `$<br>
$`\small t=0.125`$	für q > 15.0 mm d–1	$`\small [d] `$<br>
 