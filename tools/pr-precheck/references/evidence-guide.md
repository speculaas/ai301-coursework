# Evidence guide: where evidence lives in a PR package

Eval bundles have: **Repo facts**, **Issue**, **Thread highlights**,
**Plan context** (repro evidence + plan with Scope, Not in scope, `Files:`,
Test plan), and **Candidate PR** (Title, Description, Commits, Diff, Test
evidence). Plan wording varies; look for the content, not the heading.

Live mode (my own PR): plan = `plan.md` (+ `## Deviations`); diff =
`git diff main...HEAD` and `git log --oneline main..HEAD`; title and
description = `pr_draft.md`; test evidence = `test_evidence.md`; repo asks =
`.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, `scope.md` house
rules; thread = the issue page.

## Plan fidelity (harness category: silent-drift)

- **Where:** plan `Files:` line, Scope / "one bounded change", Not in scope;
  live also `## Deviations`. Against it: every `+++ b/<path>` in the diff, and
  the description's fidelity sentences ("exactly", "no changes beyond it",
  "no options added", "tests for X, Y, Z").
- **Good looks like:** every changed file is named by the plan (file or its
  folder; "a changelog entry" names the changelog) or by a deviation note;
  every Not-in-scope item is untouched; each thing the description says was
  added or not added matches the diff. Drift either way: an extra file (calib-03
  `property.js`) or a promised part missing with no note (calib-03's control and
  nested fixtures). A description claiming more than the diff is drift.
- My PR: plan names `.env.example` only; `git diff main...HEAD --stat` must list only it.

## Test evidence (harness category: not-tested)

- **Where:** the Test evidence section (live `test_evidence.md`), read item by
  item against the plan's Test plan and the repro's steps; the repo's own
  checks (the template's Testing list: `make test-unit`, `make test-integration`,
  `make lint`, `make typecheck` on Path Review).
- **Good looks like:** for each Test plan item, a command followed by its real
  printed output, before and after, showing the stated expected-after (an
  output line, a count, an exit code). Each repo check is shown with its
  printed result, failures included with a reason. A `# note` after a command,
  or "the suite passes", is not output. A Test plan item with no re-run (calib-04
  repro 2) is missing evidence.
- My PR: grep/cp steps with counts `0` before, `1` after; four make targets
  pasted, `make test-integration`'s "no tests ran" as is.
- **Repo suite in eval bundles:** a sentence naming the suite command and its
  outcome ("`go test ./...` passes (4108 tests)") is how the bundles report
  the repo's own checks, and it counts for `repo-checks-run`. It never counts
  as the repro re-run: that needs printed output. An unnamed "tests pass" or
  "verified on my machine" counts for neither.

## Diff quality (harness category: unreviewable)

- **Where:** the unified diff and the commit list.
- **Good looks like:** the fix is visible in a few hunks, each hunk serves the
  plan, commit messages describe it. Debris tells: debug prints, commented-out
  code, TODOs, whitespace/formatting churn, renames or refactors the issue
  doesn't need (calib-03's rewritten `printPropertyKey`), lockfile or unrelated
  config edits, and planning files (`plan.md`, `pr_draft.md`) committed.

## Standards and comms (harness category: standards-wall)

- **Where:** Repo facts "pull requests" (template asks) and "contribution
  policy" (incl. AI policy); Thread highlights for maintainer direction. Live:
  the PR template, CONTRIBUTING, `scope.md` house rules.
- **Good looks like:** every template section has real content (not the
  comment placeholder), the checklist ticks only what the evidence shows, the
  asked-for artifacts exist (changelog entry, tests, issue link), AI use is
  disclosed where the policy asks (eval: only when the repo's stated policy
  requires disclosing AI use in PRs or "all AI usage"; a policy that only asks
  for human-written comments, or no AI policy, needs none; live: always, in
  Notes for Reviewers), and explicit maintainer direction
  in the thread is followed or answered. Path Review: Summary, `Closes #N`,
  Changes, Testing, Screenshots/Demo, Notes for Reviewers (AI disclosure goes
  here), branch `type/<num>-slug` from my fork.
