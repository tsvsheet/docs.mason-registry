# Contributor documentation

How [`tsvsheet/mason-registry`](https://github.com/tsvsheet/mason-registry) works inside and how to verify a change. User-facing documentation lives in [`content/`](../content/).

## Layout

One YAML file per package under `packages/<name>/package.yaml`, in the [mason-org/mason-registry](https://github.com/mason-org/mason-registry) schema. The `registry` workflow compiles them into `registry.json.zip` + `checksums.txt` and publishes a release on every `main` push — mason resolves the **latest** release, and the workflow marks a non-head rerun `--latest=false` so stale content can never become what consumers fetch.

## Build and test

`make check` runs the gate:

- `make validate` — `scripts/validate` proves every package's shape (fail-closed: a field yq cannot read is a failure), that the pinned version is the upstream's latest release, that every templated asset filename exists on that release, and that every archive asset contains the binary its definition names. It rejects template tokens, targets, and archive formats it does not know — teach it first, then use them.
- `make build` — compiles `var/registry.json` (the packages directory is a prerequisite, so adding or removing a package rebuilds).

Bumping a package is a one-line `source.id` edit; the weekly scheduled run fails when a pin goes stale, which is the signal to bump.
