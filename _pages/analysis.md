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
    - systemenergy: {file: energy.dat, nstep: 500}
    - xtcfile: {file: traj.xtc, nstep: 1000}
    - widom:  {molecule: water, ninsert: 20, nstep: 50}
    - molrdf: {name1: water, name2: water, nstep: 100,
               dr: 0.1, dim: 3, file: rdf.dat}
    - ...
~~~

**Note:** all analysis methods support the `nstep` keyword that defines the interval between
sampling points. In addition all analysis provide output statistics of number of sample
points, and the relative run-time spent on the analysis.
{: .notice--info}

## Density

`density     `   |  Description
---------------- |  -------------------------------------------
`nstep=0`        |  Interval between samples

This will calculate the average density, $$\langle N_i/V \rangle$$ of molecules and atoms
which fluctuate in _e.g._ the isobaric ensemble or the Grand Canonical ensemble.
For atomic groups, densities of individual atom types are reported.

## Radial Distribution Function

### Atomic

`atomrdf`      |  Description
-------------- | ---------------------------------------------------------
`nstep=0`      |  Interval between samples
`file`         |  Output file, two column
`name1`        |  Atom name 1
`name2`        |  Atom name 2
`dr=0.1`       |  $$g(r)$$ resolution
`dim=3`        |  Dimensions for volume element

We sample the pair correlation function between atom id's _i_ and _j_,

$$
g_{ij}(r) = \frac{ N_{ij}(r) }{ \sum_{r=0}^{\infty} N_{ij}(r) }
\cdot \frac{ \langle V \rangle }{ V(r) }
$$

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
`file`         |  Output file, two column
`name1`        |  Molecule name 1
`name2`        |  Molecule name 2
`dr=0.1`       |  $$g(r)$$ resolution
`dim=3`        |  Dimensions for volume element
`nstep=0`      |  Interval between samples.

Same as `atomrdf` but for molecular mass-centers.

## Save State

`savestate`    |  Description
-------------- | ---------------------------------------------------------
`file`         |  File to save; format detected by file extension: `pqr`, `aam`, `state`
`nstep=-1`     |  Interval between samples. If -1, save at end of simulation

Saves the current configuration and potentially the system state to a file.
If a `.state` file is specified, the complete system state is saved to a single
json file that can be used to restore the state.

## System Energy

`systemenergy`   |  Description
---------------- |  -------------------------------------------
`file`           |  Output filename for energy vs. step output
`nstep=0`        |  Interval between samples

Calculates the energy contributions from all terms in the Hamiltonian and
outputs to a file.
If filename ends with `.csv`, a comma separated value file will be saved,
otherwise a simple space separeted file with a single hash commented header line.
All units in $$k_BT$$.

## Virtual Volume Move

`virtualvolume` | Description
--------------- | -------------------------------------
`dV`            | Volume perturbation (angstrom cubed)
`nstep`         | Interval between samples

Performs a [virtual volume move](http://dx.doi.org/10.1063/1.472721) by
scaling the simulation volume to $$V+\Delta V$$ along with
molecular mass centers and atomic positions. The excess pressure is evatuated
as a Widom average:

$$
p^{ex} = -\frac{k_BT}{\Delta V} \ln
    \langle e^{-\delta u / k_BT} \rangle_{NVT}
$$

For more advanced applications of volume perturbations (pressure tensors, surface tension etc.)
see [here](http://dx.doi.org/10.1063/1.4767375).

## Widom Insertion

`widom`       | Description
------------- | -----------------------------------------
`molecule`    | Name of _inactive_ molecule to insert (atomic or molecular)
`ninsert`     | Number of insertions per sample event
`dir=[1,1,1]` | Inserting directions
`absz=false`  | Apply `std::fabs` on all z-coordinates of inserted molecule
`nstep=0`      |  Interval between samples

This will insert a non-perturbing ghost molecule into
the system and calculate a [Widom average](https://doi.org/10.1063/1.1734110)
to measure the free energy of the insertion process, _i.e._ the
excess chemical potential:

$$
\mu^{ex} = -k_BT \ln \langle e^{-\delta u/k_BT} \rangle_0
$$

where $$\delta u$$ is the energy change of the perturbation and the
average runs over the _unperturbed_ ensemble.

**Note:**
At least one _inactive_ `molecule` must be added to the simulation using the `Ninactive`
keyword when defining molecule types in the topology.
{: .notice--info}

Upon insertion, random positions and orientations are
generated. For use with rod-like particles on surfaces, the `absz`
keyword may be used to ensure orientations on only one
half-sphere.


## XTC trajectory

`xtcfile`      |  Description
-------------- | ---------------------------------------------------------
`file`         |  Filename of output xtc file
`nstep=0`      |  Interval between samples.

Save configurations to a Gromacs XTC trajectory file.
