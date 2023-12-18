# create-pull-request-action

This action simply calls the GitHub API to create a pull request with the provided parameters.
It also returns the response from the API as a stringified JSON output.

If the authentication token passed is the generated GITHUB_TOKEN, it should have the
pull-requests: write permissions. It should be noted also, that the GITHUB_TOKEN used when creating
pull requests [*won't trigger the majority of workflows*.](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow)

## Usage

```yaml
name: Create the pull request yo

on: [push]

jobs:
  some-work:
    steps:
      - name: Create Pull Request
        id: create-pull-request
        uses: infrastructure-blocks/create-pull-request-action@v1
        with:
          head: ${{ steps.merge-template-branch.outputs.branch }}
          base: master
          title: Update from template - ${{ steps.merge-template.branch.outputs.date }}
          github-token: ${{ secrets.github-pat }}
      - name: Report PR number
        run: |
          echo "Success, created new PR: ${{ fromJson(steps.create-pull-request.outputs.pull-request).number }}"
```

## Development

This project is written in Typescript and leverages `nvm` to manage its version. It also uses Git hooks
to automatically build and commit compiled code. This last part emerges from the fact that GitHub actions
run Javascript (and not typescript) and that all the node_modules/ are expected to be provided in the Git
repository of the action.

Having a Git hook to compile automatically helps in diminishing the chances that a developer forgets to
provide the compiled sources in a change request.

### Setup

Once `nvm` is installed, simply run the following:

```
nvm install
npm install
``` 

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-action](https://github.com/infrastructure-blocks/git-tag-semver-from-label-action).
