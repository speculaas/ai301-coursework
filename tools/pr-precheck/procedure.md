# Procedure: how this tool grades a PR package

## Read order

1. Decide the mode. Eval mode: the bundle is the whole world; do not
   fetch the live issue even though the `source:` line names it. Live
   mode: read `scope.md` first and stop if its `Repo:` line is the
   placeholder or the issue is outside the scoped repo; then read
   `voice-guide.md`.
2. Read the package in this order, writing short notes from each part
   before moving on:
   1. **Repo facts** (live: `.github/PULL_REQUEST_TEMPLATE.md`,
      `docs/CONTRIBUTING.md`, `scope.md` house rules): copy, word for
      word, the PR template's sections and checklist, any required
      artifact (changelog/whatsnew entry, `closes #N`, tests), the
      repo's test/check commands, and the AI policy with whether it
      applies to PRs, comments only, or all use.
   2. **Issue** and **Thread highlights** (live: the issue page): the
      reported behavior in one line; every OWNER / MEMBER /
      COLLABORATOR line that names, asks for, or rejects an approach.
   3. **Plan context** (live: `plan.md`): the repro's numbered steps
      and expected/actual; then the plan's in-scope list, Files list,
      Not-in-scope line, each Approach item, each Test plan item (each
      named repro, failure mode, test, or fixture), and every deviation
      or deferral note. This is the yardstick list.
   4. **Diff** (live: `git diff main...HEAD`) and **Commits** (live:
      `git log --oneline main..HEAD`): list every `+++ b/<path>`, and
      one line per hunk saying what it does.
   5. **Test evidence** (live: `test_evidence.md`): list each command
      or step and the output printed under it.
   6. **Title** and **Description** (live: `pr_draft.md`, first line
      is the title): list each factual claim as its own line, marking
      negative claims ("no X", "nothing beyond the plan").
3. Why this order: the plan is read before the diff, and the diff
   before the description, so every side-by-side has its yardstick in
   my notes before I meet the claim. Reading the description first is
   how calib-03 slides through: its "no changes beyond the plan" sounds
   true until the file list is next to the plan's Files line.

## Evidence gathering

Use `references/evidence-guide.md` for where each family lives. Build
these pairings before grading anything:

1. **diff-within-plan**: two columns. Left: each changed file and hunk
   from Read order 2.4. Right: the plan item or plan deviation note that names it, or "none"
   (a description-only mention is noted but still "none"). A "none" next to a hunk the fix did not need is
   the deciding line.
2. **plan-delivered**: two columns. Left: each Approach and Test plan
   item from 2.3. Right: the file/hunk that delivers it, or the
   deviation/description sentence that defers it, or "missing".
3. **description-matches-diff**: for each claim from 2.6, the diff
   line that makes it true, or the line (or absence) that makes it
   false. For a negative claim, search the diff for the thing denied.
4. **repro-rerun-observable**: for each Test plan item and repro step,
   the evidence entry that re-runs it and the output line that shows
   the expected-after. Write "not re-run", "no output", "note only",
   or "different path" when that is what you find.
5. **repo-checks-run**: list the repo's check commands from 2.1, and
   next to each the evidence line that names its result (eval) or the
   pasted output (live), or "absent". Note any ticked Testing box with
   nothing behind it.
6. **no-debris**: scan only added and removed lines for debug prints,
   commented-out code, dead functions, new TODO/FIXME, import or
   whitespace churn, unrelated config/lockfile edits, and planning
   files. Quote each hit with its path.
7. **template-asks-met**: for each template section, checklist item, or
   required artifact from 2.1, the Description text or diff file that
   meets it, or "missing"/"placeholder only". Live: also check the
   branch name matches `type/<issue>-slug`.
8. **ai-disclosure**: quote the AI policy from 2.1 and decide if it
   requires disclosure in a PR. If yes (or live mode), quote the
   Description's disclosure sentence, or write "none".
9. **Preferred checks**: maintainer-direction lines from 2.2 with
   follow/ignore; any new or updated test file; commit messages against
   the stated convention.
10. Quote only from the package (eval) or the live sources named above.
    Never fill a gap with what the real project probably has.

## Check execution

1. Run checks in rubric table order: diff-within-plan, plan-delivered,
   description-matches-diff, repro-rerun-observable, repo-checks-run,
   no-debris, template-asks-met, ai-disclosure, then the preferred
   checks. Grade every check even after one fails.
2. Apply only that check's pass condition to its gathered pairing. Go
   back to the package only for a section the pairing is missing.
3. Grade:
   - pass: the pairing meets the condition.
   - fail: name the line that breaks it (the extra hunk, the missing
     item, the false claim, the note standing in for output).
   - unclear: the section the check needs is absent. Name it.
4. Before failing plan-delivered or description-matches-diff for a
   missing planned item, check the plan's deviation/deferral notes and
   the description for an honest disclosure of that exact item; a
   disclosed deferral passes. Before failing diff-within-plan for added
   work, look for a plan deviation note naming it and why the fix
   needed it; a description-only disclosure does not expand the plan,
   and an unrelated cleanup fails even when disclosed.
5. Grade the change, not the write-up: no credit for a long, polished
   description and no penalty for a terse one (calib-01 is ready).
6. If two readings of a pass condition both seem fair, grade the
   stricter one for required checks and say which line forced it.

## Verdict assembly

1. Apply the rubric's verdict rule: any required fail or unclear means
   reject; otherwise accept. Preferred grades never flip the verdict.
2. Deciding check: for a reject, the first failing required check in
   rubric table order; for an accept, diff-within-plan. Quote its
   evidence line next to the verdict in the summary.
3. Live mode: after the per-check lines, list any voice-guide rule the
   title or description breaks, quoting rule and text. This never
   changes the verdict.
4. End with the fenced JSON block from SKILL.md: every check in table
   order with pass, fail, or unclear and its one-line evidence, then
   the verdict. Nothing after it.
