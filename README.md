# Reusable GitHub Actions Workflows

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](./LICENSE)

This repository provides **reusable GitHub Actions workflows** (via `workflow_call`) to standardize CI/CD across multiple projects.

## Available Workflows

### CLI CI

- Workflow file: `.github/workflows/cli-ci.yml`
- Use case: Node/Bun/Deno CLI projects
- Package managers: `npm`, `pnpm`, `yarn`, `bun`, `deno`
- Default behavior: skips CI steps for release commits (`chore(release): vX.Y.Z` or `chore: release vX.Y.Z`)

### CLI Release (Tag) (Recommended)

- Workflow file: `.github/workflows/cli-release-tag.yml`
- Use case: publish a CLI tool on tag push (`vX.Y.Z`)
- Default behavior:
  - Validates `package.json#version` matches the tag version (non-`deno`)
  - Publishes to npm (`npm|pnpm|yarn|bun`) and creates a GitHub Release via `changelogithub`
  - Avoids double-build by skipping explicit build when `prepublishOnly`/`prepack` exists

### CLI Release (Commit) (Legacy)

- Workflow file: `.github/workflows/cli-release.yml`
- Use case: publish a CLI tool on release commits (`chore(release): vX.Y.Z` or `chore: release vX.Y.Z`)
- Default behavior:
  - Validates `package.json#version` matches the commit version (non-`deno`)
  - Creates/pushes a git tag `vX.Y.Z`
  - Builds and publishes to npm (`npm|pnpm|yarn|bun`)
  - Creates a GitHub Release via `changelogithub`

## Quick Start

Pin to a tag (recommended) when calling these workflows, e.g. `@v1`.

### Example: CI

Create `.github/workflows/ci.yml` in your project:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: kingsword09/workflows/.github/workflows/cli-ci.yml@v1
    with:
      packageManager: bun
```

### Example: Release (npm publish with token)

Create `.github/workflows/release.yml` in your project:

```yaml
name: Release

on:
  push:
    tags: [v*.*.*]

jobs:
  release:
    permissions:
      contents: write
      id-token: write
    uses: kingsword09/workflows/.github/workflows/cli-release-tag.yml@v1
    with:
      packageManager: bun
      # Optional: enable GitHub Environment protections/secrets
      # environment: production
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

### Example: Release (npm trusted publishing / OIDC)

```yaml
name: Release

on:
  push:
    tags: [v*.*.*]

jobs:
  release:
    permissions:
      contents: write
      id-token: write
    uses: kingsword09/workflows/.github/workflows/cli-release-tag.yml@v1
    with:
      packageManager: bun
      trustedPublishing: true
      # Optional: enable GitHub Environment protections/secrets
      # environment: production
```

## Notes

- Tag release expects tag format: `vX.Y.Z` (you can override via `releaseTag`)
- Commit/CI release regex default: `^(chore\\(release\\):\\s*|chore:\\s*release\\s+)v?(\\d+\\.\\d+\\.\\d+)(\\s|$)`
- If your scripts/commands differ, override via `*Command` inputs (e.g. `buildCommand`, `testCommand`).
- Trusted publishing (OIDC):
  - Set `trustedPublishing: true` and do not pass `NPM_TOKEN`.
  - Your calling workflow must include `permissions: id-token: write`.
  - Configure the trusted publisher on npm to match your **calling workflow filename** (e.g. `release.yml` in your repo), not this repository's reusable workflow.
