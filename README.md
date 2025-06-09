# github-workflows

Personal, Reusable, GitHub Workflows.

## Available Workflows

### Maven Build and Publish Library

**Location:** `.github/workflows/maven-build-publish-library.yml`

Builds, tests, and publishes Kotlin/Java libraries using Maven. Distributes via GitHub Packages.

**Usage Example:**

```yaml
name: Build and Publish

on:
  push:
    branches: [ main ]
  pull_request:
  workflow_dispatch:

permissions:
  contents: read
  packages: write

jobs:
  build-and-publish:
    uses: tajacks/github-workflows/.github/workflows/maven-build-publish-library.yml
    with:
      java-version: '21'  # Optional, defaults to '21'
      publish-snapshots: false  # Optional, defaults to false
```

**No additional Maven configuration required** - the workflow handles deployment configuration automatically.

**Publishing Behavior:**
- **Release versions** (non-SNAPSHOT): Always published to GitHub Packages
- **SNAPSHOT versions**: Only published when `publish-snapshots: true` is set
  - The intention of this behaviour is to prevent accidental SNAPSHOT uploads during development while allowing 
    controlled SNAPSHOT releases for testing

**Required Setup:**

1. **Add permissions to your calling workflow** (as shown in the usage example above):
   ```yaml
   permissions:
     contents: read
     packages: write
   ```

2. **Configure repository settings** for GitHub Packages:
   - Go to your repository **Settings** → **Actions** → **General**
   - Under **Workflow permissions**, select **Read and write permissions**

Both the workflow permissions and repository settings are required for the workflow to publish packages successfully.

### Maven Release Library

**Location:** `.github/workflows/maven-release-library.yml`

Creates releases through a PR-based workflow that also leverages the build and publish workflow.

**Usage:**

This workflow is triggered manually from the GitHub Actions tab:

1. Go to **Actions** → **Maven Release Library** → **Run workflow**
2. Select the release type (patch/minor/major)
3. Optionally specify Java version (defaults to 21)

**Release Process:**

1. **Workflow creates release PR:**
   - Bumps version from SNAPSHOT to release
   - Creates Git tag on release commit
   - Bumps version to next SNAPSHOT
   - Creates PR with both commits

2. **Review and merge:**
   - Build workflow runs on the PR
   - Developer reviews and merges the PR

3. **Automatic publishing:**
   - Build workflow detects release version on main branch
   - Publishes release to GitHub Packages
   - Creates GitHub Release automatically

**Required Setup:**

The following permissions are required for this workflow:
```yaml
permissions:
  contents: write    # Required for creating releases and tags
  pull-requests: write  # Required for creating PRs
```
