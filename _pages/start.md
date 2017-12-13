---
permalink: /start/
sidebar:
    nav: "docs"
---
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Getting Started

## Installation

### Requirements

Faunus require `cmake` 3.6 or higher and a C++ compiler with good support for C++14.

### Compiling

Download the latest release and perform the following steps in a terminal:

~~~ bash
cd faunus
cmake .      # see build options below
make tests   # optional
make faunus
~~~

The build system automatically downloads a set of required dependencies.

<a name="input-output"></a>
## Input/Output 

Natively, Faunus input and output are in the [JSON format](http://json.org/example.html):

~~~ json
{
    "atomlist": [
        {
            "Na+": { "q": 1.0, "mw": 22.99 }
        }
    ]
}
~~~

However, via the small helper script `yason.py`, JSON can be converted to/from
[YAML](http://www.yaml.org) which is less verbose, more readable and therefore
used throughout this documentation:

~~~ yaml
atomlist:
    - Na+: { q: 1.0, mw: 22.99 }
~~~

### Post-Processing 

For further processing of input and output, JSON (and YAML) can be read by
most programming languages. For example:

~~~ python
import json
with open('out.json') as f:
    d = json.load(f) # --> dict
    print( d['atomlist'][0]["Na+"]["mw"] ) # --> 22.99
~~~

##<a name="running"></a> Running Faunus

By default, input is read from `stdin` and can either be JSON or,
via `yason.py`, YAML:

~~~ bash
faunus < input.json # from json
yason.py minimal.yml | faunus # from yaml
~~~

Output (json) can be conveniently converted to
yaml for better readability:

~~~ bash
yason.py < out.json > out.yml
~~~

