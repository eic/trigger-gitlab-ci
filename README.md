# GitHub Action: eic/trigger-gitlab-ci
![test](https://github.com/eic/trigger-gitlab-ci/workflows/test/badge.svg)

This GitHub Action triggers a GitLab CI pipeline through webhooks.

## Instructions

### Example

You can use this GitHub Action in a workflow in your own repository with `uses: eic/trigger-gitlab-ci@v1`.

A minimal job example for GitHub-hosted runners of type `ubuntu-latest`:
```yaml
jobs:
  run-eic-shell:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: eic/trigger-gitlab-ci@v1
      with:
        url: https://gitlab.com
        token: ${{ secrets.TOKEN }}
        project_id: 37728736
        variables:
          VAR1: "value1"
```

In this case, you will need to upload the GitLab CI pipeline trigger token to the GitHub repository or organization secrets.
