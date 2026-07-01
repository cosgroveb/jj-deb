# jj-starship Debian packages

Private binary Debian packaging for
[dmmulroy/jj-starship](https://github.com/dmmulroy/jj-starship).

This repository builds `amd64` and `arm64` `.deb` files for:

- Debian 12 bookworm
- Debian 13 trixie
- Debian sid
- Ubuntu 24.04 noble

It intentionally does not maintain a Debian source package. Upstream currently
requires Rust 1.89 and Cargo dependencies that are a poor fit for oldstable and
stable archive-only builds, so the packaging path is binary-only and private.

## Build locally

```sh
scripts/build-package
```

Useful overrides:

```sh
SUITE=trixie UPSTREAM_REF=v0.7.1 PACKAGE_REVISION=1 scripts/build-package
```

The package is written to `dist/`.

## GitHub Actions

`.github/workflows/build-debs.yml` builds in Debian and Ubuntu containers for
bookworm, trixie, sid, and noble on native `amd64` and `arm64` GitHub-hosted
runners.

Manual runs accept:

- `upstream_ref`: jj-starship tag, branch, or commit to build
- `package_revision`: Debian package revision suffix
- `publish_release`: whether to attach packages to a GitHub Release
- `release_tag`: release tag for manual publishing

Tag pushes also build packages and publish all `.deb` files to the matching
GitHub Release.

## Package contents

The generated package installs:

- `/usr/bin/jj-starship`
- `/usr/share/doc/jj-starship/README.md`
- `/usr/share/doc/jj-starship/changelog.Debian.gz`
- `/usr/share/doc/jj-starship/copyright`
- `/usr/share/man/man1/jj-starship.1.gz`

Runtime dependency metadata is intentionally minimal. The binary is built inside
the target Debian suite container and declares the dynamic libraries observed in
local package verification: `libc6`, `libgcc-s1`, and `zlib1g`.
