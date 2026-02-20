# Bash Actions

Bash utilities to use in a workflow.

Example usage:

```yaml
name: Example workflow

...

jobs:

  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: fragoi/bash-actions@v2
      - name: Create release
        env:
          ## this is needed to use the `gh` CLI
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          createRelease

  merge-to-dev:
    needs: [release]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          ref: dev
      - uses: fragoi/bash-actions@v2
      - name: Update dev branch
        run: |
          updateDev main

```

## Notes

### Changing version scheme

When changing version scheme, for example adding a manually changed prefix to the version, or when changing the prefix of the version tag, it is better to manually tag the latest version tag on each released branch with the new scheme, so that it can be found as the previous tag of the current version.

**Example:**

Suppose we have the following history:

```
d3c87c8 dev: update readme
45231e4 (tag: v2.0.0) Version: 2.0.0
f95f8f6 feat: change version index logic
```

And we want to change the version tag prefix to be `v_` instead of `v`.

We should tag the commit `45231e4` as `v_2.0.0` and push the tag:

```
d3c87c8 dev: update readme
45231e4 (tag: v_2.0.0, tag: v2.0.0) Version: 2.0.0
f95f8f6 feat: change version index logic
```

before changing the `TAG_PREFIX` environment variable to `v_`:

```yaml
name: Example workflow

env:
  TAG_PREFIX: v_

...

```
