---
permalink: /moves/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Monte Carlo Moves

A simulation can have an arbitrary number of MC moves operating on molecules, atoms, the volume, or
any other parameter affecting the system energy. The list of moves are defined by the `moves` section
at the top level input. For example:

~~~ yaml
moves:
    - moltransrot: { mol: water, dp: 2.0, repeat: N
                     dprot: 1.0, dir: [1,1,0] }
    - ...
~~~

## Molecular Translation and Rotation

`moltransrot`    |  Description
---------------- |  ---------------------------------
`mol`            |  Molecule name to operate on
`dir=[1,1,1]`    |  Translational directions
`dp`             |  Translational displacement parameter
`dprot`          |  Rotational displacement parameter (radians)
`repeat=N`       |  Number of repeats per MC sweep. `N` equals $$N_{molid}$$ times.

This will simultaneously translate and rotate a molecular group by the following operation

$$\textbf{r}^N_{trial} = \mbox{Rot}(\textbf{r}^N) + \delta$$

where $$\mbox{Rot}$$ rotates `dprot`$$\cdot \left (\zeta-\frac{1}{2} \right )$$ radians around a random unit vector
emanating from the mass center,
$$\zeta$$ is a random number in the interval $$[0,1[$$, and
$$\delta$$ is a random unit vector scaled by `dp`.
Upon MC movement, the mean squared displacement
will be tracked.


