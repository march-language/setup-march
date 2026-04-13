# setup-march

A GitHub Action that installs the [March programming language](https://github.com/march-language/march) in your CI workflow.

## Usage

```yaml
- uses: march-language/setup-march@v1
  with:
    march-version: main
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `march-version` | Branch, tag, or commit SHA to install | `main` |
| `ocaml-compiler` | OCaml compiler version used to build March | `5.3.0` |
| `github-token` | GitHub token to avoid API rate limits | `${{ github.token }}` |

## Outputs

| Output | Description |
|--------|-------------|
| `march-version` | The commit SHA of the installed March version |
| `march-path` | Path to the March installation directory |

## Example

```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: march-language/setup-march@v1
        with:
          march-version: main

      - run: march my_program.march
```

## How it works

1. Resolves the `march-version` input to a commit SHA for reliable cache keying.
2. Restores the March build from cache if available (keyed by SHA + OS + OCaml version).
3. On a cache miss: sets up OCaml via [`ocaml/setup-ocaml`](https://github.com/ocaml/setup-ocaml), clones the march repo, installs opam dependencies, and builds with `dune`.
4. Adds `march` to `PATH`.

Cached runs skip OCaml setup entirely, keeping them fast.

## Platform support

| OS | Status |
|----|--------|
| Ubuntu | Supported |
| macOS | Supported |
| Windows | Not supported |
