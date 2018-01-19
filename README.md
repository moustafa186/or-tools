# OR-Tools - Google Optimization Tools

[![Build Status](https://travis-ci.org/google/or-tools.svg?branch=master)](https://travis-ci.org/google/or-tools)

Copyright 2018 Google Inc.

https://developers.google.com/optimization/

## Table of Contents

- [About OR-Tools](#about-or-tools)
- [Codemap](#codemap)
- [License](#license)
- [Installation](#installation)
- [Experimental Build with CMake](#experimental-build-with-cmake)
- [Quick Start](#quick-start)
- [Documentation](#documentation)
- [Contributing](#contributing)

## About OR-Tools

Google Optimization Tools (a.k.a., OR-Tools) is an open-source, fast and
portable software suite for solving combinatorial optimization problems.  
You can find [OR-Tools's documentation on the Google Developers site](https://developers.google.com/optimization/).

This repository contains the C++ library code.  
Using [SWIG](http://www.swig.org), we also provides wrapper in Python, C# and Java.

## Codemap

This software suite is composed of the following components:

	ortools/               <- Source code
	ortools/algorithms/    <- Basic algorithms
	ortools/base/          <- Basic utilities
	ortools/bop/           <- Boolean solver based on SAT
	ortools/com/           <- C# and Java source files
	ortools/constraint_solver/ <- Constraint solver
	ortools/flatzinc/      <- Flatzinc interpreter
	ortools/gen/           <- Generated files
	ortools/glop/          <- Linear solver
	ortools/graph/         <- Graph algorithms
	ortools/linear_solver/ <- Linear solver wrapper
	ortools/lp_data/       <- Data structures for linear model
	ortools/ortools        <- Python source code
	ortools/sat/           <- Sat solver
	ortools/util/          <- Utilities needed by the constraint solver
	Makefile               <- Top-level Makefile
	makefiles/             <- Subsidiary makefiles
	CMakeLists.txt         <- Top-level CMakeLists.txt
	cmake/                 <- Subsidiary CMake
	examples/              <- Root directory for all examples
	examples/com/          <- Java examples
	examples/cpp/          <- C++ examples
	examples/csharp/       <- C# examples
	examples/data/         <- Data files for examples
	examples/flatzinc/     <- Flatzinc examples
	examples/fsharp/       <- F# examples
	examples/notebook/     <- Jupyter/IPython notebooks
	examples/python/       <- Python examples
	examples/tests/        <- C# unit tests and bug reports
	LICENSE-2.0.txt        <- Apache license
	tools/                 <- Delivery Tools (e.g. Windows gnu binaries, scripts, dockers)

## License

The OR-Tools software suite is licensed under the terms of the Apache
license.  
See [LICENSE-2.0](LICENSE-2.0.txt) for more information.

## Installation

This software suite has been tested under:
- Ubuntu 16.04 up (64-bit).
- Mac OS X El Capitan with Xcode 7.x (64 bit).
- Microsoft Windows with Visual Studio 2015 and 2017 (64-bit)

OR-Tools is currently based on Makefile for building, but also provides Bazel and
CMake (Experimental) support.

For installation instructions (both source and binary), visit
https://developers.google.com/optimization/introduction/installing.

## Experimental Build with CMake

You can use the usual CMake way to build the OR-Tools software suite.
```sh
cmake -H. -Bbuild
cmake --build build
CTEST_OUTPUT_ON_FAILURE=1 cmake --build build --target test
```

Please check the [CMake build instructions](CMake.md) for more details.

## Quick Start

The best way to learn how to use OR-Tools is to follow the tutorials in our
developer guide:

https://developers.google.com/optimization/introduction/using

If you want to learn from code examples, take a look at the examples in the
[examples](examples) directory.

## Documentation

The complete documentation for OR-Tools is available via the
web at:

https://developers.google.com/optimization/

## Contributing

The [CONTRIBUTING.md](CONTRIBUTING.md) file contains instructions on how to
file the Contributor License Agreement before sending any pull requests (PRs).
Of course, if you're new to the project, it's usually best to discuss any
proposals and reach consensus before sending your first PR.
