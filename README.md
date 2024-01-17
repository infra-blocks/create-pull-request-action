# create-pull-request-action

This action simply calls the GitHub API to create a pull request with the provided parameters.
It also returns the response from the API as a stringified JSON output.

If the authentication token passed is the generated GITHUB_TOKEN, it should have the
pull-requests: write permissions. It should be noted also, that the GITHUB_TOKEN used when creating
pull requests [*won't trigger the majority of workflows*.](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow)

## Inputs

|     Name     | Required | Description                                                                                                                                                                                                                     |
|:------------:|:--------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     head     |   true   | The name of the branch where your changes are implemented. For cross-repository pull requests in the same network, namespace head with a user like this: username:branch.                                                       |
|     base     |   true   | The name of the branch you want the changes pulled into. This should be an existing branch on the current repository. You cannot submit a pull request to one repository that requests a merge to a base of another repository. |
|    title     |  false   | The pull request title.                                                                                                                                                                                                         |
|     body     |  false   | The pull request body.                                                                                                                                                                                                          |
| github-token |  false   | The GitHub token to authenticate with the API. Defaults to ${{ github.token }}                                                                                                                                                  |
|  repository  |  false   | The repository where the pull request will be opened. Defaults to ${{ github.repository }}.                                                                                                                                     |

## Outputs

|     Name     | Description                                                                                                                                          |
|:------------:|------------------------------------------------------------------------------------------------------------------------------------------------------|
| pull-request | The created pull request payload, as returned by the [api](https://docs.github.com/en/rest/pulls/pulls?apiVersion=2022-11-28#create-a-pull-request). |

## Permissions

If you're using the GITHUB_TOKEN, then it should have the following permissions:

|     Scope     | Level | Reason                        |
|:-------------:|:-----:|-------------------------------|
| pull-requests | write | To create a pull request esé. |

## Usage

```yaml
- uses: infrastructure-blocks/create-pull-request-action@v1
  with:
    head: feature/your-branch
    base: main
```

## Development

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
