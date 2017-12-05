---
permalink: /topology/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Topology

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

