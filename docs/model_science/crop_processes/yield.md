# Yield

A crop’s marketable yield is currently calculated from the simulated dry matter of the storage organ:

$$E_{FM} = W_s \cdot HI \cdot DM$$

$E_{FM}$ Fresh matter yield	$[kg \, FM \, m^{-2}]$<br>
$W_s$ Dry matter storage organ $[kg \, DM \, m^{-2}]$<br>
$HI$ Harvest index<br>
$DM$ Dry matter content	$[kg \, DM \, kg \, FM^{-1}]$<br>

Raw protein content in grain yield is calculated from N in grain yield. N in grain yield is calculated from the ratio of marketable and non-marketable yield and a plant-specific parameter which determines the N distribution between those two compartments:

$$P_{FM} = \frac{N_A} {W_s + (p_N \cdot W_R)} \cdot \frac{1}{N_p}$$

$P_{FM}$ Raw protein content in dry matter yield $[kg \, Raw \, protein \, kg \, DM^{-1} ]$<br>
$N_A$ N concentration in above-ground dry matter $[kg \, N \, m^{-2}]$<br>
$W_s$ Dry matter storage organ $[kg \, DM \, m^{-2}]$<br>
$p_N$ N distribution coefficient<br>
$W_R$ Dry matter above-ground crop residues	$[kg \, DM \, m^{-2}]$<br>
$N_p$ Average N concentration raw protein (= 0.16)	$[kg \, N \, kg \, Raw \, protein^{-1}]$<br>

where

$$p_N = \frac{N_R}{N_Y}$$

$p_N$ N distribution coefficient<br>
$N_R$ Average N concentration in crop residues $[kg \, N \, kg \, DM^{-1}]$<br>
$N_Y$ Average N concentration in marketable yield<br>
 