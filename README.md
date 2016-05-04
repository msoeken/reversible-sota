# State-of-the-art in reversible logic synthesis

## Gate libraries

## Synthesis algorithms

### Exact methods
Exact synthesis methods guarantee minimality in number of gates (time) and number of lines (space).

#### SAT-based exact synthesis
Given a truth table of a Boolean function *f*, the decision problem “*Does there exist a reversible circuit with k gates that represents f?*” is translated into a SAT problem. A circuit can be extracted from a satisfying assignment to the problem. Asking the question, starting from *k* being 0 and incrementing it until the problem is satisfiable, gives the gate-optimum circuit.

**Input representations:** truth table

**Gate libraries:** MCT, MPMCT

**Implementations:** [RevKit](https://github.com/msoeken/cirkit/blob/master/addons/cirkit-addon-reversible/src/reversible/synthesis/exact_synthesis.cpp) (command: `exs --mode 1`)

**References:**
* [D. Große, R. Wille, G.W. Dueck, and R. Drechsler: Exact multiple-control Toffoli network synthesis with SAT techniques, in: *IEEE Trans. on CAD* **28**, 2009, 703&ndash;715.](http://dx.doi.org/10.1109/TCAD.2009.2017215)  
* [R. Wille, M. Soeken, N. Przigoda, R. Drechsler: Effect of negative control lines on the exact synthesis of reversible circuits, in: *Multiple-valued Logic and Soft Computing* **21**, 2013, 627-640.](http://www.oldcitypublishing.com/MVLSC/MVLSCabstracts/MVLSC21.5-6abstracts/MVLSCv21n5-6p627-640Wille.html)

### Ancilla-free methods
Ancilla-free synthesis methods guarantee minimality in number of lines (space).

### Hierarchical methods
Hierachical methods do neither guarantee optimality for number of gates nor for number of lines.

## Optimization algorithms

