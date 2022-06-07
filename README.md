# GitHub Action: cmake

GitHub action that builds the code within a repository and runs the tests configured in CMake.


GitHub offers to check, whether a software project's code compiles and passes some test cases, after
it has been pushed to the online repository. This GitHub action contributes to this idea by offering
an easy to use interface to test whether the tests configured in CMake work after pushing. To use
this interface, you might add a file `camke.yml` to your repository's folder `.github/workflows`.
This file might contain

```
name: CMake

on: [push]

env:
  PYTHON: python3-dev python3-numpy python3-scipy cython3

jobs:
  build_gnu10:
    name: Test build with g++-10
    runs-on: ubuntu-20.04
    env:
      CXX: g++-10

    steps:
    - name: Checkout
      uses: actions/checkout@v2
      with:
       submodules: recursive
    - name: Install Python
      run: sudo apt-get update; sudo apt-get install -y ${{env.PYTHON}} ${{env.CXX}}
    - name: Conduct CMake test
      uses: HyperHDG/actions@cmake
      with:
        cxx_compiler: ${{env.CXX}}
```

which works for software projects that contain C++ code which is used by Python scripts whose
interface is implemented in Cython. Possible input arguments of `HyperHDG/actions@cmake` are
described in the section `inputs` of this repository's `action.yml`.
