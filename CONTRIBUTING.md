Contributing to JuliaQuantumControl packages
--------------------------------------------

## Reporting Defects or Suggesting Features

If you encounter problems or limitations when installing or using `QuantumControl.jl` or related packages, please do the following:

1. Search the issues of the package where you encountered the problem, including closed issues, and [Discussions](https://github.com/orgs/JuliaQuantumControl/discussions). If your search finds a report of the same problem, please post a comment in the issue.
2. If you cannot find an existing issue, please file a new issue. Describe exactly what you did, and what the actual and expected output was. When reporting a bug, always provide a complete minimal working example. That is, a script (or maybe a link to something like a [Pluto notebook](https://plutojl.org)) that can be run to reproduce the bug.


## Contributing Code or Documentation

Development of packages in the [JuliaQuantumControl] org is organized via pull requests on GitHub.

1. Clone the repository you want to contribute to, from the [JuliaQuantumControl] org. If you want to contribute to the ecosystem in the long term, or if your changes touch multiple packages in the organization, it is probably best to clone _all_ the packages in the organization via [the development environment](https://github.com/JuliaQuantumControl/JuliaQuantumControl) we provide.
2. If you do not have commit access to the repository, fork the package you want to work on to your personal GitHub account. Add your fork as a second remote to your checkout of the package. Follow the general procedure in Aaron Meurer's old-but-still relevant [Git Workflow](https://www.asmeurer.com/git-workflow/).
3. Create a branch, implement your changes, push to your remote, then open a pull request (PR).
4. A PR must include full documentation and tests. When contributing to a "stable" package (version >= 1.0), it must update the `CHANGELOG`. You may update the version number in `Project.toml` to `x.y.z-dev`, where `x.y.z` is the appropriate version number according to [semantic versioning](https://semver.org) if a release was made immediately after merging the PR. The version on every commit except commits tagged as releases [must include either the `+dev` or `-dev` suffix](https://michaelgoerz.net/notes/inter-release-versioning-recommendations.html).
5. Run tests locally and build the documentation via `make` (see `make help` for details). If you are on a non-Unix system and cannot run `make`, see the `Makefile` for the equivalent commands to run by hand. Use [JuliaFormatter] to format your code according to the settings in [`.JuliaFormatter.toml`].
6. Pull requests should always be based on the current `master`. If necessary, [rebase](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) your topic branch. Ideally, also use `git rebase` to keep your commit history clean. A pull request with well-organized commits will be merged preserving its history. Otherwise, it will be [squash-merged](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges#squash-and-merge-your-commits).


## Source Code Formatting

This project uses a code style described in [`.JuliaFormatter.toml`] and enforced via [JuliaFormatter]. For pull requests, adherence to the code style is automatically checked during continuous integration.

To apply the code style locally, you should have `JuliaFormatter` installed in your global Julia environment. Download [`.JuliaFormatter.toml`] into the root of the project folder. Lastly, in a Julia REPL within the project folder, run `using JuliaFormatter; format(".")`.

Alternatively, if you are on Unix and have `make` installed, run `make codestyle`. See `make help` for details.


## Running the Tests

There are a few way to run the tests:

* Start a Julia REPL with `julia --project=.`, then type `] test`.

* If you are on Unix and have `make` installed, run `make test`. Also consider `make devrepl`, `make coverage` and `make htmlcoverage`, see `make help` for details.

* Start a Julia REPL in the test environment with `julia --project=test`. Instantiate with `] instantiate`, then run the tests with `include("test/runtests.jl")`. On Julia < 1.11, you may need `] dev .` to ensure that the test environment uses the current code.

Run `make clean` or `make distclean` to delete coverage information, see `make help` for details.


## Building the Documentation

Use one of the following two possibilities to build the documentation locally:

* Start a Julia REPL in the docs environment with `julia --project=docs`. Instantiate with `] instantiate`, then build the documentation with `include("docs/make.jl")`. On Julia < 1.11, you may need `] dev .` to ensure that the test environment uses the current code. You can also use `make devrepl`

* If you are on Unix and have `make` installed, run `make docs`. See `make help` for details.

This will build the documentation in the `docs/build` subfolder of the project root. The preview it, you must run a web server, either via the [`LiveServer`](https://github.com/JuliaDocs/LiveServer.jl) package, or (if you have Python installed), via `python3 -m http.server`. See the [Documenter Guide](https://documenter.juliadocs.org/stable/man/guide/#Note-6b659cc6046c5199) for details.

Run `make clean` or `make distclean` to remove the documentation build, see `make help` for details.


## Maintainer Notes

See also the [README of the JuliaQuantumControl Dev Environment](https://github.com/JuliaQuantumControl/JuliaQuantumControl?tab=readme-ov-file#juliaquantumcontrol-dev-environment).


### PR review and merging

* PRs can be merged by anyone with commit access.
* PRs by authors with commit access can be self-merged after approval from a (co-)maintainer, or directly (without review) for trivial PRs
* The merge pattern outlined in [the README of the `git-merge-pr` script](https://github.com/goerz/git-merge-pr?tab=readme-ov-file#introduction) is encouraged. That is, PRs should be rebased on the current `master`, should preserve any clean history or squash unclean history, and be merged with a merge commit. That merge commit is a good place to also apply editorial changes, such as updating the version number of the changelog.


## Release process

Releases are made by the package maintainer only.

- [ ] Create a `release-x.y.z` branch
- [ ] Create a single "release commit":
    - [ ] Check the version number in `Project.toml`, bumping or removing a `-dev` suffix as necessary
    - [ ] Ensure the `CHANGELOG.md` is complete and up-to-date
- [ ] Push the `release-x.y.z` branch, but do not create a pull request
- [ ] Comment `@JuliaRegistrator register` on the commit that should be tagged as the release
- [ ] Wait for the registration to go through, for TagBot to tag the commit, and for the documentation to be built and deployed
- [ ] Manually merge the `release-x.y.z` branch into `master`, with a merge commit that bumps the version number by applying a `+dev` suffix:
    - [ ] `git merge --no-ff --no-commit release-x.y.z`
    - [ ] Update `Project.toml`
    - [ ] `git commit -m "Bump version to x.y.z+dev"`
- [ ] Push the `master` branch and delete the `release-x.y.z` branch (`git branch -D release-x.y.z && git push origin :release-x.y.z`)

[JuliaQuantumControl]: https://github.com/JuliaQuantumControl
[`.JuliaFormatter.toml`]: https://raw.githubusercontent.com/JuliaQuantumControl/JuliaQuantumControl/refs/heads/master/.JuliaFormatter.toml
[JuliaFormatter]: https://github.com/domluna/JuliaFormatter.jl
