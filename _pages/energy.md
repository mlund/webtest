---
permalink: /energy/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Energy <a name="energy"></a>

The system energy is described by a Hamiltonian where an arbitrary number of potential energy terms can be added,

$$\mathcal{H}_{sys} = U_1 + U_2 + ... $$

The energy terms are given as a list to `energy` in the top level input:

~~~ yaml
energy:
    - isobaric: {P/atm: 1}
    - nonbonded_coulomblj:
        lennardjones: {mixing: LB}
        coulomb: {type: plain, epsr: 80, cutoff: 10}
    - ...
~~~

## External Pressure <a name="isobaric"></a>

`isobaric`   | Description
-------------|-------------------------------------------------------
`P/unit`     | External pressure where unit can be `mM`, `atm`, or `Pa`.

This adds the following pressure term[^frenkel] to the Hamiltonian, appropriate for
MC moves in $$\ln V$$:

$$ U = PV - k_BT\left ( N + 1 \right ) \ln V $$

where $$N$$ is the total number of molecules and atomic species.

[^frenkel]: _Frenkel and Smith, 2nd Ed., Chapter 5.4_.

## Nonbonded Interactions <a name="nonbonded"></a>

`energy`               | $$u_{ij}$$
-----------------------|---------------------------
`nonbonded_coulomblj`  | `coulomb`+`lennardjones`
`nonbonded_coulombhs`  | `coulomb`+`hardsphere`

This term loops over pairs of atoms, $$i$$, and $$j$$, summing a given pair-wise additive potential, $$u_{ij}$$,

$$ U_{NB} = \sum_i\sum_j u_{ij}(\textbf{r}_j-\textbf{r}_i)$$

**Note:** the pair-potential can be a combination of several potentials, but this must currently be defined at _compile
time_ and cannot be arbitrarily selected from input. It is straight forward to add more by editing the class
`Energy::Hamiltonian` found in `src/energy.h` and then re-compile.
{: .notice--info}

Below is a description of possible pair-potentials and their configuration.

### Coulomb

 `coulomb`   |  Description
 ----------- |  -------------------------------------------------
 `type`      |  Coulomb type, see below
 `cutoff`    |  Spherical cutoff, $$R_c$$ after which the potential is zero
 `epsr`      |  Relative dielectric constant of the medium

This is a multipurpose pair potential that handles several variants of electrostatic
pair-potentials. Beyond a spherical cutoff, $$R_c$$, the potential is zero while if
below,

$$ u_{ij} = \frac{e^2 z_i z_j }{ 4\pi\epsilon_0\epsilon_r r_{ij} }\mathcal{S}(q)$$

