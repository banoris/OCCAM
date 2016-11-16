[![PyPI version](https://badge.fury.io/py/razor.svg)](https://badge.fury.io/py/razor)
[![Build Status](https://travis-ci.org/SRI-CSL/OCCAM.svg?branch=master)](https://travis-ci.org/SRI-CSL/OCCAM)


Prerequisites
============

OCCAM currently works fine on Linux, OS X, and FreeBSD. You will
need an installation of llvm-3.5. If you need to generate application bitcode,
you will want to install the pip package [wllvm](https://github.com/SRI-CSL/whole-program-llvm.git).


Building and Installing
=======================

Set where OCCAM's library will be stored:
```
  export OCCAM_HOME={path to location in your home directory}
```

Point to your LLVM's location, if non-standard:
```
  export LLVM_HOME=/usr/local/llvm-3.5
  export LLVM_CONFIG=llvm-config-3.5
```

Set where system libraries, including Google Protocol Buffers, are located:
```
  export LD_FLAGS='-L/usr/local/lib'
```

Build and install OCCAM with:

```
  make
  make install
```

Using OCCAM
===========

You can choose to record logs from the OCCAM
tool by setting the following variables:

```
  export OCCAM_LOGFILE={absolute path to log location}
  export OCCAM_LOGLEVEL={INFO, WARNING, or ERROR}
```


Using razor
===========

`razor` is a pip package that relies on the same dynamic library as `occam`,
so you should first build and install `occam` as described above.
You can install it from this repository, or you can just do a
```
pip install razor
```
To install and editable version from this repository:

```
make -f Makefile develop
```

This may require sudo priviliges. Either way you can now use slash:

```
slash [--work-dir=<dir>]  [--force] [--no-strip] [--no-specialize] <manifest>
```

`slash` also accepts the following new command line option:
```
--no-specialize
```

which will prevent any inter-module specializations.

The Manifest(o)
===============

The manifest for `slash` should be valid JSON. The following keys 
have meaning:

+ `main` : a path to the bitcode module containing the `main` entry point.

+ `modules`: a list of paths to the other bitcode modules needed.

+ `binary` : the name of the desired executable.

+ `native_libs' : a list of flags ('-lm', '-lc', '-lpthread') or paths to native objects (`.o', '.a', '.so', '.dylib')

+ `ldflags`: a list of linker flags such as '--static', '--nostdlib'

+ args : the arguments you wish to specialize the main of `main` to.

