# Validate version GitHub action

checks if version tag exists

# Example

```yaml
---
name: Release

on:
  push:
    branches:
      - "*"

jobs:
  validate_new_version:
    name: Validate new version
    runs-on: ubuntu-latest
    outputs:
      planned_version: ${{ steps.validate_new_version.outputs.planned_version }}
      tag_hash: ${{ steps.validate_new_version.outputs.tag_hash }}
      can_create: ${{ steps.validate_new_version.outputs.can_create }}
      tag_exists: ${{ steps.validate_new_version.outputs.tag_exists }}
      branch_name: ${{ steps.validate_new_version.outputs.branch_name }}

    steps:

      - name: Check out code
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Use latest released action
        id: validate_new_version
        uses: reinvented-stuff/validate-version-action@master
        with:
          version_filename: ".version"
          github_token: "${{secrets.GITHUB_TOKEN}}"
          fail_on_error: true
...

```