# GitHub Action: eic/trigger-gitlab-ci
![test](https://github.com/eic/trigger-gitlab-ci/workflows/test/badge.svg)

This GitHub Action triggers a GitLab CI pipeline through webhooks.

## Instructions

### Example

You can use this GitHub Action in a workflow in your own repository with `uses: eic/trigger-gitlab-ci@v1`.

A minimal job example looks as follows:
```yaml
jobs:
  trigger-gitlab-ci:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: eic/trigger-gitlab-ci@v1
      with:
        project_id: 37728736
        token: ${{ secrets.TOKEN }}
```
You will need to upload the GitLab CI pipeline trigger token to the GitHub repository or organization secrets. This token can be created in the GitLab repository settings under CI/CD > Pipeline Triggers. 

### Inputs
This action requires the following inputs:
- `project_id`: GitLab project ID
- `token`: GitLab pipeline trigger token (GitLab Settings > CI/CD > Pipeline Triggers)

This action also accepts several optional inputs:
- `url`: GitLab server url (at the level under which the API starts), default: `https://gitlab.com`
- `ref_name`: GitLab project ref_name (branch or tag name), default: `main`
- `variables`: Additional variables in `VAR1=value VAR2=value` format to pass to the pipeline, default: ``

A more advanced example could look as follows, passing variables for call backs:
```yaml
jobs:
  trigger-gitlab-ci:
    runs-on: ubuntu-latest
    if: ${{ github.event_name == 'pull_request' }}
    steps:
    - uses: actions/checkout@v2
    - uses: eic/trigger-gitlab-ci@v1
      with:
        url: https://gitlab.cern.ch
        project_id: 6751
        token: ${{ secrets.TOKEN }}
        ref_name: master
        variables: |
          GITHUB_REPOSITORYURL: ${{ github.repositoryUrl }}
          GITHUB_SHA: ${{ github.sha }}
```

### Outputs
This actions provides the following outputs:
- `json`: GitLab webhook response (JSON)
- `web_url`: GitLab pipeline URL

These outputs can be used as follows to print the GitLab pipeline URL to :
```yaml
jobs:
  trigger-gitlab-ci:
    runs-on: ubuntu-latest
    steps:
    - uses: eic/trigger-gitlab-ci@v1
      with:
        project_id: 37728736
        token: ${{ secrets.TOKEN }}
    - uses: peter-evans/commit-comment@v2
      with:
        body: |
          [GitLab pipeline](${{ steps.trigger.outputs.web_url }})
```
