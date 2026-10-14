# Rubric: is this pull request ready to submit?

Every check names its evidence with the section names in
`references/evidence-guide.md`. In eval mode those are bundle sections
(Repo facts, Issue, Thread highlights, Plan context, Candidate PR:
Title, Description, Commits, Diff, Test evidence). In live mode they are
`plan.md` (with `## Deviations`), `git diff main...HEAD`,
`pr_draft.md`, `test_evidence.md`, the issue thread, and Path Review's
`.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`. Plan
headings vary; read whichever paragraph carries the scope, the not-in
line, the files, and the test plan.

"The plan" always means the plan plus its deviation notes. A plan-named item that is
deliberately deferred or dropped is covered when a deviation note in
the plan, or a plain sentence in the description, names it and says
why. Extra work the fix needed (an added file, hunk, option or
refactor) is covered only by a deviation note in the plan that names
it and says why; the description can report it but cannot authorize
it by itself. That is the honest-outcome rule: a disclosed shortfall,
or a needed deviation recorded in the plan, can be ready. It
does not cover an unrelated change: a drive-by cleanup the issue never
needed holds the PR even when the description owns up to it.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan | Every changed file and every hunk in the Diff (`+++ b/<path>` lines and the hunk contents), read against the plan's Scope, Files, Approach, and Not-in-scope lines and any deviation note. | Pass if every changed file is one the plan names (or sits in a folder or artifact the plan names, e.g. "a changelog entry" covers the changelog file) AND every hunk serves the planned change. Fail if any file or hunk does something the plan did not name and no deviation note in the plan authorizes it (a description-only mention does not): an extra file, a rename or refactor pass, a new option, flag, or setting, a rewrite of a neighbouring function. A correct core fix does not rescue an undisclosed extra, and an unrelated cleanup (something the fix did not need) fails even when disclosed. | required |
| plan-delivered | The plan's Approach and Test plan items (each named change, each named test or fixture, each named doc or changelog update), read against the Diff. | Pass if every change and every test the plan names appears in the Diff, OR the missing item is disclosed as deferred or dropped in a deviation note or the description. Fail if a plan-named item is absent and nothing discloses it (the diff does less than the plan, silently). | required |
| description-matches-diff | Each factual sentence in the Description and Title ("adds X", "updates docs", "no changes beyond the plan", "no new options", "implements the plan exactly", "tests for A, B, C"), read against the Diff. | Pass if every positive claim is visible in the Diff and every negative claim ("no X", "nothing else changed") is true of the Diff. Fail if the description claims something the Diff does not contain, or claims fidelity ("exactly as planned", "no functional changes outside...") that a diff-within-plan or plan-delivered fail contradicts. | required |
| repro-rerun-observable | The Test evidence section, item by item, read against the plan's Test plan and the repro's numbered steps (each named failure mode, each named repro). | Pass if every repro or failure mode the Test plan names is re-run on the changed code path, with real printed output (command output, error text, exit code, count, or a before/after table) showing the stated expected-after; the before may be reused from the repro. Output pasted with inline before/after labels (e.g. `-- before: { code = 1, ... }` / `-- after: { code = 0 }`) counts as printed output when it shows the actual values. A control or unchanged-behaviour check (a no-ampersand case, an existing flag still working) may be reported in one sentence; only the bug's own repro and named failure modes need pasted output. Fail if any named repro or failure mode is never re-run, if the evidence exercises a different or unchanged path (a control, a different request, a single-file case when the bug needs two), if a `# note` or prose sentence ("works now", "verified on my machine", "colors work") stands in for the output, or if a command is shown with no output under it. | required |
| repo-checks-run | The Test evidence and Description, read against the repo's stated checks (Repo facts PR template / contribution docs; live: the four `make` targets in Path Review's PR template). | Eval: pass if the evidence names the repo's own test command or suite and its outcome (e.g. "`go test ./...` passes (4108 tests)"), or honestly reports that a check could not run and why. Fail if no repo check is mentioned at all, or only an unnamed "tests pass" / "all good". Live: pass only if each of `make test-unit`, `make test-integration`, `make lint`, `make typecheck` appears with its pasted output (a failing or "no tests ran" result pasted with its reason passes); a ticked Testing box with no matching pasted output fails. | required |
| no-debris | The Diff hunks and the Commits list. | Pass if every added or removed line serves the change. Fail if the Diff adds any of: a debug print or log line, commented-out code (an old attempt or a disabled line), a dead function or `allow(dead_code)`/`noqa` to hide it, a new TODO/FIXME, import reordering or re-indent/whitespace churn in lines the fix does not touch, an unrelated config or lockfile edit, or a planning file (`plan.md`, `pr_draft.md`, `test_evidence.md`). Commit messages like "wip"/"misc cleanups while debugging" are supporting evidence, never the sole reason. A comment that explains the new code, and output a test itself needs (a print inside a test fixture), are not debris. | required |
| template-asks-met | Repo facts "pull requests" line (template sections, required checklist, required artifacts such as a whatsnew/CHANGELOG entry or "closes #N") read against the Description and Diff. Live: every section of `.github/PULL_REQUEST_TEMPLATE.md` read against `pr_draft.md`, plus the `scope.md` house rules (branch name `type/<issue>-slug` from my fork). | Pass if each section or checklist item the repo states is present with real content (not the template's placeholder comment, not left empty), and each artifact the template requires (issue link, changelog/whatsnew entry, test) is in the Description or Diff. Pass with no further template requirement only if Repo facts state no PR template and the contribution docs state no PR-specific asks (sections, checklist, issue link, tests, changelog/whatsnew). Fail if a stated required section, checklist, issue link, or required artifact is visibly missing. Ticking a box the evidence contradicts is also a fail. | required |
| ai-disclosure | Repo facts "contribution policy" (AI policy wording) read against the Description. Eval: treat every package as AI-assisted work. Live: the course always requires disclosure, in Notes for Reviewers. | Pass if no stated policy requires disclosure in PRs (no AI policy, or a policy that only asks for human-written comments or a human in the loop), OR the Description carries a disclosure that names the tool and the extent of help. Live: pass only if Notes for Reviewers says, in my own words, how AI was used on this change. Fail if the policy requires disclosing AI use (in PRs or "all AI usage") and the Description has no such statement. | required |
| maintainer-direction | Thread highlights lines from OWNER / MEMBER / COLLABORATOR that name an approach, reject one, or ask a question; read against the Description. | Description follows that direction or says why not. Pass if the thread has none. | preferred |
| regression-test | The Diff's test files. | The PR adds or updates an automated test that fails before and passes after. | preferred |
| commit-convention | The Commits list, read against the repo's stated commit convention. | Commit messages follow the stated convention (Path Review: Conventional Commits). | preferred |

## Verdict rule

accept (ready to submit) only if every required check is pass. Any
required check graded fail or unclear means reject (hold). unclear
counts as fail: if the package lacks the section a required check needs
(no Test evidence, no Description), that check is unclear and the
package holds. Preferred checks are reported in the summary but never
change the verdict, so a terse PR with no new test and loose commit
messages can still be ready. A planned item honestly disclosed as deferred or dropped (plan note or
description) is not a fail on plan-delivered or
description-matches-diff. Added work outside the plan needs a deviation
note in the plan; a description-only mention does not put it in scope.
