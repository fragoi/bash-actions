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
      - uses: fragoi/bash-actions@main
      - name: Update dev branch
        run: |
          updateDev main

```
