---
permalink: /energy/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# The Hamiltonian

The system energy is described by a Hamiltonian where an arbitrary number of energy terms can be added.

## Nonbonded Interactions

## Pair Potentials

### Coulomb Galore: `coulomb`

Beyond a spherical cutoff, $R_c$, the potential is cut and if
below, $ u(r) = \frac{\lambda_B z_i z_j }{ r }\mathcal{S}(q)$ with $q=r/R_c$
is returned with the following splitting functions, $\mathcal{S}$, that
will be splined during construction and thus evaluate at similar speeds,

 Type            | $\mathcal{S}(q=r/R_c)$               | Additional keywords  | Comment
 --------------- | ---------------------------------------- | -------------------- | ----------------------
 `plain`         | $ 1 $                                | none                 | http://doi.org/ctnnsj
 `none`          | $ 0 $                                | none                 | For convenience, only.
 `wolf`          | $ erfc(\alpha R_c q)-erfc(\alpha R_c)q $ | `alpha`          | http://doi.org/cfcxdk
 `fennel`        | $ erfc(\alpha R_c q)-erfc(\alpha R_c)q + ( q -1 ) q \left( erfc(\alpha R_c) + \frac{2\alpha R_c}{\sqrt{\pi}} e^{-\alpha^2 R_c^2} \right) $ | `alpha`| http://doi.org/bqgmv2
 `yonezawa`      | $ 1 + erfc(\alpha R_c)q + q^2 $      | `alpha`              | http://dx.doi.org/10/j97
 `fanourgakis`   | $ 1-\frac{7}{4}q+\frac{21}{4}q^5-7q^6+\frac{5}{2}q^7$| none | http://doi.org/f639q5
 `qpotential`     | $ \prod_{n=1}^{order}(1-q^n) $       | `order=300`          | ISBN [9789174224405](http://goo.gl/hynRTS) (Paper V)
 `reactionfield` | $ 1 + \frac{\epsilon_{RF}-\epsilon_{r}}{2\epsilon_{RF}+\epsilon_{r}} q^3  - 3\frac{\epsilon_{RF}}{2\epsilon_{RF}+\epsilon_{r}}q $      | `epsrf`     | http://doi.org/dbs99w
 `yukawa`        | $ e^{-\kappa R_c q}-e^{-\kappa R_c}$  | `debyelength`        | ISBN 0486652424

 The following keywords are required for all types:

 Keyword       |  Description
 ------------- |  -------------------------------------------
 `coulombtype` |  Type of splitting function as defined above
 `cutoff`      |  Spherical cutoff in angstroms
 `epsr`        |  Relative dielectric constant of the medium

 More info:

 - On the dielectric constant, http://dx.doi.org/10.1080/00268978300102721
 - Generalized reaction field using ionic strength, http://dx.doi.org/10.1063/1.469273

