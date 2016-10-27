# NSWI098 CMake

This is a CMake config for the testing framework of NSWI098 Compiler Principles
as taught at the Faculty of Mathematics and Physics at the Charles University in
Prague during the winter semester of 2016.

**If you find any bugs or if you can help to improve this config, please do send
me an e-mail. All suggestions are appreciated.**

## Dependencies

On Arch Linux, you'll need about these packages (all of which are official):

  * `bison`
  * `flex`
  * `libxslt`
  * `libxml2`

Everything works with latest versions.

## What does it do

This CMake config was reverse-engineered from the `*.vcxproj` files of MS VS.
It provides basically the same service on Unices the VS version does on Windows,
with the advantage of being human-readable, two kilometers shorter and twice
as fast.

Several source files and headers are auto-generated (and re-generated as needed).
The only binary target is the `mlc` executable which is the actual compiler.

Feel free to ingore the warning of `YY_NULLPTR` being redefined.

## Usage

I assume you unpacked the zipped framework to `assignment`.

    git clone git@github.com:dcepelik/nswi098-cmake.git
    cp nswi098-cmake/CMakeLists.txt assignment

    cd assignment
    mkdir build
    cd build
    cmake .. && make

The `mlc` executable will be found at `build`, all the generated files reside
in `build/generated`.

**With future assignments, you'll probably need to `git fetch` the repository
to get up-to-date version of the CMakeLists.txt file.**
