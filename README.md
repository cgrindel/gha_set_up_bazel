# Set Up Bazel GitHub Action

GitHub Action that wraps [bazel-contrib/setup-bazel](https://github.com/bazel-contrib/setup-bazel)
to configure Bazel caching, with two additional fixes:

1. **Removes `startup --output_base` from `~/.bazelrc`** -- `setup-bazel` sets a shared output base
   so cache paths are predictable. This causes integration tests that spawn their own Bazel server to
   block on the output-base lock.
2. **Enables `--config=ci`** -- Appends `common --config=ci` to `~/.bazelrc` so your `ci.bazelrc`
   settings (e.g., `--announce_rc`, `--color=no`) are applied automatically.

## Usage

```yaml
- uses: cgrindel/gha_set_up_bazel@v2
```

### Inputs

| Input              | Default   | Description                                             |
| ------------------ | --------- | ------------------------------------------------------- |
| `bazelisk_cache`   | `"true"`  | Cache Bazelisk downloads based on `.bazelversion`.      |
| `repository_cache` | `"true"`  | Cache repositories based on `MODULE.bazel`/`WORKSPACE`. |
| `disk_cache`       | `"false"` | Cache build outputs based on BUILD files.               |
| `enable_ci_config` | `"true"`  | Append `common --config=ci` to `~/.bazelrc`.            |

### Example with disk cache and remote cache

```yaml
# Using a remote cache -- disable disk cache (the default)
- uses: cgrindel/gha_set_up_bazel@v2

# Using disk cache instead of remote cache
- uses: cgrindel/gha_set_up_bazel@v2
  with:
    disk_cache: "true"
```
