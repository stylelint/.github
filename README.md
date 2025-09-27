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
```
