# Assimilate partitioning

Assimilates being produced in the photosynthesis module are distributed to the single plant organs. The partitioning coefficients are taken from a matrix which includes the development stages of the crop.

*Table 1: Matrix of assimilate partitioning for winter wheat*

| &nbsp;                             | Root  | Leaf  | Shoot  | Fruit  | Permanent structures  |
|------------------------------------|-------|-------|--------|--------|-----------------------|
| **Sowing to emergence**            | 0.5   | 0.5   | 0      | 0      | 0                     |
| **Emergence to double ridge**      | 0.2   | 0.2   | 0.6    | 0      | 0                     |
| **Double ridge to begin of bloom** | 0.13  | 0.2   | 0.67   | 0      | 0                     |
| **Bloom**                          | 0     | 0     | 0.03   | 0.97   | 0                     |
| **Grain filling**                  | 0     | 0     | 0      | 1.0    | 0                     |
| **Senescence**                     | 0     | 0     | 0      | 0      | 0                     |

The daily partitioning is calculated using linear regression between the elements of the matrix and the relative development:

$$a_i = a_{i,j} + ((a_{i, j+1} - a_{i,j}) \cdot DD_{rels})$$

and

$$DD_{rels} = \frac{DD_{acts}}{DD_{crops}}$$

$a_i$ Actual assimilate partitioning coefficient for organ $i$<br>
$a_{i,j}$ Partitioning coefficient at begin of stage $j$<br>
$a_{i,j+1}$	Partitioning coefficient at begin of the subsequent stage $j+1$<br>
$DD_{rels}$	Relative phenological development within the stage<br>
$DD_{acts}$	Actual temperature sum in the developmental stage $[^{\circ}C \, d]$<br>
$DD_{crops}$ Plant-specific temperature sum in the developmental stage $[^{\circ}C \, d]$<br>
 
The number of organs, developmental stages, as well as their names can be freely defined by the user.

The daily increase of assimilates in the respective organ results from:

$$\frac{d}{dt}W_i = A_g \cdot a_i \cdot dt$$

$W_i$ Biomass of organ $i$ $[kg \, TM \, ha^{-1}]$<br>
$A_g$ Gross CO<sub>2</sub> assimilation	$[kg \, CO_2 \, ha^{-1} \, d^{-1}]$<br>
$a_i$ Partitioning coefficient at begin of stage $j$<br>
$t$	Time $[d]$<br>
 
The daily mass decrease due to senescence of the single organs is calculated accordingly.