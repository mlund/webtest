---
permalink: /analysis/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Analysis

Faunus can perform on-the-fly analysis during simulation by allowing for an arbitrary number
of analysis functions to be added. The list of analysis is defined in the `analysis` section
at the top level input:

~~~ yaml
analysis:
    - systemenergy: { file: energy.dat, nstep: 500 }
    - xtcfile: { file: traj.xtc, nstep: 1000 }
    - molrdf: { name1: water, name2: water,
                nstep: 100, dr: 0.1, dim: 3, file: rdf.dat }
    - ...
~~~

**All** analysis methods support the `nstep` keyword that defines the interval between
sampling points. In addition all analysis provide output statistics of number of sample
points and the relative time of the total run-time that was spent on the analysis.

## System Energy

`systemenergy`   |  Description
---------------- |  -------------------------------------------
`nstep=0`        |  Interval between samples
`file`           |  Output filename for energy vs. step output

Calculates the system energy; collect the average; and outputs to file.

## Save State

`savestate`    |  Description
-------------- | ---------------------------------------------------------
`nstep=-1`     |  Interval between samples. If -1, save at end of simulation
`file`         |  File to save; format detected by file extension: `pqr`, `aam`, `state`

Saves the current configuration and potentially the system state to a file.
If a `.state` file is specified, the complete system state is saved to a single
json file that can be used to restore the state.

## XTC trajectory

`xtcfile`      |  Description
-------------- | ---------------------------------------------------------
`nstep=0`      |  Interval between samples.
`file`         |  Filename of output xtc file

Save configurations to a Gromacs XTC trajectory file. 

## Radial Distribution Function

### Atomic

`atomrdf`      |  Description
-------------- | ---------------------------------------------------------
`nstep=0`      |  Interval between samples.
`file`         |  Output file, two column
`name1`        |  Atom name 1
`name2`        |  Atom name 2
`dr=0.1`       |  $$g(r)$$ resolution
`dim=3`        |  Dimensions for volume element

We sample the pair correlation function between atom id's _i_ and _j_,

$$ g_{ij}(r) = \frac{ N_{ij}(r) }{ \sum_{r=0}^{\infty} N_{ij}(r) }
\cdot \frac{ \langle V \rangle }{ V(r) } $$

where $$N_{ij}(r)$$ is the number of observed pairs, accumulated over the
entire ensemble, in the separation
interval $$[r, r+dr]$$ and $$V(r)$$ is the corresponding volume element
which depends on dimensionality:

$$ V(r) $$                 | Dimensions (`dim`)
:------------------------- | :----------------------------------------
$$ 4\pi r^2 dr $$          | 3 (for particles in free space, default)
$$ 2\pi r dr $$            | 2 (for particles confined on a plane)
$$ 2\pi R sin(r/R) dr $$   | 2 (for particles confined on a 2D hypersphere surface, also needs input `Rhypersphere`)
$$ dr $$                   | 1 (for particles confined on a line)

### Molecular

`molrdf`       |  Description
-------------- | ---------------------------------------------------------
`nstep=0`      |  Interval between samples.
`file`         |  Output file, two column
`name1`        |  Molecule name 1
`name2`        |  Molecule name 2
`dr=0.1`       |  $$g(r)$$ resolution
`dim=3`        |  Dimensions for volume element

Same as `atomrdf` but for molecular mass-centers.

