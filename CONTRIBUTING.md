# Contributing to PenSkills

PenSkills is published at [Aevon-sec/PenSkills](https://github.com/Aevon-sec/PenSkills). Issues and pull requests belong on this repository. 
Contributions should stay on the scanner: detection, reporting, and the skill review workflow.

We welcome contributions. 

## How to Contribute

1. **Open an issue** describing the bug or feature you'd like to work on.
2. **Fork** the repository and create a branch for your change.
3. **Make your changes**, ensuring all tests pass (`make test`).
4. **Sign off** every commit (see below).
5. **Open a pull request** referencing the issue.

## Coding Standards

- Run `make lint` and `make format` before submitting.
- All new source files must include the SPDX license header (see any existing
  `.py` file for the template).
- New analyzers should include corresponding unit tests and, where applicable,
  test fixtures.


