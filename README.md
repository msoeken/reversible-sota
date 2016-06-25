# State-of-the-art in reversible logic synthesis

#### Table of Contents

- [Overview](#overview)
- [Gate libraries](#gate-libraries)
	- [NCT (Not, CNOT, Toffoli)](#nct-not-cnot-toffoli)
- [Embedding](#embedding)
- [Synthesis algorithms for reversible functions](#synthesis-algorithms-for-reversible-functions)
	- [Functional exact algorithms](#functional-exact-algorithms)
		- [SAT-based synthesis](#sat-based-synthesis)
		- [Enumerative synthesis](#enumerative-synthesis)
	- [Functional heuristic algorithms](#functional-heuristic-algorithms)
		- [Transformation-based synthesis](#transformation-based-synthesis)
		- [Decomposition-based synthesis](#decomposition-based-synthesis)
		- [Metehauristic synthesis](#metaheuristic-synthesis)
		- [Greedy synthesis](#greedy-synthesis)
- [Synthesis algorithms for nonreversible functions](#synthesis-algorithms-for-nonreversible-functions)
	- [Structural algorithms](#structural-algorithms)
		- [Hierarchical synthesis](#hierarchical-synthesis) 
- [Historical remarks](#historical-remarks)

## Overview

![Overview](data/overview.png)

We classify reversible synthesis algorithms using 4 levels of categorization:

1. **Reversibility of the input**: we distinguish between nonreversible and reversible functions.  Embedding is not part of this categorization and need to be applied as a preprocess if needed. Columns in the overview figure refer to this level.
2. **Manipulation of the input**: if the synthesis algorithm modifies the input during execution or emphasizes on the underlying function of the input rather than its structure, we refer to the synthesis approaches as *functional synthesis*, otherwise as *structural synthesis*.  Functional synthesis algorithms guarantee optimum lines during synthesis and may also allow optimum number of gates. Rows in the overview figure refer to this level.
3. **Algorithm**: For each category several algorithms may be proposed which differ in their conceptual methodology. Items in boxes refer to this level.
4. **Implementation**: Each algorithm can be implemented in several different ways using several different data structures for the input representation.  This level is not visible in the overview figure but discussed in the following text.

**Example**: The original transformation-based synthesis algorithm from D.M. Miller, D. Maslov and G.W. Dueck [[*DAC* **40**, 2003, 318-323.]](http://dl.acm.org/citation.cfm?doid=775832.775915) starts from a *reversible input function* (1) and it is *functional* (2).  Obviously, it falls in the category of *transformation-based synthesis* algorithms (3) and the specific works marks one of many implementations in this category (4).

## Gate libraries

### NCT (Not, CNOT, Toffoli)

| NOT                  | CNOT                   | Toffoli                      |
| -------------------- | ---------------------- | ---------------------------- |
| ![NOT](data/not.png) | ![CNOT](data/cnot.png) | ![Toffoli](data/toffoli.png) |

The NCT library consists of the three gates NOT, CNOT, and Toffoli.  The Toffoli gate is a double-controlled NOT gate. These gates are universal in a sense, that every Boolean function can be realized if an arbitrary number of variables (lines) is permitted.  Given *n* variables, the NCT gate library can realize all permutations over *n* ≤ 3 variables and all even permutations over *n* > 3 variables.

## Embedding

## Synthesis algorithms for reversible functions

### Functional exact algorithms
Exact synthesis algorithms guarantee minimality in number of gates (time) and number of lines (space).

#### SAT-based synthesis
Given a truth table of a Boolean function *f*, the decision problem “*Does there exist a reversible circuit with k gates that represents f?*” is translated into a SAT problem. A circuit can be extracted from a satisfying assignment to the problem. Asking the question, starting from *k* being 0 and incrementing it until the problem is satisfiable, gives the gate-optimum circuit.

**Input representations:** truth table

**Gate libraries:** MCT, MPMCT

**Implementations:** [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/exact_synthesis.cpp) (command: `exs --mode 1`), [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/quantified_exact_synthesis.cpp) (command: `exs --mode 0`)

**References:**
* [D. Große, R. Wille, G.W. Dueck, and R. Drechsler: Exact multiple-control Toffoli network synthesis with SAT techniques, in: *IEEE Trans. on CAD* **28**, 2009, 703&ndash;715.](http://dx.doi.org/10.1109/TCAD.2009.2017215)  
* [R. Wille, M. Soeken, N. Przigoda, and R. Drechsler: Effect of negative control lines on the exact synthesis of reversible circuits, in: *Multiple-valued Logic and Soft Computing* **21**, 2013, 627-640.](http://www.oldcitypublishing.com/MVLSC/MVLSCabstracts/MVLSC21.5-6abstracts/MVLSCv21n5-6p627-640Wille.html)
* [R. Wille, H.M. Le, G.W. Dueck, and D. Große: Quantified Synthesis of Reversible Logic, in: *DATE*, 2008, 1015-1020.](http://dx.doi.org/10.1109/DATE.2008.4484814)

#### Enumerative synthesis
**TODO**

**Input representations:** truth table

**Gate libraries:** MCT, MPMCT

**References:**
* [O. Golubitsky, S.M. Falconer, and D. Maslov: Synthesis of the optimal 4-bit reversible circuits, in *DAC* **47**, 2010, 653-656.](http://doi.acm.org/10.1145/1837274.1837440)

  **TODO**
* [O. Golubitsky and D. Maslov: A study of optimal 4-Bit reversible Toffoli circuits and their synthesis, in: *IEEE Trans. Computers* **61**, 2012, 1341-1353.](http://dx.doi.org/10.1109/TC.2011.144)

  **TODO**
* [M. Szyprowski and P. Kerntopf: Optimal 4-bit reversible mixed-polarity Toffoli circuits, in *RC* **4**, 2012, 138-151.](http://dx.doi.org/10.1007/978-3-642-36315-3_11)

  **TODO**

### Functional heuristic algorithms
Functional heuristic synthesis algorithms guarantee minimality in number of lines (space).

#### Transformation-based synthesis
Starting from a reversible function, transformation-based synthesis applies gates and adjusts the function representation accordingly in a way that each gate application gets the function closer to the identity function.  If the identity function has been reached, all applied gates make up for the circuit that realizes the initial function.

**Input representations:** truth table, RCBDD

**Gate libraries:** MCT, MPMCT (only negative controls), MCT+F

**Implementations:** [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/transformation_based_synthesis.cpp) (command: `tbs`), [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/symbolic_transformation_based_synthesis.cpp) (command: `tbs -b` and `tbs -s`)

**References:**
* [D.M. Miller, D. Maslov, and G.W. Dueck: A transformation based algorithm for reversible logic synthesis, in: *DAC* **40**, 2003, 318-323.](http://dl.acm.org/citation.cfm?doid=775832.775915)

  This paper first introduced the transformation based synthesis algorithm.  It descibes first the basic unidirectional version but also the bidirectional variant which often results in circuits of smaller size.
* [M. Soeken, G.W. Dueck, and D.M. Miller: A fast symbolic transformation based algorithm for reversible logic synthesis, in: *RC* **8**, 2016.](http://msoeken.github.io/papers/2016_rc_1.pdf)
* [M. Soeken and A. Chattopadhyay: Fredkin-enabled transformation-based reversible logic synthesis, in: *ISMVL* **46**, 2015, 60-65.](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=7238133)
* [M. Soeken, R. Wille, C. Hilken, N. Przigoda, and R. Drechsler: Synthesis of reversible circuits with minimal lines for large functions, in: *ASP-DAC* **17**, 2012, 85-92.](http://dx.doi.org/10.1109/ASPDAC.2012.6165069)
* [D. Maslov, G.W. Dueck, and D.M. Miller: Toffoli network synthesis with templates, in: *IEEE Trans. on CAD of Integrated Circuits and Systems* **24**, 2005, 807-817.](http://dx.doi.org/10.1109/TCAD.2005.847911)
* [D. Maslov, G.W. Dueck, and D.M. Miller:
Synthesis of Fredkin-Toffoli reversible networks, in: *IEEE Trans. VLSI Syst.* **13**, 2005, 765-769.](http://dx.doi.org/10.1109/TVLSI.2005.844284)

#### Decomposition-based synthesis
In decomposition-based synthesis the reversible function is iteratively decomposed into simpler functions based on the  *Young subgroup decomposition*: Given a line *i*, every reversible function *f* can be decomposed into three functions *f = g<sub>1</sub>* ○ *f'* ○ *g<sub>2</sub>*, where *g<sub>1</sub>* and *g<sub>2</sub>* can be realized with a single-target gate on line *i* and *f'* is a reversible function that does not change in line *i*. Based on this decomposition, synthesis algorithms determine the gates for *g<sub>1</sub>* and *g<sub>2</sub>* and then recur on *f'*.

**Input representations:** truth table, RCBDD

**Gate libraries:** STG

**Implementations:** [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/young_subgroup_synthesis.cpp) (command: `dbs`), [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/rcbdd_synthesis.cpp) (command: `dbs -s`)

**References:**
* [A. De Vos and Y. Van Rentergem: Young subgroups for reversible computers, in: *Adv. in Math. of Comm.* **2**, 2008, 183-200.](http://dx.doi.org/10.3934/amc.2008.2.183)
* [M. Soeken, L. Tague, G.W. Dueck, and R. Drechsler: Ancilla-free synthesis of large reversible functions using binary decision diagrams, in: *J. Symb. Comput.* **73**, 2016, 1-26.](http://dx.doi.org/10.1016/j.jsc.2015.03.002)

#### Metaheuristic synthesis
Synthesis in these category synthesize a circuit based on a metaheuristic such as genetic algorithms.

**Input representation:** truth table

**Gate libraries:** MCT

**References:**
* [F.Z. Hadjam and C. Moraga: RIMEP2: Evolutionary design of reversible digital circuits, in *JETC* **11**, 2014, 27:1-27:23.](http://dl.acm.org/citation.cfm?doid=2629534)

#### Greedy synthesis
Greedy synthesis is similar to transformation-based synthesis.  At each step it applies a set of gates to the current function to be synthesized and chooses the gate that brings the function closest to the identity function.

**Input representation:** BDD

**Gate libraries:** arbitrary

**References:**
* [P. Kerntopf: A new heuristic algorithm for reversible logic synthesis, in: *DAC* **41**, 2004, 834-837.](http://dl.acm.org/citation.cfm?id=996789)

## Synthesis algorithms for nonreversible functions

### Structural algorithms
Structural algorithms do neither guarantee optimality for number of gates nor for number of lines.

#### Hierarchical synthesis
In hierarchical synthesis the function is represented in a structural way, e.g., using a logic network. Then, small subparts of the structure are considered functionally, embedded into reversible functions and synthesized using functional algorithms.  The resulting reversible circuits are combined with respect to the structural representation of the function.  This combination of subcircuits leads to an additional number of lines, which are essentially required to store intermediate computation steps.

**Input representation:** BDD, AIG

**Gate libraries:** imposed by the underlying functional synthesis algorithm

**Implementations:** [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/bdd_synthesis.cpp) (command: `hdbs`), [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/cut_based_synthesis.cpp) (command: `cbs`)

**References:**
* [R. Wille and R. Drechsler: BDD-based synthesis of reversible logic for large functions, in: *DAC* **46**, 2009, 270-275.](http://doi.acm.org/10.1145/1629911.1629984)
* [M. Soeken and A. Chattopadhyay: Unlocking efficiency and scalability of reversible logic synthesis using conventional logic synthesis, in: *DAC* **53**, 2016.](http://msoeken.github.io/papers/2016_dac_2.pdf)
* [A. Chattopadhyay, A. Littarru, L.G. Amarù, P.-E. Gaillardon, and G. De Micheli:
Reversible logic synthesis via biconditional binary decision diagrams, in: *ISMVL* **45**, 2015, 2-7.](http://dx.doi.org/10.1109/ISMVL.2015.21)
* [M. Krishna and Anupam Chattopadhyay: Efficient reversible logic synthesis via isomorphic subgraph matching, in: *ISMVL* **44**, 2014, 103-108.](http://dx.doi.org/10.1109/ISMVL.2014.26)
* [M. Soeken, R. Wille, and R. Drechsler:
Hierarchical synthesis of reversible circuits using positive and negative Davio decomposition, in: *IDT* **5**, 2010, 143-148.](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=5724427)

## Historical remarks
**TODO**
