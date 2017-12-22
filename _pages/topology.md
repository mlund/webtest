---
permalink: /topology/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Topology

The topology describes atomic and molecular properties and is given at the top-level input:

~~~ yaml
atomlist:
    - Hw: {q:  0.4238}
    - Ow: {q: -0.8476, eps: 0.65, sigma: 3.165, mw: 16}
    - Na: {q:  1.0, sigma: 4, eps: 0.05, dp: 0.4}
    - Cl: {q: -1.0, sigma: 4, eps: 0.05, dp: 0.4}
moleculelist:
    - water: {Ninit: 256, structure: water.xyz}
    - salt:  {Ninit: 10, atoms: [Na,Cl], atomic: true}
~~~

## Atom Properties

`atomlist`    | Description
------------- | --------------------------------------------------------
`activity=0`  | Chemical activity for grand canonical MC [mol/l]
`alpha`       | Polarizability in units of [$$4\pi\epsilon_0$$]
`dp=0`        | Translational displacement parameter [Å]
`dprot=0`     | Rotational displacement parameter [degrees] (will be converted to radians)
`eps=0`       | Epsilon energy scaling commonly used for Lennard-Jones interactions etc. [kJ/mol]
`hydrophobic=false` | Is the particle hydrophobic? [`true`/`false`]
`mu=[0,0,0]`  | Dipole moment vector [Debye]
`Ninit=0`     | Initial number of atoms (used by `MoleculeData` to insert atoms
`theta`       | Quadrupole moment tensor [Debye $$\cdot$$ Å]
`mw=1`        | Molecular weight [g/mol]
`patchtype`   | Patchtype for sphero-cylinders
`q=0`         | Valency / partial charge number [$$e$$]
`r=0`         | Radius = `sigma/2` [Å]
`sigma=0`     | `2r` [Å] (overrides radius)
`tfe=0`       | Transfer free energy [J/mol/Å$$^2$$/M]
`alphax`      | Excess polarizability in units of [Å$$^3$$]

## Molecule Properties

`moleculelist`      | Description
------------------- | --------------------------------------------------------
`activity=0`        | Chemical activity for grand canonical MC [mol/l]
`atomic=false`      | True if collection of atomic species, salt etc.
`atoms=[]`          | Array of atom names - required if `atomic==true`
`implicit=false`    | If this species is implicit in GCMC schemes
`insdir=[1,1,1]`    | Insert directions are scaled by this
`insoffset=[0,0,0]` | Shifts mass center after insertion
`keeppos=false`     | Keep original positions of `structure`
`Ninactive=0`       | Deactivates `Ninactive` of the inserted molecules
`Ninit=0`           | How many molecules to insert
`structure`         | Structure file (`.pqr|.aam|.xyz`)
`bondlist`          | List of _internal_ bonds (harmonic, dihedrals etc.)


## Bonded Inteactions

Bonds and angular potentials are added via the keyword `bondlist` either directly
in a molecule definition (for internal bonds) or in the `energy`/`bonded` section:

~~~ yaml
moleculelist:
    - water:
        structure: water.xyz
        bondlist:
            - harmonic: { index=[0,1], k=100, req=1.0 }  
            - harmonic: { index=[0,2], k=100, req=1.0 }  
~~~

The following is a list of supported potential types:

`harmonic`      | Harmonic bond
--------------: | -------------------------------------------
`k`             | Harmonic spring constant (kJ/mol/Å$$^2$$)
`req`           | Equilibrium distance (Å)
`index`         | array with _exactly two_ index (relative to molecule)

Harmonic potential,

$$ u = \frac{1}{2}k(r_{12}-r_{eq})^2 $$

`fene`          | FENE potential
--------------: | -------------------------------------------
`k`             | Bond stiffness ($$k_BT$$)
`rmax`          | Maximum separation, $$r_m$$ (Å)
`index`         | array with _exactly two_ index (relative to molecule)

[Finite Extensible Nonlinear Elastic Potential](http://dx.doi.org/10.1103/PhysRevE.59.4248):

$$ u = -k_BT \frac{k r_m^2}{2}\ln \left [ 1-(r/r_m)^2 \right ] $$

for $$r < r_m$$; infinity otherwise.

## Processes

`processlist`   | Description
--------------- | ------------------------------------------------
`process`       | Process involving molecular groups (string)
`K`/`pK`        | Molar equilibrium constant or minus log thereof

The `process` string describes a transformation of molecular reactants (left of =)
into molecular products (right of =). A trailing tilde (~) denotes that
the species is to be treated _implicitly_.
In the example below we assume that `HA`, `H`, and `A` have
been defined in `moleculelist`:

~~~ yaml
processlist:
    - { process: "HA = H~ + A", pK=4.8 }
~~~
