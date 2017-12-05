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

This terms loops over pairs of atoms, $$i$$, and $$j$$, summing a pair-wise additive potential, $$u_{ij}$$,

$$ U_{NB} = \sum_i\sum_j u_{ij}(\textbf{r}_{ij}) $$

The pair-potential can be a combination of several potentials, but this must currently be defined at compile
time and cannot be arbitrarily selected from input.
Below is a description of the individual pair-potentials and their input.

### Electrostatics
_Keyword:_ `coulomb`

This is a versatile pair potential that handles several variants of electrostatic
pair-potentials.
Beyond a spherical cutoff, $$R_c$$, the potential is zero while if
below,

$$ u_{ij} = \frac{q_i q_j }{ 4\pi\epsilon_0\epsilon_r r_{ij} }\mathcal{S}(q)$$

where $$q=r/R_c$$, and $$\mathcal{S}$$ is a splined splitting function
that is determined by the keyword `type`:

 `type`          | $$\mathcal{S}(q=r/R_c)$$               | Additional keywords  | Comment
 --------------- | -------------------------------------- | -------------------- | ----------------------
 `plain`         | $$ 1 $$                                | `none`               | [doi](http://doi.org/ctnnsj)
 `none`          | $$ 0 $$                                | `none`               | 
 `wolf`          | $$ erfc(\alpha R_c q)-erfc(\alpha R_c)q $$ | `alpha`          | [doi](http://doi.org/cfcxdk)
 `fennel`        | $$ \scriptstyle erfc(\alpha R_c q)-erfc(\alpha R_c)q + ( q -1 ) q \left( erfc(\alpha R_c) + \frac{2\alpha R_c}{\sqrt{\pi}} e^{-\alpha^2 R_c^2} \right) $$ | `alpha`| [doi](http://doi.org/bqgmv2)
 `yonezawa`      | $$ 1 + erfc(\alpha R_c)q + q^2 $$      | `alpha`              | [doi](http://dx.doi.org/10/j97)
 `fanourgakis`   | $$ 1-\frac{7}{4}q+\frac{21}{4}q^5-7q^6+\frac{5}{2}q^7$$| `none` | [doi](http://doi.org/f639q5)
 `qpotential`    | $$ \prod_{n=1}^{order}(1-q^n) $$       | `order=300`         | [ISBN](http://goo.gl/hynRTS) (Paper V)
 `reactionfield` | $$ 1 + \frac{\epsilon_{RF}-\epsilon_{r}}{2\epsilon_{RF}+\epsilon_{r}} q^3  - 3\frac{\epsilon_{RF}}{2\epsilon_{RF}+\epsilon_{r}}q $$      | `epsrf`     | [doi](http://doi.org/dbs99w)
 `yukawa`        | $$ e^{-\kappa R_c q}-e^{-\kappa R_c}$$  | `debyelength`        | [ISBN](https://isbnsearch.org/isbn/0486652424)

The following keywords are _required_ for all types,

 Keyword     | type     | Description
 ----------- | -------- | -------------------------------------------------
 `type`      | `str`    | Type of potential as defined above
 `cutoff`    | `float`  | Spherical cutoff after which the potential is zero
 `epsr`      | `float`  | Relative dielectric constant of the medium

 Additional information regarding electrostatics:

 - [On the dielectric constant](http://dx.doi.org/10.1080/00268978300102721)
 - [Generalized reaction field using ionic strength](http://dx.doi.org/10.1063/1.469273)

### Lennard-Jones
_Keyword:_ `lennardjones`

The Lennard-Jones potential has the form:

$$ u_{ij} = 4\epsilon_{ij} \left (
    \left ( \frac{\sigma_{ij}} {r_{ij})} \right )^{12} - \left ( \frac{\sigma_{ij}}{r_{ij})}\right )^6 \right ) $$

where the default mixing rule is Lorentz-Berthelot:

$$\sigma_{ij} = \frac{\sigma_i+\sigma_j}{2}$$

and

$$\epsilon_{ij} = \sqrt{\epsilon_i \epsilon_j}$$.

Keyword    | type   | Description
---------  | ------ | ------------------------------------------------
`mixing`   | `str`  | Mixing rule, `LB`
`ljcustom` | `list`  | Custom $$\epsilon$$ and $$\sigma$$ combinations

