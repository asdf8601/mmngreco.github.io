---
title: "Some useful github actions"
date: 2024-11-18T08:39:51+01:00
draft: false
---



## List of Github action

- https://github.com/googleapis/release-please
- https://github.com/ncipollo/release-action
- https://github.com/PaulHatch/semantic-version
- https://github.com/mathieudutour/github-tag-action
- https://github.com/slackapi/slack-github-action
- https://github.com/mshick/add-pr-comment
- https://github.com/xt0rted/slash-command-action


### Automatic Release

```

name: Publish release

on:
  push:
    branches:
    - "main"

jobs:
    prepare-github-release:
        name: GitHub Release
        runs-on: ubuntu-latest
        outputs:
            version_tag: ${{ steps.calculate_tag_version.outputs.version_tag }}
        steps:

        - name: Checkout repository
          uses: actions/checkout@v4
          with:
            fetch-depth: 0

        - name: Manage semantic versioning
          uses: paulhatch/semantic-version@v5.4.0
          id: calculate_tag_version
          with:
            tag_prefix: ""
            major_pattern: "[MAJOR]"
            minor_pattern: "[MINOR]"
            version_format: "${major}.${minor}.${patch}"
            bump_each_commit: false
            search_commit_body: true
            debug: true

        - name: Bump version and push tag
          id: tag_version
          uses: mathieudutour/github-tag-action@v6.1
          with:
            github_token: ${{ secrets.GH_TOKEN }}
            custom_tag: ${{ steps.calculate_tag_version.outputs.version_tag }}
            tag_prefix: ""

        - name: Create a GitHub release
          uses: ncipollo/release-action@v1.14.0
          with:
            tag: ${{ steps.calculate_tag_version.outputs.version_tag }}
            name: Release ${{ steps.calculate_tag_version.outputs.version_tag }}
            body: ${{ steps.tag_version.outputs.changelog }}

```

