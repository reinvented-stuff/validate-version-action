# Validate version GitHub action

checks if version tag exists

# Usage


## Inputs

* `version_filename` (default: '.version') - Path to the file with application version
* `github_token` - Pass the secrets.GITHUB_TOKEN variable
* `fail_on_error` (default: 'false') - Fail the pipeline on version validation error

## Outputs

* `planned_version` - Version from version file
* `version_file_exists` - False if version file doesn't exist
* `tag_hash` - Commit hash if version tag exists
* `can_create` - True if version tag can be created
* `tag_exists` - True if version tag already exists
* `branch_name` - Working branch name

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
      version_file_exists: ${{ steps.validate_new_version.outputs.version_file_exists }}
      tag_hash: ${{ steps.validate_new_version.outputs.tag_hash }}
      can_create: ${{ steps.validate_new_version.outputs.can_create }}
      tag_exists: ${{ steps.validate_new_version.outputs.tag_exists }}
      branch_name: ${{ steps.validate_new_version.outputs.branch_name }}

    steps:

      - name: Check out code
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Validate new version
        id: validate_new_version
        uses: reinvented-stuff/validate-version-action@1.1.3
        with:
          version_filename: ".version"
          github_token: "${{secrets.GITHUB_TOKEN}}"
          fail_on_error: true
...

```