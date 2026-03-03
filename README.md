# plugin-release

GitHub Action for automatically bump FastAPI Best Architecture plugin version after a release.

When a plugin repository pushes a new tag, this Action automatically creates a pull request
to update the corresponding submodule in [fastapi-practices/plugins](https://github.com/fastapi-practices/plugins).

## Usage

Add the following workflow file to your plugin repository (e.g. `.github/workflows/release.yml`):

```yaml
name: Plugin Release

on:
  push:
    tags:
      - "v*"

jobs:
  update-submodule:
    runs-on: ubuntu-latest
    steps:
      - name: Update plugin submodule
        uses: fastapi-practices/plugin-release@v1
        with:
          push-to: your-username/plugins
```

## Inputs

| Input         | Required | Default                       | Description                                                                              |
| ------------- | -------- | ----------------------------- | ---------------------------------------------------------------------------------------- |
| `push-to`     | Yes      | —                             | The repository to push the update branch to (`owner/repo`), e.g. `your-username/plugins` |
| `plugin-name` | No       | Repository name of the caller | Plugin name, must match the submodule directory name in `fastapi-practices/plugins`      |

## How it works

1. Validates that the version in `plugin.toml` matches the pushed tag
2. Checks out the push-to repository with all submodules
3. Syncs with the upstream `fastapi-practices/plugins` repository
4. Validates that the specified plugin submodule exists
5. Creates a new branch in the push-to repository
6. Updates the submodule to the latest commit via `git submodule update --remote`
7. Commits and pushes the changes
8. Opens a pull request against the upstream plugins repository

If the submodule is already up to date, no PR is created.

## License

[MIT](./LICENSE)
