Contributing to JuliaQuantumControl packages
--------------------------------------------

## Reporting Defects or Suggesting Features

If you encounter problems or limitations when installing or using `QuantumControl.jl` or related packages, please do the following:

1. Search the [issues](https://github.com/JuliaQuantumControl/QuantumControl.jl/issues) of the package where you encountered the problem, including closed issues, and [Discussions](https://github.com/orgs/JuliaQuantumControl/discussions). If your search finds a report of the same problem, please post a comment in the issue.
2. If you cannot find an existing issue, please file a new issue. Describe exactly what you did, and what the actual and expected output was. When reporting a bug, always provide a complete minimal working example. That is, a script (or maybe a link to something like a [Pluto notebook](https://plutojl.org)) that can be run to reproduce the bug.

## Contributing Code or Documentation

Development of packages in the [JuliaQuantumControl][] org is organized via pull requests on GitHub.

1. Clone the repository you want to contribute to, from the [JuliaQuantumControl][] org. If you want to contribute to the ecosystem in the long term, or if your changes touch multiple packages in the organization, it is probably best to clone _all_ the packages in the organization via [the development environment](https://github.com/JuliaQuantumControl/JuliaQuantumControl) we provide.
2. If you do not have commit access to the repository, fork the package you want to work on to your personal GitHub account. Add your fork as a second remote to your checkout of the package. Follow the general procedure in Aaron Meurer's old-but-still relevant [Git Workflow](https://www.asmeurer.com/git-workflow/).
3. Create a branch, implement your changes, push to your remote, then open a pull request (PR).
4. A PR must include full documentation and tests. When contributing to a "stable" package (version >= 1.0), it must update the `CHANGELOG`. You may update the version number in `Project.toml` to `x.y.z-dev`, where `x.y.z` is the appropriate version number according to [semantic versioning](https://semver.org) if a release was made immediately after merging the PR. The version on every commit except commits tagged as releases [must include either the `+dev` or `-dev` suffix](https://michaelgoerz.net/notes/inter-release-versioning-recommendations.html).
5. Run tests locally and build the documentation via `make` (see `make help` for details). If you are on a non-Unix system and cannot run `make`, see the `Makefile` for the equivalent commands to run by hand. Use [JuliaFormatter](https://github.com/domluna/JuliaFormatter.jl) to format your code according to the settings in [`.JuliaFormatter.toml`](https://github.com/JuliaQuantumControl/JuliaQuantumControl/blob/master/.JuliaFormatter.toml).
6. Pull requests should always be based on the current `master`. If necessary, [rebase](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) your topic branch. Ideally, also use `git rebase` to keep your commit history clean. A pull request with well-organized commits will be merged preserving its history. Otherwise, it will be [squash-merged](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges#squash-and-merge-your-commits).

[JuliaQuantumControl]: https://github.com/JuliaQuantumControl
