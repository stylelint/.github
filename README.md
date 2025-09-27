# .github

Stylelint org-wide GitHub settings.

## Syncing labels

You can sync our common labels to a new repository using [github-label-sync](https://github.com/Financial-Times/github-label-sync):

1. Clone this repo.
2. Run `npx github-label-sync --access-token xxxx stylelint/<repo-name>`.

## Reusable workflows

The following [reusable workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) for GitHub Actions allow us to set up CI settings easily:

- [`lint`](.github/workflows/call-lint.yml)
- [`test`](.github/workflows/call-test.yml)
- [`release-pr`](.github/workflows/call-release-pr.yml)
- [`release`](.github/workflows/call-release.yml)

Usage:

```yaml
jobs:
  lint:
    uses: stylelint/.github/.github/workflows/call-lint.yml@3ba9ed961fcf158d128b77ad87371c25f8784a45 # main
    # Specify values different from the defaults.
    # with:
    #   node-version: 18
    #   os: macos-latest

  test:
    uses: stylelint/.github/.github/workflows/call-test.yml@3ba9ed961fcf158d128b77ad87371c25f8784a45 # main
    # Specify values different from the defaults. Need to pass a JSON string.
    # with:
    #   node-version: '["16", "18"]'
    #   os: '["ubuntu-latest", "windows-latest"]'
    #   exclude: '[{"node-version": "16", "os": "windows-latest"}]'
    #   test-options: '--foo --bar'

  release-pr:
    uses: stylelint/.github/.github/workflows/call-release-pr.yml@3ba9ed961fcf158d128b77ad87371c25f8784a45 # main
    with:
      new-version: ${{ github.event.inputs.new-version }}

  release:
    uses: stylelint/.github/.github/workflows/call-release.yml@3ba9ed961fcf158d128b77ad87371c25f8784a45 # main
    # Specify values different from the defaults.
    # with:
    #   publish: false
```

### Release workflow

To perform releasing in a repository with the shareable workflows, take the following steps:

Prerequisites:

- Must import the branch and tag [rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets).
- Must set up the `npm` [environment](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments).

Steps:

1. A releaser triggers the `release-pr` workflow.
2. A release PR is automatically created.
3. Owners review and merge the PR.
4. The `release` workflow automatically starts.
5. Owners with publishing access approve the pending `release` job.
6. A package is automatically published to npm.
7. A draft GitHub release is automatically created.
8. A releaser reviews and publishes the draft release.
