---
permalink: /energy/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# The Hamiltonian

The system energy is described by a Hamiltonian where an arbitrary number of potential energy terms can be added,

$$U_{sys} = U_1 + U_2 + ... $$

## Nonbonded Interactions

This energy term adds a pair-wise additive potential to the Hamiltonian, looping over pairs of particles, $$i$$, and $$j$$:

$$ U_{nb} = \sum_i\sum_j u_{ij}(\textbf{r}_{ij}) $$

### Electrostatics
`coulomb`

Beyond a spherical cutoff, $$R_c$$, the potential is zero while if
below,

$$ u_{ij} = \frac{\lambda_B z_i z_j }{ r }\mathcal{S}(q)$$

with $$q=r/R_c$$ is returned with the following splitting functions, $$\mathcal{S}$$, that
will be splined during construction and thus evaluate at similar speeds:

 Type            | $$\mathcal{S}(q=r/R_c)$$               | Additional keywords  | Comment
 --------------- | -------------------------------------- | -------------------- | ----------------------
 `plain`         | $$ 1 $$                                | `none`               | [doi](http://doi.org/ctnnsj)
 `none`          | $$ 0 $$                                | `none`               | 
 `wolf`          | $$ erfc(\alpha R_c q)-erfc(\alpha R_c)q $$ | `alpha`          | [doi](http://doi.org/cfcxdk)
 `fennel`        | $$ \scriptstyle erfc(\alpha R_c q)-erfc(\alpha R_c)q + ( q -1 ) q \left( erfc(\alpha R_c) + \frac{2\alpha R_c}{\sqrt{\pi}} e^{-\alpha^2 R_c^2} \right) $$ | `alpha`| [doi](http://doi.org/bqgmv2)
 `yonezawa`      | $$ 1 + erfc(\alpha R_c)q + q^2 $$      | `alpha`              | [doi](http://dx.doi.org/10/j97)
 `fanourgakis`   | $$ 1-\frac{7}{4}q+\frac{21}{4}q^5-7q^6+\frac{5}{2}q^7$$| none | [doi](http://doi.org/f639q5)
 `qpotential`     | $$ \prod_{n=1}^{order}(1-q^n) $$       | `order=300`         | [ISBN](http://goo.gl/hynRTS) (Paper V)
 `reactionfield` | $$ 1 + \frac{\epsilon_{RF}-\epsilon_{r}}{2\epsilon_{RF}+\epsilon_{r}} q^3  - 3\frac{\epsilon_{RF}}{2\epsilon_{RF}+\epsilon_{r}}q $$      | `epsrf`     | [doi](http://doi.org/dbs99w)
 `yukawa`        | $$ e^{-\kappa R_c q}-e^{-\kappa R_c}$$  | `debyelength`        | [ISBN](https://isbnsearch.org/isbn/0486652424)

 The following keywords are required for all types:

 Keyword     | type     | Description
 ----------- | -------- | -------------------------------------------
 `type`      | `str`    | Type of splitting function as defined above
 `cutoff`    | `float`  | Spherical cutoff
 `epsr`      | `float`  | Relative dielectric constant of the medium

 More info:

 - [On the dielectric constant](http://dx.doi.org/10.1080/00268978300102721)
 - [Generalized reaction field using ionic strength](http://dx.doi.org/10.1063/1.469273)

### Lennard-Jones
`lennardjones`

The Lennard-Jones potential has the form:

$$ u_{ij} = 4\epsilon_{ij} \left (
    \left ( \frac{\sigma_{ij}} {r_{ij})} \right )^{12} - \left ( \frac{\sigma_{ij}}{r_{ij})}\right )^6 \right ) $$

where the default mixing rule is Lorentz-Berthelot:

$$\sigma_{ij} = (\sigma_i+\sigma_j)/2$$

$$\epsilon_{lj}=\epsilon$$.

