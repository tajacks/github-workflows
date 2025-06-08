# github-workflows

Personal, Reusable, GitHub Workflows.

## Available Workflows

### Kotlin Build and Package Library

**Location:** `.github/workflows/kotlin-build-package-library.yml`

Builds, tests, and packages Kotlin/Java libraries using Maven for distribution to GitHub Packages.

**Usage Example:**

```yaml
name: Build and Package

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-package:
    uses: tajacks/github-workflows/.github/workflows/kotlin-build-package-library.yml
    with:
      java-version: '21'  # Optional, defaults to '21'
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**No additional Maven configuration required** - the workflow handles deployment configuration automatically.
