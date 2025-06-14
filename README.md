# github-workflows

Personal, Reusable, GitHub Workflows.

## Available Workflows

### Maven Build and Publish Library

**Location:** `.github/workflows/mvnw-build-publish-library.yml`

Builds, tests, and publishes Kotlin/Java libraries using a provided Maven wrapper. Conditionally publishes to GitHub 
Packages.

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
    uses: tajacks/github-workflows/.github/workflows/mvnw-build-publish-library.yml
    with:
      java-version: '21'  # Optional, defaults to '21'
      publish-snapshots: false  # Optional, defaults to false
```

**No additional Maven configuration required** - the workflow handles deployment configuration automatically.

**Publishing Behavior:**
- **Release versions** (non-SNAPSHOT): Always published to GitHub Packages
- **SNAPSHOT versions**: Only published when `publish-snapshots: true` is set
- This prevents accidental SNAPSHOT uploads during development while allowing controlled SNAPSHOT releases for testing

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
