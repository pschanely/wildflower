# Wildflower
Wildflower is a minimalistic, pure functional, JSON-formatted programming language.

## Status
 * Wildflower is just an idea at the moment.
 * The specification is largely yet-to-be developed and no platform implementations exist.
 * There is, however, a [JSON schema](module.schema.json) available for Wildflower source files.

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
```
{
    "refs": [
    	{"url": "base.wf.json", "size": 1942, "hash": "95ae9e835bb041ac9c8cce52fb58d9d4"}
    ],
    "src_version": "0.1",
    "functions": {
	"number_combine" : {
	    "comment": "Interface for a function that takes two numbers and does stuff with them",
	    "condition": [
                {"op": "literal", "val": false}
	    ],
	    "code": []
	}
    }
}
{
    "refs": [
    	{"url": "number_interface.wf.json", "size": 1942, "hash": "95ae9e835bb041ac9c8cce52fb58d9d4"},
    	{"url": "base.wf.json", "size": 1942, "hash": "95ae9e835bb041ac9c8cce52fb58d9d4"}
    ],
   "refs": [{"url": "
	"add_and_sum": {
	    "comment": "",
	    "condition": [
	        {"op": "literal", "val": true}
	    ],
	    "code": [
	    ]
	}
		{"op": "literal", "val": 2},
		{"op": "literal", "val": 2},
		{"op": "save", "name": "myvar"},
		{"op": "load", "name": "myvar"},
		{"op": "cond", "branches": [
		    {
			"condition": [
			    {"op": "literal", "val": 0},
			    {"op": "call", "ref": "5f69ac55bdb34f24ab178c301b5b423f", "at": 0}
			 ],
			"code": [
			    {"op": "call", "ref": "b486a7b7704a42a682889cb0a8027f53", "at": 0},
			    {"op": "call", "ref": "b486a7b7704a42a682889cb0a8027f53", "at": 0},
			    {"op": "literal", "val": -1}
			]
		    },
		    {
			"code": [
			    {"op": "call", "ref": "cf6085288ff54d4e8812ff4632b70128", "at": 0},
			    {"op": "call", "ref": "3b7a481f2a9c429a828eee43d1ec7c3b", "at": 0},
			    {"op": "call", "ref": "d10b90f9541e4844b996750799f66eef", "at": 0}
			]
		    }
		]}
	    ]
	}
    }
}

{
  "refs": [
    {"url": "http://.../dataflow.wf.json", "size": 1942, "hash": "95ae9e835bb041ac9c8cce52fb58d9d4"},
    {"url": "http://.../basic_arithemetic.wf.json", "size": 23452, "hash": "37e80391de41442c855512b9ce6aa0ee"}
  ],

  "functions": {

    "add_and_square": {
      "comment": "An interface for a function that sums two numbers and squares the result",
      "inputs": [{}, {}],
      "outputs": [{}],
      "invariants": [
        {
	  "rewrite": [
	    {"op": "call", "#/refs/0/swap"},
            {"op": "call", "ref": "#/functions/add_and_square"}
	  ],
	  "to": [
	    {"op": "call", "ref": "#/functions/add_and_square"}
          ]
        }
      ],
      "domain": [
        {"id": "B", "op": "call", "ref": "#/refs/1/isint"},
	{"id": "D", "op": "callat", "at": 1, "ref": "#/refs/1/isint"},
      ]
    },

    "add_and_square_implementation": {
      "implements": "#/functions/add_and_square",
      "code": [
        {"id": "A", "op": "call", "ref": "#/refs/1/add"},
        {"id": "B", "op": "call", "ref": "#/refs/0/duplicate"},
        {"id": "C", "op": "call", "ref": "#/refs/1/multiply"}
      ]
    }

  }
}
```

## Stack-graph hybrid example w/ lambda
```
  "code": [
    {"id":"A", "op":"lambda", "code":[{"literal":1}, {"call":"#/refs/1/add"}]},
    {"id":"B", "op":"literal", "val":[2,4,6]},
    {"id":"C", "op":"lambda", "code":[{"literal":2}, {"pull":1}, {"call":"#/refs/1/divide"}], "comment":"divide by 2"},
    {"id":"D", "op":"call", "ref": "#/refs/0/map"},
    {"id":"E", "op":"eval", inputs:2, outputs:1},
    {"id":"F", "op":"push", "code": [
    ]}
    {"id":"I", "op":"cond", "branches": [
      [{"call":..}, {"op":"predicate", "kind":"conditional"}, ..],
      [..],
    ]},
  ]
```


<sup>1</sup>  Technically, the evaluation order may be determined by the target platform, though all Wildflower platforms guarantee applicative order termination characteristics.

