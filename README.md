# required-workflows

Source of the org-required workflows for Declaratus. The org rulesets that
require them, and the policy behind them, live in
[`Declaratus/governance`](https://github.com/Declaratus/governance)
(spec 05, `rulesets/branch/tier-*-workflows.json`).

This repo is public on purpose. GitHub runs a required workflow only on
repositories at the same visibility as its source repository or lower, so a
workflow required on public repositories must come from a public repository.

## Workflows

| File | Status check | What it does |
|---|---|---|
| [`baseline-lint.yml`](.github/workflows/baseline-lint.yml) | `org/baseline-lint` | Detects the stack by file presence and runs pinned Ruff, ESLint and Prettier (when configured), and golangci-lint. |
| [`security-baseline.yml`](.github/workflows/security-baseline.yml) | `org/security-baseline` | Dependency review when a PR touches a manifest or lockfile, and `pip-audit` for Python. No CodeQL: GitHub refuses it inside a required workflow. |

The `tier-*-workflows` rulesets pin both files at `ref: main`, so a merge
here changes the checks on every targeted repo's next PR. Treat every
change as an org-wide rollout: code-owner review is required, and a
workflow that starts failing across the org is rolled back per the
governance repo's `RUNBOOKS/ruleset-rollback.md`.

To add a required workflow, add it here first (it needs a `pull_request`
trigger, or the ruleset API returns 422), then add its path to the
`tier-*-workflows` rulesets in `Declaratus/governance`.
