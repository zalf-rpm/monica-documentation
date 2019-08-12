# Yield

A crop’s marketable yield is currently calculated from the simulated dry matter of the storage organ

$`\small E_{FM} = W_s \cdot HI \cdot DM `$

$`\small E_{FM}`$	Fresh matter yield	$`\small [kg \, FM \, m^{-2}] `$<br>
$`\small W_s`$	Dry matter storage organ	$`\small [kg \, DM \, m^{-2}] `$<br>
$`\small HI`$	Harvest index	 <br>
$`\small DM`$	Dry matter content	$`\small [kg \, DM \, kg \, FM^{-1}] `$<br>

Raw protein content in grain yield is calculated from N in grain yield. N in grain yield is calculated from the ratio of marketable and non-marketable yield and a plant-specific parameter which determines the N distribution between those two compartments.

$`\small P_{FM} = \frac{N_A} {W_s + (p_N \cdot W_R)} \cdot \frac{1}{N_p}   `$

$`\small P_{FM} `$	Raw protein content in dry matter yield	$`\small [kg \, Raw \, protein \, kg \, DM^{-1} ] `$<br>
$`\small N_A`$	N concentration in above-ground dry matter	$`\small [kg \, N \, m^{-2}] `$<br>
$`\small W_s`$	Dry matter storage organ	$`\small [kg \, DM \, m^{-2}] `$<br>
$`\small p_N`$	N distribution coefficient	 <br>
$`\small W_R`$	Dry matter above-ground crop residues	$`\small [kg \, DM \, m^{-2}] `$<br>
$`\small N_p`$	Average N concentration raw protein (= 0.16)	$`\small [kg \, N \, kg \, Raw \, protein^{-1}] `$<br>

where

$`\small p_N = \frac{N_R}{N_Y}`$

$`\small p_N `$	N distribution coefficient	 <br>
$`\small N_R`$	Average N concentration in crop residues	$`\small [kg \, N \, kg \, DM^{-1}] `$<br>
$`\small N_Y`$	Average N concentration in marketable yield<br>
 