---
permalink: /energy/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Energy

The system energy is described by a Hamiltonian where an arbitrary number of potential energy terms can be added,

$$\mathcal{H}_{sys} = U_1 + U_2 + ... $$

The energy terms are given as a list to `energy` in the top level input:

~~~ yaml
energy:
    - nonbonded_coulomblj:
        lennardjones: {mixing: LB}
        coulomb: {type: plain, epsr: 80, cutoff: 10}
    - ...
~~~

## Nonbonded Interactions

This term loops over pairs of atoms, $$i$$, and $$j$$, summing a given pair-wise additive potential, $$u_{ij}$$,

$$ U_{NB} = \sum_i\sum_j u(\textbf{r}_{ij})$$

Note: the pair-potential can be a combination of several potentials, but this must currently be defined at _compile
time_ and cannot be arbitrarily selected from input. Predefined combinations:
- `nonbonded_coulomblj` &#8594; `coulomb`+`lennardjones`
{: .notice--info}

Predefined pair-potential combinations:

`energy`               |  Pair-potential(s)
---------------------- |  -------------------------
`nonbonded_coulomblj`  |  `coulomb`+`lennardjones`

Below is a description of available pair-potentials and their configuration.

### Electrostatics

This is a multipurpose pair potential that handles several variants of electrostatic
pair-potentials. Beyond a spherical cutoff, $$R_c$$, the potential is zero while if
below,

$$ u_{ij} = \frac{q_i q_j }{ 4\pi\epsilon_0\epsilon_r r_{ij} }\mathcal{S}(q)$$

where $$q=r/R_c$$, and $$\mathcal{S}(q)$$ is a splitting function defined below.
The following keywords are _required_:

 `coulomb`   |  Description
 ----------- |  -------------------------------------------------
 `type`      |  Type of potential as defined above
 `cutoff`    |  Spherical cutoff after which the potential is zero
 `epsr`      |  Relative dielectric constant of the medium

The splitting function is selected with `type` that can be any of the following:

 `type`          | $$\mathcal{S}(q=r/R_c)$$               | Additional keywords | Comment
 --------------- | -------------------------------------- | ------------------- | ----------------------
 `plain`         | $$ 1 $$                                |                     | [doi](http://doi.org/ctnnsj)
 `none`          | $$ 0 $$                                |                     | 
 `wolf`          | $$ erfc(\alpha R_c q)-erfc(\alpha R_c)q $$ | `alpha`         | [doi](http://doi.org/cfcxdk)
 `fennel`        | $$ \scriptstyle erfc(\alpha R_c q)-erfc(\alpha R_c)q + ( q -1 ) q \left( erfc(\alpha R_c) + \frac{2\alpha R_c}{\sqrt{\pi}} e^{-\alpha^2 R_c^2} \right) $$ | `alpha`| [doi](http://doi.org/bqgmv2)
 `yonezawa`      | $$ 1 + erfc(\alpha R_c)q + q^2 $$      | `alpha`             | [doi](http://dx.doi.org/10/j97)
 `fanourgakis`   | $$ 1-\frac{7}{4}q+\frac{21}{4}q^5-7q^6+\frac{5}{2}q^7$$|     | [doi](http://doi.org/f639q5)
 `qpotential`    | $$ \prod_{n=1}^{order}(1-q^n) $$       | `order=300`         | [ISBN](http://goo.gl/hynRTS) (Paper V)
 `reactionfield` | $$ 1 + \frac{\epsilon_{RF}-\epsilon_{r}}{2\epsilon_{RF}+\epsilon_{r}} q^3  - 3\frac{\epsilon_{RF}}{2\epsilon_{RF}+\epsilon_{r}}q $$      | `epsrf`     | [doi](http://doi.org/dbs99w)
 `yukawa`        | $$ e^{-\kappa R_c q}-e^{-\kappa R_c}$$  | `debyelength`      | [ISBN](https://isbnsearch.org/isbn/0486652424)

Note: $$\mathcal{S}(q)$$ is _splined_ whereby all types evaluate equally fast.
{: .notice--info}

Additional information regarding electrostatics:

 - [On the dielectric constant](http://dx.doi.org/10.1080/00268978300102721)
 - [Generalized reaction field using ionic strength](http://dx.doi.org/10.1063/1.469273)

### Lennard-Jones

The Lennard-Jones potential has the form:

$$ u_{ij} = 4\epsilon_{ij} \left (
    \left ( \frac{\sigma_{ij}} {r_{ij})} \right )^{12} - \left ( \frac{\sigma_{ij}}{r_{ij})}\right )^6 \right ) $$

where the default mixing rule is Lorentz-Berthelot (LB):

$$\sigma_{ij} = \frac{\sigma_i+\sigma_j}{2}$$

and

$$\epsilon_{ij} = \sqrt{\epsilon_i \epsilon_j}$$

`lennardjones` |  Description
-------------  |  ------------------------------------------------
`mixing=LB`    |  Mixing rule, `LB`
`ljcustom`     |  Custom $$\epsilon$$ and $$\sigma$$ combinations

## External Pressure

## Bonded

