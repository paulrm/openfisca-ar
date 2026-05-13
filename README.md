# OpenFisca Argentina

[![Python](https://img.shields.io/pypi/pyversions/openfisca-ar.svg)](https://pypi.python.org/pypi/openfisca-ar)
[![PyPI](https://img.shields.io/pypi/v/openfisca-ar.svg?style=flat)](https://pypi.python.org/pypi/openfisca-ar)

> **Note:** This is an early-stage implementation. The modelling of Argentina's
> tax and benefit system is not yet complete. Contributions are very welcome!
>
> The Python module is currently named `openfisca_argentina` (inherited
> from the OpenFisca country template). It will be renamed to `openfisca_ar`
> in a future step. Commands and paths in this README reflect the current
> state of the repository.

## Introduction

[OpenFisca](https://openfisca.org) is a versatile open-source microsimulation
framework. This repository contains the OpenFisca model of the Argentine tax
and benefit system.

For more information on OpenFisca's features and usage, see the
[general OpenFisca documentation](https://openfisca.org/doc/).

## What is Modelled

The following elements of Argentina's tax and benefit system are currently
modelled. All legislation is in the `openfisca_argentina` folder.

- **Taxes** (`variables/taxes.py`, `parameters/taxes/`):
  - Income tax (`income_tax`) — a flat-rate tax applied to salary, capital
    returns, and pension income.
  - Social security contribution (`social_security_contribution`) — a
    progressive contribution on salaries, computed via a marginal scale.
  - Housing tax (`housing_tax`) — an annual tax proportional to the size of
    the household's accommodation.

- **Benefits** (`variables/benefits.py`, `parameters/benefits/`):
  - Basic income (`basic_income`) — a monthly allowance for adults,
    introduced in December 2015.
  - Housing allowance (`housing_allowance`) — a rental allowance available
    until November 2016.
  - Pension (`pension`) — a monthly benefit for individuals above retirement
    age.
  - Parenting allowance (`parenting_allowance`) — an allowance for
    low-income households with dependent children.

- **Reforms** (`reforms/`):
  - A reform project that modifies social security taxation brackets is
    included as an example.

> This is an initial implementation. Many taxes, transfers, and social
> contributions still need to be added. See the
> [issues](https://github.com/paulrm/openfisca-ar/issues) for planned work.

## Installation

This package requires [Python 3.9](https://www.python.org/downloads/) or
later. GNU/Linux, macOS, and Windows are all supported.

### A. Minimal Installation (pip)

Follow this path if you want to:

- run calculations on a population;
- create tax and benefit simulations;
- write an extension on top of this legislation;
- serve the package with the OpenFisca Web API.

```sh
pip install openfisca-ar
```

:tada: OpenFisca Argentina is now installed and ready to use!

#### Next Steps

- Learn how to use OpenFisca by following the
  [tutorials](https://openfisca.org/doc/).
- Serve the package with the
  [OpenFisca Web API](#serve-openfisca-argentina-with-the-web-api).

Additional packages you may find useful:

- [matplotlib](http://matplotlib.org/) — to plot simulation results.
- [pandas](http://pandas.pydata.org/) — to manage tabular data.
- See the
  [Extensions documentation](https://openfisca.org/doc/contribute/extensions.html)
  to build on top of this package.

### B. Advanced Installation (Git Clone)

Follow this path if you want to:

- add or modify the modelled legislation;
- contribute to the source code.

Make sure [Git](https://www.git-scm.com/) is installed, then:

```sh
git clone https://github.com/paulrm/openfisca-ar.git
cd openfisca-ar
pip install --upgrade pip build twine
pip install --editable ".[dev]" --upgrade
```

Verify the installation by running the tests:

```sh
make test
```

> [Learn more about tests](https://openfisca.org/doc/coding-the-legislation/writing_yaml_tests.html)

:tada: OpenFisca Argentina is now installed in development mode!

#### Next Steps

- To write new legislation, read
  [Coding the legislation](https://openfisca.org/doc/coding-the-legislation/index.html).
- To contribute to the code, read the
  [Contribution Guidebook](https://openfisca.org/doc/contribute/index.html).

### C. Using uv (recommended)

[uv](https://docs.astral.sh/uv/) is a fast Python package manager that
simplifies dependency management.

```sh
uv init
uv add openfisca-ar
uv add openfisca-core[web-api]  # optional, needed for the Web API
```

For development:

```sh
git clone https://github.com/paulrm/openfisca-ar.git
cd openfisca-ar
uv sync --group dev
```

## Testing

Run the test suite with:

```sh
make test
```

Or directly with uv:

```sh
uv run openfisca test --country-package openfisca_argentina openfisca_argentina/tests
```

## Code Style

This repository enforces a consistent code style using
[Ruff](https://docs.astral.sh/ruff/).

Check for style issues:

```sh
make lint
```

Auto-fix style issues:

```sh
make format
```

## Contributing

All contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md)
for guidelines on how to open pull requests, document changes, and bump the
version number.

In short:

- We follow [GitHub Flow](https://guides.github.com/introduction/flow/).
- We use [semantic versioning](http://semver.org/) (see below).
- Every change must be documented in [CHANGELOG.md](CHANGELOG.md).

## Versioning Strategy

OpenFisca Argentina follows [semantic versioning](http://semver.org/).
Every published version conveys API compatibility information:

| Change type | Version bump | Example |
|---|---|---|
| Renaming or removing a variable | **Major** (`X.0.0`) | `1.0.0` → `2.0.0` |
| Adding a new variable | **Minor** (`x.Y.0`) | `1.0.0` → `1.1.0` |
| Fixing or improving a calculation | **Patch** (`x.y.Z`) | `1.0.0` → `1.0.1` |

All changes are documented in [CHANGELOG.md](CHANGELOG.md). Users who pin a
minor version (e.g. `>=1.2.0,<2`) are guaranteed backwards-compatible
calculations.

> For example, an application using version `1.1.0` will also work with
> `1.2.0`. An upgrade to `2.0.0` may require client-side adaptation.

New versions are published automatically to
[PyPI](https://pypi.org/project/openfisca-ar/) via GitHub Actions whenever a
change is merged to the `main` branch.

## Serve OpenFisca Argentina with the Web API

You can serve the OpenFisca Web API locally:

```sh
openfisca serve --port 5000 --country-package openfisca_argentina
```

Or with the Makefile shortcut:

```sh
make serve-local
```

Check that the API is running:

```sh
curl "http://localhost:5000/spec"
```

This returns the [OpenAPI specification](https://www.openapis.org/) of your
instance.

:tada: OpenFisca Argentina is now served by the OpenFisca Web API! See the
[Web API documentation](https://openfisca.org/doc/openfisca-web-api/index.html)
to learn more.

Run a sample calculation against the API:

```sh
curl -X POST -H "Content-Type: application/json" \
  -d @./openfisca_argentina/situation_examples/couple.json \
  http://localhost:5000/calculate
```

## Publishing a New Version

This package is distributed as a Python package on
[PyPI](https://pypi.org/project/openfisca-ar/).

### Automatic (via GitHub Actions)

Merging to `main` triggers the continuous deployment workflow, which builds
and publishes the package automatically. To activate it:

1. Create an account on [PyPI](https://pypi.org/) if you don't have one.
2. Generate an API token in your PyPI account settings.
3. Add the token to your GitHub repository secrets as `PYPI_TOKEN`.

### Manual

Follow the
[Python Packaging Authority guidelines](https://packaging.python.org/tutorials/packaging-projects/)
to build and upload manually.

## Contributors

See the [list of contributors](https://github.com/paulrm/openfisca-ar/graphs/contributors).
