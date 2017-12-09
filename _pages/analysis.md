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

# XTC trajectory

`xtcfile`      |  Description
-------------- | ---------------------------------------------------------
`nstep=0`      |  Interval between samples. If -1, save at end of simulation
`file`         |  Filename of output xtc file

Save configurations to a Gromacs XTC trajectory file. 
