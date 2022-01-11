# Set Up Bazel GitHub Action

GitHub Action to configure Bazel cache and settings for a specified configuration (e.g. ci). This
action assumes that a `.bazelrc` file of the Bazel workspace and it contains a directive to load
`local.bazelrc`.

```conf
# Try to import a local.rc file; typically, written by CI
try-import %workspace%/local.bazelrc
```

## Quickstart

```yaml
name: CI for PR Merge

on:
  pull_request:
    branches: [ main ]

jobs:
  ubuntu_ci:
    runs-on: ubuntu-20.04
    steps:
    - uses: actions/checkout@v2

    - name: Configure Bazel
      uses: ./
      with:
        repo_name: gha_set_up_bazel_ci
        bazel_repo_cache_dir: ~/.cache/custom_bazel_repo
        bazel_disk_cache_dir: ~/.cache/custom_bazel_disk
        workspace_dir: ci
    
    - name: Test the Workspace
      shell: bash
      run: |
        cd ci
        bazel test //...
```
