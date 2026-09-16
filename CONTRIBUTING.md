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
5. Run the tests locally and build the documentation, see the [Development Workflow](#development-workflow). Apply the code style with `make codestyle`.
6. If your changes require unreleased changes in a sibling package, see [Sibling Packages](#sibling-packages).
7. Pull requests should always be based on the current `master`. If necessary, [rebase](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) your topic branch. Ideally, also use `git rebase` to keep your commit history clean. A pull request with well-organized commits will be merged preserving its history. Otherwise, it will be [squash-merged](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges#squash-and-merge-your-commits).


## Development Workflow

All packages in the [JuliaQuantumControl] organization share the same development workflow. Each package has a `Makefile` with the following targets (run `make help` for a summary):

| Target              | Description                                                                              |
|:--------------------|:-----------------------------------------------------------------------------------------|
| `make test`         | Run the test suite in the `test` environment                                             |
| `make devrepl`      | Start a REPL for running individual tests and building the documentation interactively   |
| `make docs`         | Build the documentation in the `docs` environment                                        |
| `make coverage`     | Run the test suite with coverage, write `lcov.info`, and show a summary                  |
| `make htmlcoverage` | Like `make coverage`, and write an HTML report to `./coverage` (requires `genhtml` from [lcov](https://github.com/linux-test-project/lcov)) |
| `make codestyle`    | Apply the code style, and check `CHANGELOG.md` and the `[sources]` of all environments   |
| `make clean`        | Remove build, documentation, and coverage artifacts                                      |
| `make distclean`    | Also remove all `Manifest.toml` files, restoring a clean checkout                        |

The `make` targets require a Unix system ([WSL](https://docs.microsoft.com/en-us/windows/wsl/) on Windows) and Julia ≥ 1.11. We assume Julia 1.13 for local development. See [Older Julia Versions](#older-julia-versions) for running the tests with an older version of Julia. If you cannot use `make`, see the `Makefile` for the equivalent Julia commands.

We recommend installing [Revise](https://github.com/timholy/Revise.jl) and [LiveServer](https://github.com/JuliaDocs/LiveServer.jl) in your global Julia environment.


### Environments

Each package has two independent Julia project environments:

* `test/Project.toml` for running the tests
* `docs/Project.toml` for building the documentation

Both environments include the package itself via the `[sources]` entry `{path = ".."}`. They list only what the tests or the documentation actually use. Development tools like JuliaFormatter or the coverage tools are not part of either environment: the `make` targets install them into temporary environments.

The root `Project.toml` of a package has neither `[sources]` nor a `[workspace]` section. See the [Design Rationale](#design-rationale) for the reasons.


### Running the Tests

Run `make test` to run the full test suite. Like `Pkg.test` on CI, `make test` puts only the `test` environment on the `LOAD_PATH` (`JULIA_LOAD_PATH="@"`), so a package that the tests use but that is missing from `test/Project.toml` (including standard libraries like `Random`) causes an error. Without `make`, start Julia with `julia --project=test`, run `] instantiate`, and then `include("test/runtests.jl")`.

To run the tests with coverage, use `make coverage` or `make htmlcoverage`.

For working on the tests, use the [development REPL](#development-repl).


### Development REPL

`make devrepl` starts a Julia REPL with the `test` environment active and the `docs` environment added to the `LOAD_PATH`, with Revise loaded from your global environment. This allows you to run individual test files (e.g., `include("test/test_invalid_interfaces.jl")`), the entire test suite (`include("test/runtests.jl")`), or to build the documentation (`include("docs/make.jl")`) repeatedly in the same session. Building the documentation from a running REPL is much faster than `make docs`, which starts a fresh Julia process. The REPL disables the checking of external links in the documentation.

Stacking environments has a caveat: a package that is in more than one environment with different versions can only be loaded in one version. At startup, `make devrepl` lists any such packages in the `test`, `docs`, and global environments (for the latter, only Revise and its dependencies). If a problem might be related to one of the listed packages, reproduce it in a single environment with `julia --project=test` or `julia --project=docs`, or with `make test` or `make docs`.


### Building the Documentation

Run `make docs` to build the documentation in the `docs/build` subfolder. For interactive work on the documentation, use `include("docs/make.jl")` in the [development REPL](#development-repl). Without `make`, start Julia with `julia --project=docs`, run `] instantiate`, and then `include("docs/make.jl")`.

To preview the documentation, run a web server, e.g., with `using LiveServer; serve(dir="docs/build")` from your global environment, or with `python3 -m http.server --directory docs/build`. See the [Documenter Guide](https://documenter.juliadocs.org/stable/man/guide/#Note-6b659cc6046c5199) for details.

The `docs/make.jl` script only builds the documentation. Deployment happens in [continuous integration](#continuous-integration), via `docs/deploy.jl`.


### Source Code Formatting

This project uses a code style described in [`.JuliaFormatter.toml`] and enforced via [JuliaFormatter]. For pull requests, adherence to the code style is automatically checked during continuous integration. The formatting depends on the exact JuliaFormatter version, which is set by `JULIAFORMATTER_VERSION` in the `Makefile`. The CI code style check uses the same `Makefile`.

Run `make codestyle` to apply the code style. It installs the pinned version of JuliaFormatter into a temporary environment, fetches the [`.JuliaFormatter.toml`] automatically, and also checks `CHANGELOG.md` and the `[sources]` of all environments.

Without `make`, install the pinned JuliaFormatter version in a separate Julia environment, download [`.JuliaFormatter.toml`] into the root of the project folder, and in a Julia REPL within the project folder run `using JuliaFormatter; format(".")`.


### Sibling Packages

The packages in the organization depend on each other. By default, the `test` and `docs` environments use the *registered releases* of all sibling packages. This ensures that a package is tested against the versions of its siblings that users will actually install.

If a change requires an unreleased change in a sibling package, point the environment to the branch of the sibling repository that contains the change, via a `[sources]` entry in `test/Project.toml` and/or `docs/Project.toml`, e.g.:

```toml
[sources]
QuantumControl = {path = ".."}
QuantumPropagators = {url = "https://github.com/JuliaQuantumControl/QuantumPropagators.jl", rev = "master"}
```

On Julia 1.13, `] add https://github.com/JuliaQuantumControl/QuantumPropagators.jl#master` in the respective environment writes this entry. On Julia 1.11 and 1.12, edit the `Project.toml` file by hand. The sibling must also be listed in `[deps]`. If both the tests and the documentation require the unreleased sibling, add the entry to both environments.

The following rules apply to `[sources]`, and are enforced by the codestyle CI check (and `make codestyle`):

* The root `Project.toml` of a package must not have a `[sources]` section.
* The only allowed `path` entry is the package itself (`{path = ".."}`). A local path to a sibling package must never be committed.
* Every other entry must have a `url` of a GitHub repository whose name matches the package (e.g., a fork), and a `rev` that is an existing branch or tag in that repository.

Such entries should be temporary. The CI jobs show an "Unreleased sibling package" warning for every sibling that is not a registered release. Remove the entry as soon as the sibling has been released. The recommended order is to merge the pull request for the sibling first, point the `rev` to `master`, then merge the dependent pull request, and remove the entry after the sibling has been released.

To remove an entry from the Julia REPL, use `] free QuantumPropagators` (Julia ≥ 1.11). Note that `] add QuantumPropagators` does *not* switch back to the registered release on Julia 1.11 and 1.12: it silently keeps the `[sources]` entry.


### Local Checkouts of Sibling Packages

In the [development environment](https://github.com/JuliaQuantumControl/JuliaQuantumControl), you can switch the environments of a package to the local checkouts of all of its sibling packages (including indirect dependencies), e.g., to test changes across several packages before pushing any of them. In the package folder, run

```
julia ../scripts/installorg.jl
```

On Julia ≥ 1.11, this writes relative `path` entries for the siblings to the `[sources]` of `test/Project.toml` and `docs/Project.toml`. On Julia 1.10, it uses `Pkg.develop` instead. To switch back, run

```
julia ../scripts/installorg.jl --revert
```

which restores the original `Project.toml` files and re-instantiates the environments. Never commit the changes made by `installorg.jl`: the codestyle CI check rejects local paths in `[sources]`.

Note that Julia 1.13 writes a `[sources]` entry for every package that the `Manifest.toml` tracks by path whenever it resolves the environment. A `Manifest.toml` left over from local checkouts (e.g., from a checkout that predates the current workflow) can therefore add local paths to `Project.toml` during `make test` or `make docs`. Run `make distclean` to remove stale `Manifest.toml` files, and check `make codestyle` before committing.


### Older Julia Versions

The oldest Julia version supported by the packages is Julia 1.10. Continuous integration tests every package with the oldest supported version. To reproduce a problem with an older version of Julia locally, run e.g.

```
make distclean
make test JULIA="julia +1.11"
```

(the `+1.11` channel selection requires [juliaup](https://github.com/JuliaLang/juliaup)). Run `make distclean` again when switching back to the default Julia version, since the `Manifest.toml` files depend on the Julia version.

Julia 1.10 ignores `[sources]`. On Julia 1.10, the `make` targets use `../scripts/envcheck.jl apply-sources` to install the package itself and any URL `[sources]` explicitly. This requires the development environment.


### Continuous Integration

The `CI` workflow of each package has the following jobs:

* **Test**: Runs the tests with the latest Julia release, via [julia-runtest](https://github.com/julia-actions/julia-runtest), and uploads the coverage to [Codecov](https://codecov.io). It warns about [unreleased sibling packages](#sibling-packages), and about dependencies of the package that the test environment holds back below the newest version that the package's `[compat]` allows (in which case the newest version is not actually tested). For pull requests by Dependabot, julia-runtest requires the latest compatible versions of all dependencies.
* **Test (oldest Julia, lowest compat bounds)**: Runs the tests with the oldest supported Julia version and the lowest versions of all dependencies that the `[compat]` bounds allow, using [julia-downgrade-compat](https://github.com/julia-actions/julia-downgrade-compat). A failure means that a lower compat bound must be raised.
* **Documentation**: Builds the documentation and uploads it as the `docs-build` artifact. The build runs without write permissions and without any secrets.
* **Deploy documentation**: Deploys the `docs-build` artifact to the `gh-pages` branch by running `docs/deploy.jl`, which installs only Documenter and calls `deploydocs`. For pull requests, this job runs only for branches of the package repository, not for pull requests from forks or from Dependabot, and it deploys a preview only if `docs/deploy.jl` sets `push_preview = true`. To review the documentation of such a pull request, download the `docs-build` artifact from the CI run, and run `python3 -m http.server --directory build` in the unzipped folder.
* **Codestyle**: Checks spelling ([typos](https://github.com/crate-ci/typos)), the version number, the `[sources]`, the code style, and `CHANGELOG.md`.

Some packages also test their downstream packages (e.g., `QuantumControl` runs the tests of `Krotov` and `GRAPE` against its current version). These jobs are informational: a failure means that the downstream package must be adapted.

The scripts used by the CI jobs are downloaded from the `master` branch of the [development environment](https://github.com/JuliaQuantumControl/JuliaQuantumControl) (`scripts/envcheck.jl`).

To reproduce the lowest-compat job locally, run [`downgrade.jl`](https://github.com/julia-actions/julia-downgrade-compat) in a clean Julia depot that only has the General registry: the retired `QuantumControlRegistry` in a depot makes it crash on Julia 1.13.


## Design Rationale

The development workflow follows the standard workflow for Julia packages as closely as possible, with the following decisions specific to the interdependent packages in the organization.

### Registered sibling packages by default

A package is only useful to users with the registered versions of its siblings. Testing against unreleased development versions (as the organization did in the past) hides incompatibilities until a release, and a green CI run does not show that the package works with what users install. Temporary `[sources]` entries allow depending on unreleased changes where necessary, but the CI warnings make them visible, so that they are removed quickly.

### No local paths and no `[sources]` in the root `Project.toml`

A local path only exists on the machine of the developer who wrote it. Since Julia 1.13 writes such paths into `[sources]` automatically (e.g., with `Pkg.develop`), they are easy to commit accidentally, so CI rejects them. `[sources]` in the root `Project.toml` of a package would also apply to every environment that develops the package (Julia ≥ 1.13 follows the `[sources]` of path-tracked dependencies), so they are only allowed in the `test` and `docs` environments.

### Independent `test` and `docs` environments (no workspace)

A [Pkg workspace](https://pkgdocs.julialang.org/v1/toml-files/#The-%5Bworkspace%5D-section) would resolve the package, the `test` environment, and the `docs` environment into a single `Manifest.toml`. We deliberately do not use one:

* The dependencies of the documentation (e.g., Plots, DocumenterCitations, or downstream packages like Krotov and GRAPE in the documentation of QuantumControl) would constrain the versions of the dependencies in the tests, silently holding back what is being tested.
* In a workspace, `Pkg.test` does not create a temporary test environment, which disables the `force_latest_compatible_version` check that julia-runtest applies to Dependabot pull requests. That check makes a compat bump fail loudly when the new version cannot actually be installed.
* A workspace adds failure modes: a sibling `[sources]` entry in one environment silently applies to the other, conflicting entries are merged without an error, and instantiating the `docs` environment first can produce an inconsistent manifest.

The benefits of a workspace (a single resolve, and consistent versions in the development REPL) are small in comparison. The [development REPL](#development-repl) reports the packages for which stacking the environments makes a difference instead.

### Development tools outside of the test environment

Every package in `test/Project.toml` is resolved together with the dependencies of the package under test, so its `[compat]` bounds can prevent the newest version of a dependency from being tested, without any error. For example, TimerOutputs 1.0 could not be tested in QuantumPropagators, because the coverage tools in the test environment required an old version of PrettyTables. Therefore, JuliaFormatter and the coverage tools run in temporary environments, and Revise and LiveServer come from the global environment. For the same reason, `QuantumControlTestUtils` depends only on the Julia standard library, and test helpers that require `QuantumControl` (the experimental `QuantumControl.DummyOptimization` module) live in `QuantumControl` itself.

### `installorg.jl` is optional

Switching all environments to local sibling checkouts by default (as the organization did in the past) conflicts with testing against registered releases, and required a custom setup for every `make` target and CI job. It remains available for testing changes across several packages locally.

### Separate documentation build and deployment

Building the documentation runs a lot of third-party code: every package in the `docs` environment, including package build scripts, and all code examples. If a compromised release of any of these packages ran in a job that can push to the repository, it could modify any branch or tag. Therefore, the **Documentation** job builds with a read-only token, and the **Deploy documentation** job, which has write permissions, never runs the documentation code. It does not restore the cache of the build job, either. Removing the token from the environment of the build step would not be enough, since `actions/checkout` stores the token on disk (hence `persist-credentials: false`).

Dependabot pull requests get no previews because GitHub gives them a token with write permissions when a workflow requests one, and a Dependabot pull request runs new versions of dependencies before anyone has reviewed them. The condition for the deploy job checks the author of the pull request, not `github.actor`, which can be spoofed. Pull requests from forks only ever get a read-only token; the `docs-build` artifact allows reviewing their documentation.

The deployment authenticates with `GITHUB_TOKEN` only. The `DOCUMENTER_KEY` deploy key is used by TagBot, so that a new tag triggers the `CI` workflow; it is not needed for the deployment itself, and it would expose a long-lived credential to the build.

### Oldest Julia version and lowest compat bounds

The `[compat]` bounds of a package are a promise to users. The lowest-compat CI job verifies the lower bounds, and runs with the oldest supported Julia version, which is also the oldest version any user might have. Local development uses the current Julia release.


## Maintainer Notes

See also the [README of the JuliaQuantumControl Dev Environment](https://github.com/JuliaQuantumControl/JuliaQuantumControl?tab=readme-ov-file#juliaquantumcontrol-dev-environment).


### PR review and merging

* PRs can be merged by anyone with commit access.
* PRs by authors with commit access can be self-merged after approval from a (co-)maintainer, or directly (without review) for trivial PRs
* The merge pattern outlined in [the README of the `git-merge-pr` script](https://github.com/goerz/git-merge-pr?tab=readme-ov-file#introduction) is encouraged. That is, PRs should be rebased on the current `master`, should preserve any clean history or squash unclean history, and be merged with a merge commit. That merge commit is a good place to also apply editorial changes, such as updating the version number of the changelog.


## Release process

Releases are made by the package maintainer only.

- [ ] Choose the appropriate version number `x.y.z` for the release.

  Post-1.0, breaking changes should be rare an require a major-version bump, following semantic versioning (semver).

  Pre-1.0, breaking changes are a minor (`0.x.0`) release, and new features or bugfixes get grouped into the third number.

  Review the `CHANGELOG.md`, merged PRs and closed issues, as well as the commit history since the last tagged release to determine the appropriate version number for the release.

- [ ] Check that sibling packages that the release requires have been released. Ideally, the `test` and `docs` environments of the release commit have no [sibling `[sources]`](#sibling-packages), and CI shows no "Unreleased sibling package" warnings. An exception is a breaking release whose test or documentation environment includes *downstream* packages: those can only be released after this package, so their `[sources]` may have to point to their `master` branch.

- [ ] Create a `release-x.y.z` branch

- [ ] Create a single "release commit":

    - [ ] Check the version number in `Project.toml`, bumping or removing a `-dev` suffix as necessary

    - [ ] If any dependencies have an open compat bound (`>=`), cap it for the release commit by removing `>=`.

    - [ ] Ensure the `CHANGELOG.md` is complete and up-to-date as a human-facing document.

      All user-facing changes should be reflected in the `CHANGELOG.md`. Internal
      changes or changes to development tooling may be omitted from the `CHANGELOG.md`.

      For breaking changes, the `CHANGELOG.md` must contain instructions on how to adapt to the change.

    - [] Remove the "Unreleased" header from the `CHANGELOG.md`.

- [ ] Push the `release-x.y.z` branch, but do not create a pull request. Check that all CI jobs pass, including the lowest-compat job.

- [ ] Comment `@JuliaRegistrator register` on the commit that should be tagged as the release.

  Include a `Release notes:` section in the comment, after the trigger line pointing to the `CHANGELOG.md`:

  ```
  @JuliaRegistrator register

  Release notes:

  See the [changelog](https://github.com/JuliaQuantumControl/<Package>.jl/blob/vx.y.z/CHANGELOG.md).
  ```

  The release notes become the body of the GitHub release created by TagBot.

- [ ] Wait for the registration to go through, for TagBot to tag the commit, and for the documentation to be built and deployed

- [ ] Manually merge the `release-x.y.z` branch into `master`, with a merge commit that bumps the version number by applying a `+dev` suffix:

    - [ ] `git merge --no-ff --no-commit release-x.y.z`

    - [ ] Update `Project.toml`

    - [ ] Revert any temporarily open compat bounds (restore any `>=` removed in step 2). Pre-1.0 dependencies in the JuliaQuantumControl org should have open compat bounds on the `master` branch.

    - [ ] Add a forward-looking "Unreleased" header back to the top of the `CHANGELOG.md` file:

      ```
      ## [Unreleased]


      ## [v<x.y.z>] — <YYYY-MM-DD>

      …

      [Unreleased]: https://github.com/JuliaQuantumControl/<Package>.jl/compare/v<x.y.z>..HEAD
      ```

      where `x.y.z` is the version that was just released. Sections in the `CHANGELOG.md` are separated by two blank lines, so the empty "Unreleased" section has two blank lines before and after its heading

    - [ ] `git commit -m "Bump version to x.y.z+dev"`

- [ ] Push the `master` branch and delete the `release-x.y.z` branch (`git branch -D release-x.y.z && git push origin :release-x.y.z`)

- [ ] In sibling packages whose `[sources]` point to a branch of this package, remove the `[sources]` entry now that the release is registered.

[JuliaQuantumControl]: https://github.com/JuliaQuantumControl
[`.JuliaFormatter.toml`]: https://raw.githubusercontent.com/JuliaQuantumControl/JuliaQuantumControl/refs/heads/master/.JuliaFormatter.toml
[JuliaFormatter]: https://github.com/domluna/JuliaFormatter.jl