where $$\mathcal{S}(q=r/R_c)$$ is a splitting function:

 `type`          | $$\mathcal{S}(q)$$                     | Additional keywords | Ref.
 --------------- | -------------------------------------- | ------------------- | ----------------------
 `plain`         | $$ 1 $$                                |                     | [doi](http://doi.org/ctnnsj)
 `none`          | $$ 0 $$                                |                     | 
 `wolf`          | $$ \text{erfc}(\alpha R_c q)-\text{erfc}(\alpha R_c)q $$ | `alpha`         | [doi](http://doi.org/cfcxdk)
 `fennel`        | $$ \scriptstyle \text{erfc}(\alpha R_c q)-\text{erfc}(\alpha R_c)q + ( q -1 ) q \left( \text{erfc}(\alpha R_c) + \frac{2\alpha R_c}{\sqrt{\pi}} e^{-\alpha^2 R_c^2} \right) $$ | `alpha`| [doi](http://doi.org/bqgmv2)
 `yonezawa`      | $$ 1 + \text{erfc}(\alpha R_c)q + q^2 $$      | `alpha`             | [doi](http://dx.doi.org/10/j97)
 `fanourgakis`   | $$ 1-\frac{7}{4}q+\frac{21}{4}q^5-7q^6+\frac{5}{2}q^7$$|     | [doi](http://doi.org/f639q5)
 `qpotential`    | $$ \prod_{n=1}^{order}(1-q^n) $$       | `order=300`         | [ISBN](http://goo.gl/hynRTS) (Paper V)
 `reactionfield` | $$ 1 + \frac{\epsilon_{RF}-\epsilon_{r}}{2\epsilon_{RF}+\epsilon_{r}} q^3  - 3\frac{\epsilon_{RF}}{2\epsilon_{RF}+\epsilon_{r}}q $$      | `epsrf`     | [doi](http://doi.org/dbs99w)
 `yukawa`        | $$ e^{-\kappa R_c q}-e^{-\kappa R_c}$$  | `debyelength`      | [ISBN](https://isbnsearch.org/isbn/0486652424)

**Note:** Internally $$\mathcal{S}(q)$$ is _splined_ whereby all types evaluate at similar speed.
{: .notice--info}

Additional information regarding electrostatics:

 - [On the dielectric constant](http://dx.doi.org/10.1080/00268978300102721)
 - [Generalized reaction field using ionic strength](http://dx.doi.org/10.1063/1.469273)


### Hard Sphere
`hardsphere`

The hard sphere potential does not take any input. Radii are read from the atomlist at the beginning of the simulation.


### Lennard-Jones

`lennardjones` |  Description
-------------  |  ------------------------------------------------
`mixing=LB`    |  Mixing rule. `LB`
`ljcustom`     |  Custom $$\epsilon$$ and $$\sigma$$ combinations

The Lennard-Jones potential has the form:

$$ u_{ij} = 4\epsilon_{ij} \left (
    \left ( \frac{\sigma_{ij}} {r_{ij})} \right )^{12} - \left ( \frac{\sigma_{ij}}{r_{ij})}\right )^6 \right ) $$

where the default mixing rule is Lorentz-Berthelot (`LB`):

$$\sigma_{ij} = \frac{\sigma_i+\sigma_j}{2} \quad \textrm{and} \quad
\epsilon_{ij} = \sqrt{\epsilon_i \epsilon_j}$$

 
### Weeks-Chandler-Andersen

`wca`          |  Description
-------------  |  ------------------------------------------------
`mixing=LB`    |  Mixing rule. `LB`
`ljcustom`     |  Custom $$\epsilon$$ and $$\sigma$$ combinations

Ala Lennard-Jones where the potential is cut and shifted to zero at
$$r_c=2^{1/6}\sigma$$.
More info [here](http://dx.doi.org/ct4kh9).

$$ u(r) = 4 \epsilon_{ij} \left ( (\sigma_{ij}/r)^{12} - (\sigma_{ij}/r)^6 + \frac{1}{4} \right ) $$


## Bonded Interactions

Bonds and angular potentials are added via the keyword `bondlist` either directly
in a molecule definition (topology) or in energy/bonded where the latter can be
used to add inter-molecular bonds:

~~~ yaml
moleculelist:
    - water:
        structure: water.xyz
        bondlist: # index relative to molecule
            - harmonic: { index=[0,1], k=100, req=1.0 }  
            - harmonic: { index=[0,2], k=100, req=1.0 }
energy:
    - bonded:
        bondlist: # absolute index
           - harmonic: { index=[56,921], k=10, req=15 }
~~~

Bonded potential types:

### Harmonic

`harmonic`     | Harmonic bond
-------------- | -------------------------------------------
`k`            | Harmonic spring constant (kJ/mol/Å$$^2$$)
`req`          | Equilibrium distance (Å)
`index`        | array with _exactly two_ index (relative to molecule)

$$ u(r) = \frac{1}{2}k(r-r_{eq})^2 $$

### Finite Extensible Nonlinear Elastic

`fene`         | [Finite Extensible Nonlinear Elastic Potential](http://dx.doi.org/10.1103/PhysRevE.59.4248)
-------------- | -------------------------------------------
`k`            | Bond stiffness (kJ/mol/Å$$^2$$)
`rmax`         | Maximum separation, $$r_m$$ (Å)
`index`        | array with _exactly two_ index (relative to molecule)

$$ u(r) = -\frac{1}{2} k r_m^2 \ln \left [ 1-(r/r_m)^2 \right ] $$

for $$r < r_m$$; infinity otherwise.

