# idris-chez

A [ChezScheme](https://cisco.github.io/ChezScheme/) backend for [Idris](http://idris-lang.org)

## Usage

A Chez Scheme installation is a prerequisite to use idris-chez. It uses Chez Scheme extensions and would need to be modified to work with other Scheme flavors.

Run `cabal install` to install idris-chez.

Then idris-chez is invoked when running idris with `--codegen chez` like

```idris --codegen chez example.idr -o example.ss```

```scheme --script example.ss```

Files are compiled only to Scheme source for now, if desired they can be built into object files using Chez Scheme.

## Features

* Speed!
* C FFI
* A compatibility layer for the base libraries dependence on the C rts. for example, the C calls in Prelude.Files are substituted by Scheme calls.
* Chez Scheme provides excellent stack handling and garbage collection 

## To do

* Scheme FFI (In progress!)
* Map some common but non-primitive Idris types like lists and bools to the corresponding primitives in Scheme.
* Stabilization

## Directives

```%lib chez "<shared lib>"```

In the chez backend `%lib` names dynamic libs that should be loaded at startup. The functions in that lib will then be available for FFI. See `test/samples/loadshared.idr` for an example.

```%include chez "<any scheme code>"```

In the chez backend `%include` includes arbitrary Scheme code into the resulting program. Chez Scheme's `load` or `import` can be used in an `%include` directive to actually include code from another file.     

## Benchmarks

Let's run the `pidigits` benchmark from idris' benchmarks folder!

#### idris-chez
```
$ time ./pidigits.ss 5000 > /dev/null

real    0m3.219s
user    0m0.000s
sys     0m0.030s
```
#### C backend
```
$ time ./pidigits.exe 5000 > /dev/null

real    0m10.005s
user    0m0.000s
sys     0m0.000s
```


Lets try 10000....
#### idris-chez
```
$ time ./pidigits.ss 10000 > /dev/null

real    0m12.900s
user    0m0.030s
sys     0m0.000s
```
#### C backend
```
$ time ./pidigits.exe 10000 > /dev/null
Segmentation fault

real    1m17.884s
user    0m0.016s
sys     0m0.000s
```
Ooops....