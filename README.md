<h1 align="center">github-action-release-maker</h1>


<div align="center">

<b>Github Action to create a release when triggered</b>

[![version](https://img.shields.io/github/v/release/kbrashears5/github-action-release-maker)](https://img.shields.io/github/v/release/kbrashears5/github-action-release-maker)
[![Build Status](https://dev.azure.com/kbrashears5/github/_apis/build/status/kbrashears5.github-action-release-maker?branchName=master)](https://dev.azure.com/kbrashears5/github/_build/latest?definitionId=30&branchName=master)

</div>


# Use Cases
Have releases be created automatically as part of your CI/CD, using your last commit message

# Setup
Create a new file called `/.github/workflows/release-maker.yml` that looks like so:
```yaml
name: Release Maker

on:
  repository_dispatch:
    types:
      - release

jobs:
  release_maker:
    runs-on: ubuntu-latest
    steps:
      - name: Release Maker
        uses: kbrashears5/github-action-release-maker@v1.0.0
        with:
          VERSION: ${{ github.event.client_payload.version }}
          TOKEN: ${{ secrets.ACTIONS }}
```
## Parameters
| Parameter | Required | Description |
| --- | --- | --- |
| VERSION | true | Version name to give the release |
| TOKEN | true | Personal Access Token with Repo scope |

## Invoking
### cURL
```bash
curl \
  -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -d '{"event_type":"release","client_payload":{"version":"v1.0.0"}}' \
  https://api.github.com/repos/{username}/{repo-name}/dispatches
```

### Azure DevOps Pipeline
See my yaml [templates](https://github.com/kbrashears5/yaml/blob/master/templates/create-github-release.yml) on how to use.

This template will use the build number as the version of the package.

You'll need to create a variable group called `github`, with a secret variable called `GITHUB_TOKEN`, with a token with repo:public_repo scope. See my yaml [example](https://github.com/kbrashears5/yaml/blob/master/examples/azure-pipelines-node.yml) on how to use.