# GM network modeling
Gut microbiota (GM) is a micro-ecosystem formed by a large number of microorganisms through competition and cooperation. The project constructs a complex network-based GM modeling approach to evaluate the topological features of the GM micro-ecosystem and provides computational simulations for users to infer the window period and bacterial candidates for GM intervention during disease progression. There are three major parts concluded in the program.<br>
* Network stability assessment<br>
* Network vulnerability assessment<br>
* Network robustness assessment<br>
# Algorithm and Usage
## Network stability assessment  
First, we detect the effect of the nodes in a network by adopting the abundance-weighted mean interaction strength ($wMIS_i$) index before evaluating the stability of the GM networks. The $wMIS_i$ index can be calculated for each node in a network with the following formula.<br>

$$ wMIS_i = \frac{\sum_{{j}\ne{i}}b_j|R_{i,j}|}{\sum_{{j}\ne{i}}b_j} $$

where $i$ stands for a node in a network, $j$ stands for the node connected to node $i$, $bj$ stands for the relative abundance of node $j$, and $R_{i,j}$ stands for the Spearman correlation coefficient between node $i$ and $j$. Here we could get the $wMIS_i$ for the nodes with the following script.<br>

```
perl get_MIS.pl <Averaged.abundances.for.microorganisms.csv> <Microbial.correlation.matrix.csv> <MIS.output>
```

* <Averaged.abundances.for.microorganisms.csv>: the file contains the averaged abundances for the nodes;<br>
* <Microbial.correlation.matrix.csv>: the file contains the correlation matrix among the nodes;<br>
* <MIS.output>: the output file contains $wMIS_i$ index for each node in the network.<br>

Second, we detect the core nodes in the networks, and the core nodes were defined as the consistent bacteria that existed in the GM networks across different time points.<br>

```
perl get_common_taxo.pl <Averaged_abundance.lst> <Common_tax.output>
```

* <Averaged_abundance.lst>:the taxonomical abundance file list obtained from different time points;<br>
* <Common_tax.output>: the output file contains common taxon.<br>

Lastly, we calculate the stability of the networks (S_a) with the $wMIS_i$ through the following formula.

$$ S_a = \frac{\sum_{i=1}^m wMIS_i}{\sum_{j=1}^n wMIS_j} $$

$i$ stands for a core node in the network $a$, $j$ stands for a node in the network $a$, $m$ stands for the number of core nodes in the network $a$, and $n$ stands for the number of a nodes in the network $a$. The stability of the networks can be calcuated with the follwing script.<br>

```
perl get_stability.pl <MIS.output.lst> <Common_tax.output> <Stability.output>
```

* <MIS.output.lst>: the *wMIS* file list obtained for the networks at different time points;<br>
* <Common_tax.output>: the output file contains common taxon;<br>
* <Stability.output>: the stability of the networks.<br>

## Network vulnerability assessment
The vulnerability of the GM networks was measured by adopting the maximal global efficacy decreasing ratio (mEDR). <br>
First, we need to calculate the averaged efficacy of a network ($E_a$) before calculating the mEDR, detecting the transferring speed of information in the network. $E_a$ is calculated with the following formula.<br>

$$ E_a = \frac{n}{n(n-1)}\sum_{{i}\ne{j}}\frac{1}{d_{i,j}} $$

where $n$ stand for the number of nodes in the network $a$, and $d_{i,j}$ stands for the number of edges in the shortest path between node $i$ and $j$.<br> 

Then, we removed the node from the network one by one, evaluated the altered $E_a$ after the node removal, and selected the maximal EDR as the mEDR through the following formula.<br>

$$ mEDR = max(\frac{E_a - E_{a}^'}{E_a}) $$

The $E_a$ for the nodes and the EDR can be calculated with the following script.<br>

```
perl get_efficiency.pl <Shortest.path.csv> <MIS.output> <EDR.output>
```

* <Shorest.path.csv>: the shortest paths between every two nodes;<br>
* <MIS.output>: the $wMIS_i$ index file obtained from the stability assessment;<br>
* <EDR.output>: the EDR for the network after the node removal.<br>



