# A Julia Framework for Quantum Optimal Control.

The [JuliaQuantumControl][] organization collects packages implementing a comprehensive collection of methods of open-loop quantum optimal control.

[Quantum optimal control theory](https://link.springer.com/article/10.1140%2Fepjd%2Fe2015-60464-1) attempts to steer a quantum system in some desired way by finding optimal control parameters or control fields inside the system Hamiltonian or Liouvillian. Typical control tasks are the preparation of a specific quantum state or the realization of a logical gate in a quantum computer. Thus, quantum control theory is a critical part of realizing quantum technologies, at the lowest level. Numerical methods of *open-loop* quantum control (methods that do not involve measurement feedback from a physical quantum device) such as [Krotov's method][Krotov] and [GRAPE][] address the control problem by [simulating the dynamics of the system][QuantumPropagators] and then iteratively improving the value of a functional that encodes the desired outcome.


The packages in [JuliaQuantumControl][] that implement specific individual methods are combined in a meta-package [`QuantumControl.jl`][QuantumControl]. For normal usage, i.e., outside of development within the [JuliaQuantumControl][] organization, it should be sufficient to interact only with the [`QuantumControl.jl`][QuantumControl] interface.

## Packages

| Package | Version | CI Status | Coverage | Description |
| --- | --- | --- | --- | ---|
|[QuantumPropagators.jl](https://github.com/JuliaQuantumControl/QuantumPropagators.jl) | [![version](https://juliahub.com/docs/QuantumPropagators/version.svg)](https://juliahub.com/ui/Packages/QuantumPropagators/ApFlo) | [![Build Status](https://github.com/JuliaQuantumControl/QuantumPropagators.jl/workflows/CI/badge.svg)](https://github.com/JuliaQuantumControl/QuantumPropagators.jl/actions) | [![Coverage](https://codecov.io/github/JuliaQuantumControl/QuantumPropagators.jl/branch/master/graph/badge.svg)](https://codecov.io/github/JuliaQuantumControl/QuantumPropagators.jl) | Simulating the time evolution of quantum systems |
|[QuantumControlBase.jl](https://github.com/JuliaQuantumControl/QuantumControlBase.jl) | [![version](https://juliahub.com/docs/QuantumControlBase/version.svg)](https://juliahub.com/ui/Packages/QuantumControlBase/bTokw) | [![Build Status](https://github.com/JuliaQuantumControl/QuantumControlBase.jl/workflows/CI/badge.svg)](https://github.com/JuliaQuantumControl/QuantumControlBase.jl/actions) | [![Coverage](https://codecov.io/github/JuliaQuantumControl/QuantumControlBase.jl/branch/master/graph/badge.svg)](https://codecov.io/github/JuliaQuantumControl/QuantumControlBase.jl) | Shared methods and data structures for quantum control |
|[Krotov.jl](https://github.com/JuliaQuantumControl/Krotov.jl) | [![version](https://juliahub.com/docs/Krotov/version.svg)](https://juliahub.com/ui/Packages/Krotov/3mCxK) | [![Build Status](https://github.com/JuliaQuantumControl/Krotov.jl/workflows/CI/badge.svg)](https://github.com/JuliaQuantumControl/Krotov.jl/actions) | [![Coverage](https://codecov.io/github/JuliaQuantumControl/Krotov.jl/branch/master/graph/badge.svg)](https://codecov.io/github/JuliaQuantumControl/Krotov.jl) | Krotov's method of optimal control |
|[GRAPE.jl](https://github.com/JuliaQuantumControl/GRAPE.jl) | [![version](https://juliahub.com/docs/GRAPE/version.svg)](https://juliahub.com/ui/Packages/GRAPE/W0mna) | [![Build Status](https://github.com/JuliaQuantumControl/GRAPE.jl/workflows/CI/badge.svg)](https://github.com/JuliaQuantumControl/GRAPE.jl/actions) | [![Coverage](https://codecov.io/github/JuliaQuantumControl/GRAPE.jl/branch/master/graph/badge.svg)](https://codecov.io/github/JuliaQuantumControl/GRAPE.jl) | Gradient Ascent Pulse Engineering method |
|[QuantumControl.jl](https://github.com/JuliaQuantumControl/QuantumControl.jl) | [![version](https://juliahub.com/docs/QuantumControl/version.svg)](https://juliahub.com/ui/Packages/QuantumControl/no1zM) | [![Build Status](https://github.com/JuliaQuantumControl/QuantumControl.jl/workflows/CI/badge.svg)](https://github.com/JuliaQuantumControl/QuantumControl.jl/actions) | [![Coverage](https://codecov.io/github/JuliaQuantumControl/QuantumControl.jl/branch/master/graph/badge.svg)](https://codecov.io/github/JuliaQuantumControl/QuantumControl.jl) | Meta-package to combine all packages in a high-level API |


## Documentation

Read the [`QuantumControl.jl` documentation](https://juliaquantumcontrol.github.io/QuantumControl.jl/) for a comprehensive description of all the packages in the [JuliaQuantumControl][] organization.

Support is also available in the `#quantumcontrol` channel in the [Julia Slack](https://julialang.org/slack/).


## Installation

The [`QuantumControl.jl`][QuantumControl] meta-package can be installed via the [standard Pkg manager](https://docs.julialang.org/en/v1/stdlib/Pkg/):

~~~
pkg> add QuantumControl
~~~

This will also install all the packages of the [JuliaQuantumControl][] organization as dependencies.

## Development

Development or pre-release use of [JuliaQuantumControl][] packages requires a [dev-installation](https://pkgdocs.julialang.org/v1/managing-packages/#developing) of *all* packages. To facilitate this, we provide a [development environment][JuliaQuantumControlDev] for the entire organization in the [`JuliaQuantumControl` repository][JuliaQuantumControlDev] . Cloning that repository provides a `Makefile` to develop across all packages. In order to use this `Makefile`, it is strongly recommended that you use Unix as a development platform (Linux, macOS, or [WSL](https://docs.microsoft.com/en-us/windows/wsl/about) on Windows).

See [the dev-environment README](https://github.com/JuliaQuantumControl/JuliaQuantumControl#readme) for details.

[JuliaQuantumControl]: https://github.com/JuliaQuantumControl
[JuliaQuantumControlDev]: https://github.com/JuliaQuantumControl/JuliaQuantumControl
[Krotov]: https://github.com/JuliaQuantumControl/Krotov.jl
[GRAPE]: https://github.com/JuliaQuantumControl/GRAPE.jl
[QuantumPropagators]: https://github.com/JuliaQuantumControl/QuantumPropagators.jl
[QuantumControl]: https://github.com/JuliaQuantumControl/QuantumControl.jl
