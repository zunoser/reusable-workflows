# Reusable GitHub Actions workflows

## GitHub Actions lint

Run `gh actions-lock --verify`, jactionlint, ghalint, and zizmor on the caller's
workflow files. The job requires `contents: read` and can run on pull requests
without repository secrets.

`gh actions-lock --verify` fails when `.github/workflows/actions.lock` is missing or
does not cover every workflow, including `schedule` and `workflow_dispatch` workflows
that the pull request does not run. Callers without a lockfile can skip it:

```yaml
    with:
      verify-actions-lock: false
```

```yaml
name: GitHub Actions lint

on: pull_request

permissions: {}

jobs:
  gha-lint:
    permissions:
      contents: read
    uses: zunoser/reusable-workflows/.github/workflows/gha-lint.yml@RELEASE_COMMIT_SHA # v1.1.0
```

Replace `RELEASE_COMMIT_SHA` with a full commit SHA from the
[releases](https://github.com/zunoser/reusable-workflows/releases).
Enable Dependabot's `github-actions` ecosystem in the caller to get PRs for new
releases of the shared workflow.

The shared job uses the dependency lockfile in this repository. When changing its
Action dependencies, run `gh actions-lock` and `gh actions-lock --verify` here.
Manage workflow lockfiles in each caller according to the
[gha-lint skill](https://github.com/yutakobayashidev/skills/blob/main/skills/gha-lint/SKILL.md).
