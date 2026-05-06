---
name: setting-up
description: Project setup files including .gitignore and GitHub Actions workflows. Use when initializing new projects or adding CI config.
---

# Project Setup

## .gitignore

Create when `.gitignore` is not present:

```
node_modules
dist

*.log
.DS_Store
.eslintcache
```

## GitHub Actions

Add these workflows when setting up a new project. Skip if workflows already exist. All use [sxzz/workflows](https://github.com/sxzz/workflows) reusable workflows.

### Autofix Workflow

**`.github/workflows/autofix.yml`** - Auto-fix linting on PRs:

```yaml
name: autofix.ci

on: [pull_request]

jobs:
  autofix:
    uses: sxzz/workflows/.github/workflows/autofix.yml@main
    permissions:
      contents: read
```

### Unit Test Workflow

**`.github/workflows/unit-test.yml`** - Run tests on push/PR:

```yaml
name: Unit Test

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

permissions: {}

jobs:
  unit-test:
    uses: sxzz/workflows/.github/workflows/unit-test.yml@main
```

### Release Workflow

**`.github/workflows/release.yml`** - Publish on tag (library projects only):

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    uses: sxzz/workflows/.github/workflows/release.yml@main
    with:
      publish: true
    permissions:
      contents: write
      id-token: write
```
