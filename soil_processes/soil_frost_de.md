# Bodenfrost

Bodenfrostalgorithmen stammen von Olsen and Haugen (Olsen and Haugen, 1997), wobei gleichförmige thermale Eigenschaften über das gesamte Bodenprofil angenommen werden. Es wird dabei nicht auf die im Bodentemperaturmodul berechneten Temperaturen zurückgegriffen, da dieses den Wärmetransfer im Phasenübergang Wasser – Eis nicht berücksichtigt. Parameterwerte sind dem SOIL-Modell (Jansson, 1991) entnommen. Das Modul benötigt die durch die Schneedecke modifizierte Oberflächentemperatur als Eingangsinformation. Der Ansatz zur Beschreibung des Auftauens ist der des ECOMAG-Modells (Motovilov et al., 1999).

Für die Berechnung der Wärmeleitfähigkeit des gefrorenen Bodens wird ein mittlerer Wassergehalt bei Feldkapazität angenommen. Nach Hanson et al. (2004) wird dieser Wassergehalt korrigiert um

$`\small \theta_{FCmf} = \theta_{FCm} + (1 + \theta_{FCm} \cdot (13.05 \cdot \theta^{1.06}_{FCm}))  `$

$`\small \theta_{FCmf}`$	Mittlerer Wassergehalt bei Feldkapazität (gefroren)	$`\small [m^3 \, m^{-3}] `$<br>
$`\small \theta_{FCm}`$	Mittlerer Wassergehalt bei Feldkapazität	$`\small [m^3 \, m^{-3}] `$<br>

Das Verhältnis des Wärmezuflusses aus dem Unterboden zur latenten Wärme ergibt sich aus

$`\small q_Q = \frac{0.3 \cdot d_F}{Q_I} `$

$`\small Q_I = 335 \cdot \theta_{FCm}`$

$`\small q_Q`$	Verhältnis des Wärmezuflusses vom Unterboden zur latenten Wärme	 <br>
$`\small Q_I `$	latente Wärme	$`\small [MJ] `$<br>
$`\small d_F`$	Anzahl Frosttag	$`\small [d] `$<br>
$`\small \theta_{FCm}`$	Mittlerer Wassergehalt bei Feldkapazität	$`\small [m^3 \, m^{-3}] `$<br>

Die Frosttiefe wird anschließend ermittelt aus

$`\small z_F = \sqrt{\left( \frac{q_Q}{2}  \right )^2 + 2 \cdot \theta_{FCmf} \ cdot \frac{dd_n}{Q_I} - \frac{q_Q}{2}  } `$

$`\small z_F`$	Frosttiefe	$`\small [m] `$<br>
$`\small q_Q `$	Verhältnis des Wärmezuflusses vom Unterboden zur latenten Wärme	 <br>
$`\small \theta_{FCmf}`$	Mittlerer Wassergehalt bei Feldkapazität (gefroren)	$`\small [m^3 \, m^{-3}] `$<br>
$`\small Q_I`$	latente Wärme	$`\small [MJ] `$<br>
$`\small dd_n`$	Summe negativer Gradtage seit Frostbegin	$`\small [^{\circ} C \, d] `$<br>

Für den Fall von Bodenfrost wird das tägliche Fortschreiten der Taufront in die Tiefe berechnet:

$`\small \Delta z_T = \begin{cases}  \sqrt{\frac{2 \cdot \lambda_h \cdot | T_s | } {79000 \cdot \theta_{FC}}} & T_s \geq 0^{\circ}C    \\ -  \sqrt{\frac{2 \cdot \lambda_h \cdot | T_s | } {79000 \cdot \theta_{FC}}}  & T_s < 0^{\circ}C  \end{cases}`$

$`\small \Delta z_T`$	Tiefeninkrement der Taufront	$`\small [m] `$<br>
$`\small \lambda_h`$	Wärmeleitfähigkeit des Bodens	$`\small [J \, m^{-1} \, s^{-1} \, K^{-1}] `$<br>
$`\small T_s`$	Temperatur unter der Schneedecke	$`\small [^{\circ} C] `$<br>
$`\small \theta_{FC}`$	Mittlerer Wassergehalt bei Feldkapazität	$`\small [m^3 \, m^{-3}] `$<br>

mit

$`\small T_s = \frac{T_a} {1+\frac{(10 \cdot S)}{z_F}} `$

$`\small T_s`$	Temperatur unter der Schneedecke	$`\small [^{\circ} C] `$<br>
$`\small S`$	Schneehöhe	$`\small [m] `$<br>
$`\small T_a`$	Tagesmitteltemperatur	$`\small [^{\circ} C] `$<br>
$`\small z_F`$	Frosttiefe	$`\small [m] `$<br>

Es wird angenommen, dass im Falle von Bodenfrost keine Wasserbewegungen in der betroffenen Bodenschicht stattfinden ($`\small\lambda=0`$). Infiltrierendes Wasser aus Niederschlag oder aus dem schmelzenden Schneespeicher dringt in den Boden bis zur Sättigungsgrenze. Überschüssiges Wasser wird im Oberflächenspeicher zurückgehalten oder entsprechend dem Oberflächenabfluss zugeführt. Wenn der Boden wieder aufgetaut ist, d.h. wenn die Taufront die Frosttiefe erreicht hat (zT >= zF), kann das Gravitationswasser das Profil passieren und Wasser aus dem Oberflächenspeicher nachlaufen. Die Variablen zT, zF, ddn und dF werden wieder auf Null gesetzt.

## Literatur

* Hansson, K., Simunek, J., Mizoguchi, M., Lundin, L.C., van Genuchten, M.T. (2004): Water flow and heat transport in frozen soil: Numerical solution and freeze-thaw applications. Vadose Zone Journal 3, 693-704.

* Jansson, P.E. (1991): The SOIL model - Users manual. 91-6. Swedish University of Agricultural Sciences, Uppsala.

* Motovilov, Y. G., Gottschalk, L., Engeland, K., Belokurov, A. (1999): ECOMAG: Regional model of hydrological cycle - Application to the NOPEX region.  1051. Dept. Geophysics, University of Oslo, Oslo, pp. -88.

* Olsen, P. A., Haugen, L. E. (1997): Jordas termiske egenskaper.  8. Dept. Soil and Water Science, Agricultural University of Norway, Ås, pp. -14.