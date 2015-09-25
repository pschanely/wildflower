# Wildflower
Wildflower is a minimalistic, pure functional, JSON-formatted programming language.

## Status
 * Wildflower is just an idea at the moment.
 * No specification exists and no platform implementations exist.
 * There is, however, a [JSON schema](module.schema.json) available for Wildflower source files and the documentation in there provides a few hints about where I'm going.
 * I am working on a touch-based interface for editing Wildflower code here: https://github.com/pschanely/wildflower-touch

## The Wildflower programming language ...
 * is minimalistic.
 * is purely functional. (side-effect free)
 * is dynamically typed.
 * has strict (aka eager or applicative order) evaluation.<sup>1</sup>
 * is parsed and stored in an extensible JSON format.
 * has functions that consume and return a fixed number of values.
 * strives to make no further assumptions, leaving many potential features up to language extensions and metaprograms.

## Why
 * Wildflower is designed to facilitate program analysis, transformation, and optimization.
 * Wildflower is designed to compile to a wide variety of platforms: from hardware description languages like VHDL up through clustered platforms like hadoop.  It can accomplish this by specifying very little in the core language (Wildflower has no primitive datatypes!) and allowing platforms to implement only the appropriate interfaces.

## Details
 * Wildflower is represented as a stack-based language, however, it supports variables and values in closures.  Unlike other stack based languages, Wildflower code can also be analyzed as a directed graph of function calls, because the number of arguments and return values at each invocation point is known satically.
 * Wildflower does not have modules, packages, or namespaces: there is only one kind of Wildflower file, which simply contains a list of functions.  Functions are always referenced by the permalink to their definition.
 * Wildflower code is, itself, immutable: all Wildflower files must be referenced by permalink, and may never change.
 * Wildflower supports first order functions and lambda expressions, which may refer to special named values in their closure.

## Example 
This isn't a terribly useful example (largely because functions are referred to through uuids), but it is my current implementation of the reduce function:
```
{
  "src_version":"0.1",
  "name":"An implementation of reduce",
  "refs":[
    {"url":"http://millstonecw.com:11739/module/12db0c6b9d2a0776a3f1599b0fb40fff"}
  ],
  "functions": {
    "4f06d7838207044cc46e7c72db06e2d0": {
      "name":"reduce",
      "overrides":"0:febfb8588be5ed1df6f2893743921a7c",
      "tags":[],
      "numConsumed":2,
      "numProduced":1,
      "condition":[
        {"op":"literal","val":true}
      ],
      "code":[
        {"op":"cond","branches":[
          {
            "condition":[
              {"op":"0:5bdf7b1b60e0021e378be04f0b39a634"},
              {"op":"0:a83dff7e6a6c88b285c3f1657c324be5"},
              {"op":"literal","val":1},
              {"op":"0:733ba5ff2d457f51e7428d62cbbbd33f"}
            ],
            "code":[
              {"op":"0:5bdf7b1b60e0021e378be04f0b39a634"},
              {"op":"0:bfe4f5231bd430895886baee7e9ecf51"},
              {"op":"0:a56e56fdbc398e6cee309fce400bb2b1"},
              {"op":"0:5bdf7b1b60e0021e378be04f0b39a634"}
            ]
          },
          {
            "condition":[
              {"op":"literal","val":true}
            ],
            "code":[
              {"op":"save","name":"f"},
              {"op":"0:bfe4f5231bd430895886baee7e9ecf51"},
              {"op":"save","name":"x"},
              {"op":"0:bfe4f5231bd430895886baee7e9ecf51"},
              {"op":"save","name":"y"},
              {"op":"load","name":"x"},
              {"op":"load","name":"y"},
              {"op":"load","name":"f"},
              {"op":"callLambda", "numConsumed":2, "numProduced":1},
              {"op":"0:cd5c7dd3fb29443bb873c62f34f095a6"},
              {"op":"load","name":"f"},
              {"op":"0:febfb8588be5ed1df6f2893743921a7c"}
            ]
          }
        ]}
      ]
    }
  }
}
```


<sup>1</sup>  Technically, the evaluation order may be determined by the target platform, though all Wildflower platforms guarantee applicative order termination characteristics.

