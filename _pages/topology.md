---
permalink: /topology/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Topology

## Atom Properties

Atom _types_ are stored in the class `AtomData` which is used as template when adding particles to a system. Each atom type
is identified with an automatically assigned _id_ and properties are read from input via the following keys:

Key           | Description
:------------ | :-------------------------------------------------------
`activity`    | Chemical activity for grand canonical MC [mol/l]
`alpha`       | Polarizability in units of [$$4\pi\epsilon_0$$]
`dp`          | Translational displacement parameter [Å] 
`dprot`       | Rotational displacement parameter [degrees] (will be converted to radians)
`eps`         | Epsilon energy scaling commonly used for Lennard-Jones interactions etc. [kJ/mol] 
`hydrophobic` | Is the particle hydrophobic? [`true`/`false`]
`mu`          | Dipole moment vector [Debye]
`Ninit`       | Initial number of atoms (used by `MoleculeData` to insert atoms
`theta`       | Quadrupole moment tensor [Debye $$\cdot$$ Å]
`mw`          | Molecular weight [g/mol]
`patchtype`   | Patchtype for sphero-cylinders
`q`           | Valency / partial charge number [$$e$$]
`r`           | Radius = `sigma/2` [Å]
`sigma`       | `2r` [Å] (overrides radius)
`tfe`         | Transfer free energy [J/mol/Å$$^2$$/M] (default: 0.0)
`alphax`      | Excess polarizability in units of [Å$$^3$$]

## Molecule Properties
