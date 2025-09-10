# github-workflows

Personal, Reusable, GitHub Workflows.

## Available Workflows

### Elixir Build & Test

**Location:** `.github/workflows/elixir-build-test.yml`

Builds and tests Elixir projects using Mix. Performs dependency installation, code formatting validation, compilation, and test execution.

**Usage Example:**

```yaml
name: Build and Test

on:
  push:
    branches: [ main ]
  pull_request:
  workflow_dispatch:

permissions:
  contents: read

jobs:
  build-and-test:
    uses: tajacks/github-workflows/.github/workflows/elixir-build-test.yml@main
```

**With custom versions:**

```yaml
jobs:
  build-and-test:
    uses: tajacks/github-workflows/.github/workflows/elixir-build-test.yml@main
    with:
      elixir-version: '1.17.0'
      otp-version: '26'
```

**What it does:**
- Validates that `mix.exs` exists in the repository
- Sets up Elixir (default: 1.18.4) and OTP (default: 27) with configurable versions
- Caches Mix dependencies for faster builds
- Installs project dependencies with `mix deps.get`
- Checks code formatting with `mix format --check-formatted`
- Compiles the project with warnings as errors
- Runs the test suite with `mix test`
- Provides a comprehensive build summary

**Configuration Options:**
- `elixir-version`: Elixir version to use (default: '1.18.4')
- `otp-version`: OTP version to use (default: '27')
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
