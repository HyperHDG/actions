# GitHub Action: clang-format

GitHub action that checks whether given files in a folder and its subfolders satisfy the formatting
standard defined in the `.clang-format` file.


GitHub offers to check, whether a software project's code has been formatted according to the rules
defined in its `.clang-format` file. This GitHub action contributes to this idea by offering an easy
to use interface to test whether this condition is satisfied after pushing. To use the interface,
you might add a file `clang-format.yml` to your repository's folder `.github/workflows`. This file
might contain

```
name: Clang

on: [push]

jobs:
  formatting-check:
    name: Test clang-format to change nothing
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
    - name: Conduct Clang-Format Test
      uses: HyperHDG/actions@clang-format
```

Additionally, you might define the environment `format_files` to define the files that are checked.
