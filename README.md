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

      - name: Use latest released action
        id: validate_new_version
        uses: reinvented-stuff/validate-version-action@master
        with:
          version_filename: ".version"
          github_token: "${{secrets.GITHUB_TOKEN}}"

      - name: Fail if version already exists
        id: fail_on_duplicate_version
        if: steps.validate_new_version.outputs.can_create != 'true'
        run: exit 2

...

```