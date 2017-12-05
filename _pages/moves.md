---
permalink: /moves/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Monte-Carlo Moves

A simulation can have an arbitrary number of MC moves operating on molecules, atoms, the whole system, or
any other parameter affecting the system energy. The list of moves are defined by the `moves` section
at the top level input:

~~~ yaml
moves:
    - moltransrot: { mol: water, dp: 2.0, dprot: 1.0, dir: [1,1,0] }
    - ...
~~~

## Molecular Translation and Rotation

This will simultaneously translate and rotate a molecule.

`moltransrot`    |  Description
---------------- |  ---------------------------------
`mol`            |  Molecule name to operate on
`dir=[1,1,1]`    |  Translational directions
`dp`             |  Translational displacement parameter
`dprot`          |  Rotational displacement parameter (radians)

