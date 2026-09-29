# Test cycle 2026-09-29

A `voting`-state cycle for `MaciejRucinskiVIMN/branch-cleanup-test-no-prod`, backed by real
refs and pull requests on that remote. Every `last_commit_sha` is the live tip of its branch
and every `pull_request` block matches an open PR, so the fixture can be cross-checked against
the API instead of only against the schema.

## Layout

    cycle.json                       cycle-level metadata
    branches/<branch>.json           one record per candidate, path mirrors the ref name

## Candidates (6)

| branch | reasons | PR | decision | source |
| --- | --- | --- | --- | --- |
| `feature/abandoned-login-refactor` | stale_branch | — | undecided | — |
| `bugfix/flaky-checkout-test` | stale_branch, stale_pr | #1 | keep | slack |
| `spike/graphql-gateway` | stale_pr | #2 | undecided | — |
| `chore/bump-eslint-config` | stale_branch | — | delete | manual |
| `feature/legacy-player-shim` | stale_branch, stale_pr | #3 draft | delete | slack |
| `renovate/lodash-4.x` | stale_branch | — | undecided | — |

`spike/graphql-gateway` is the interesting one: at 70 days it is inside
`branch_stale_after_days` (90) but past `pr_stale_after_days` (60), so it is a candidate on the
PR threshold alone and carries only `stale_pr`.

## Skipped

`archive/old-experiment` and `release/2026-q1` by exempt prefix, `main` and
`branch-cleanup-state` as protected, `feature/recently-active` for its active PR (#4).
All five exist on the remote.

## Not covered by this cycle

`decision_source: "timeout"` and the `archived` / `deleted` / `skipped` outcomes are only
reachable at finalization, so a `voting` cycle cannot exercise them. They need a second,
`finalized` cycle whose branches have already been renamed or removed.

## Known divergence

GitHub sets PR timestamps at creation time and they cannot be backdated, so the
`last_activity_at` values here are the head-commit dates that justify candidacy rather than
what the API reports for PRs #1–#3.
