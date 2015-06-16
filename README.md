# The "Wildflower" programming language

## Status
 * Wildflower is just an idea at the moment.
 * The specification is largely yet-to-be developed and no platform implementations exist.

## The Wildflower programming language ...
 * is minimalistic.
 * is purely functional. (side-effect free)
 * is dynamically typed. (or perhaps, more accurately, has no types)
 * has strict (aka eager or applicative order) evaluation.<sup>1</sup>
 * is parsed and stored in an extensible JSON format.
 * has functions that consume and return a fixed number of arguments.
 * has an minimalistic system for interfaces. (simply groups of functions that are left undefined)
 * strives to make no further assumptions, leaving many potential features up to language extensions and metaprograms.

## Why
 * Wildflower is designed to facilitate program analysis, transformation, and optimization.
 * Wildflower is designed to compile to a wide variety of platforms: from hardware description languages like VHDL up through clustered platforms like hadoop.  It can accomplish this by specifying very little in the core language (Wildflower has no primitive datatypes!) and allowing platforms to implement only the appropriate interfaces.

## Details
 * Wildflower code is represented as a directed, acyclic graph with port labels to distinguish input and output values from each other.
 * Wildflower does not have modules, packages, or namespaces: there is only one kind of Wildflower file, which simply contains a list of functions.  Functions are always referenced by the permalink to their definition.
 * Wildflower code is, itself, immutable: all Wildflower files must be referenced by permalink, and may never change.


## Example 
```
{
  "refs": [
    {"url": "http://.../dataflow.by", "size": 1942, "hash": "95ae9e835bb041ac9c8cce52fb58d9d4"},
    {"url": "http://.../basic_arithemetic.by", "size": 23452, "hash": "37e80391de41442c855512b9ce6aa0ee"}
  ],

  "functions": {

    "add3_interface_fn": {
      "comment": "An interface for a function that sums two numbers and squares the result",
      "inputs": [{}, {}],
      "outputs": [{}]
    },

    "add3_implementation": {
      "implements": "#/functions/add3_interface_fn",
      "code": {
        "nodes": {
          "A": {"in": 0},
          "B": {"in": 1},
          "C": {"call": "#/refs/1/add"}},
          "D": {"call": "#/refs/0/duplicate"}},
          "E": {"call": "#/refs/1/multiply"}},
          "F": {"out": 0}
        },
        "edges": {
          "A": [{"to": "C", "port": 0}],
          "B": [{"to": "C", "port": 1}],
          "C": [{"to": "D"}],
          "D": [{"to": "E", "port": 0}, {"to": "E", "port": 1}],
          "E": [{"to": "F"}]
        ]
      }
    }

  }
}
```


<sup>1</sup>  Technically, the evaluation order may be determined by the target platform, though all Wildflower platforms must guarantee applicative order termination characteristics.

