# .github

Stylelint org-wide GitHub settings.

## Syncing labels

### 1. Sync `labels.json` to `stylelint/.github`

Apply [`labels.json`](labels.json) with [`gh`](https://cli.github.com/) and [`jq`](https://jqlang.github.io/jq/):

```shell
jq -r --arg 'repo' 'stylelint/.github' \
  '.[] | @sh "gh label create \(.name) --description \(.description) --color \(.color) --repo \($repo) --force"' \
  labels.json \
  | sh
```

Omit `| sh` to preview the commands. `--force` updates existing labels. Other labels are not changed or deleted.

### 2. Copy labels to other repositories

Copy the labels from `stylelint/.github` to every unarchived repository in the org:

```shell
gh repo list stylelint --no-archived --limit 100 --json 'nameWithOwner' --jq '.[].nameWithOwner | select(. != "stylelint/.github")' \
  | xargs -I '{}' gh label clone 'stylelint/.github' --repo '{}' --force
```

Insert `echo` before `gh` to preview the commands. Labels that exist only in a destination repository are not changed or deleted, so this is safe to re-run.

## Reusable workflows

The following [reusable workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) for GitHub Actions allow us to set up CI settings easily:

- [`lint`](.github/workflows/call-lint.yml)
- [`test`](.github/workflows/call-test.yml)
- [`release-pr`](.github/workflows/call-release-pr.yml)
- [`release`](.github/workflows/call-release.yml)
- [`dependency-review`](.github/workflows/call-dependency-review.yml)

Usage:

```yaml
jobs:
  lint:
    uses: stylelint/.github/.github/workflows/call-lint.yml@d8a0a5b9734a79a67803d47b9aa93e31252a0459 # 0.1.0
    # Specify values different from the defaults.
    # with:
    #   node-version: '18'
    #   node-version-file: .node-version
    #   os: macos-latest

  test:
    uses: stylelint/.github/.github/workflows/call-test.yml@d8a0a5b9734a79a67803d47b9aa93e31252a0459 # 0.1.0
    # Specify values different from the defaults. Need to pass a JSON string.
    # with:
    #   node-version: '["16", "18"]'
    #   node-version-file: .node-version
    #   os: '["ubuntu-latest", "windows-latest"]'
    #   exclude: '[{"node-version": "16", "os": "windows-latest"}]'
    #   test-options: '--foo --bar'

  release-pr:
    uses: stylelint/.github/.github/workflows/call-release-pr.yml@d8a0a5b9734a79a67803d47b9aa93e31252a0459 # 0.1.0
    with:
      new-version: ${{ github.event.inputs.new-version }}
      # Specify values different from the defaults.
      # node-version: '18'
      # node-version-file: .node-version

  release:
    uses: stylelint/.github/.github/workflows/call-release.yml@d8a0a5b9734a79a67803d47b9aa93e31252a0459 # 0.1.0
    # Specify values different from the defaults.
    # with:
    #   node-version: '18'
    #   node-version-file: .node-version
    #   publish: false

  dependency-review:
    if: github.event_name == 'pull_request'
    uses: stylelint/.github/.github/workflows/call-dependency-review.yml@d8a0a5b9734a79a67803d47b9aa93e31252a0459 # 0.1.0
```

### Release workflow

To perform releasing in a repository with the shareable workflows, take the following steps:

Prerequisites:

- Must follow the release branch name pattern `release/{something}`, e.g., `release/1.0.0`.
- Must import the branch and tag [rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets).
- Must set up the `npm` [environment](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments).

Then follow the [release steps from the `stylelint/stylelint` repo](https://github.com/stylelint/stylelint/blob/main/docs/maintainer-guide/releases.md). Only owners can review and merge the PR, and only owners with publishing access can approve the pending `release` job.
