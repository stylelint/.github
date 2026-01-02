# Changelog

## 0.5.1 - 2026-01-02

- Fixed: bump `actions/checkout` to `6.0.1`.
- Fixed: bump `actions/setup-node` to `6.1.0`.

## 0.5.0 - 2025-11-25

- Added: `published-url` input to `call-release-pr` workflow.

## 0.4.0 - 2025-11-24

- Removed: deprecated `lint` and `test` callable workflows.
- Added: support `vscode-stylelint` publishing for `call-release` workflow.
- Added: npm cache in `call-release` and `call-release-pr` workflows.
- Fixed: npm package URL to include a version during `call-release` workflow.

## 0.3.1 - 2025-10-03

- Fixed: incorrect `new-version`/`new-version-command` input references in `call-release-pr` workflow.

## 0.3.0 - 2025-10-01

- Added: `new-version-command` input to `call-release-pr` workflow.
- Added: `new-version`/`pr-branch`/`pr-url` outputs to `call-release-pr` workflow.

## 0.2.1 - 2025-10-01

- Fixed: release PR body in `call-release-pr` workflow to help understand instructions.

## 0.2.0 - 2025-10-01

- Added: `node-version` input to `call-release` and `call-release-pr` workflows.

## 0.1.1 - 2025-10-01

- Fixed: `call-release` workflow to leave tag creation to `stylelint/changelog-to-github-release-action`.

## 0.1.0 - 2025-09-29

- First release.
