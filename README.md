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
      - uses: fragoi/bash-actions@main
      - name: Create release
        run: |
          source bash-functions
          DEB_PACKAGE=example-workflow
          createRelease

  merge-to-dev:
    needs: [release]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: fragoi/bash-actions@main
      - name: Update dev branch
        run: |
          source bash-functions
          updateDev main dev

```
