---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: is this ready to submit? A PR
package is a candidate pull request (its title, description, commit list,
diff, and test evidence) read against the plan it claims to implement and
the issue that plan belongs to. Grade exactly one package per run. Do not
review code style, suggest a better fix, or judge the plan itself; the plan
was accepted in week 3 and is the yardstick here.

## Inputs and modes

Decide the mode first. If you were given a package bundle (a markdown file
with Repo facts, Issue, Thread highlights, Plan context, and Candidate PR
sections), you are in eval mode. If you were asked to grade the student's own
draft PR, you are in live mode.

- **Live mode.** Read, from the top folder of the student's fork clone:
  - the plan: `plan.md`, including its `## Deviations` section;
  - the diff: run `git diff main...HEAD` (three dots) and `git log --oneline main..HEAD`; only committed changes count;
  - the title and description: `pr_draft.md` (first line is the title);
  - the test evidence: `test_evidence.md`;
  - the issue: the URL given, read with `gh issue view <url> --comments` or the web;
  - the repo's asks: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the clone.
  A house-chain student reads the house plan and house repro pack instead of
  their own plan and repro. Treat the student's change as AI-assisted work:
  the course always asks for an AI-use disclosure in the description. If any of the files above is missing, say which one
  and grade the checks that depend on it as fail; do not substitute other files.
- **Eval mode.** The bundle is the whole world. Take every fact from the bundle
  text; fetch nothing and read no other file. Always run every check and the
  full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. If its `Repo:` line still
holds the `<ORG>/<PATH-REVIEW-REPO>` placeholder, stop without grading and tell
the student to fill the `Repo:` line in `scope.md` with their section's Path
Review repo. If the issue URL is not in the scoped repo, refuse to grade and
say why. Apply the house rules it lists (own fork's branch, one PR per issue,
template always used) as part of the standards evidence. In eval mode, ignore
`scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and hold the PR title and description
against each rule. In the summary, list every rule the draft breaks, quoting
the rule and the offending text. The voice guide never changes the verdict on
its own unless a rubric check reads it. In eval mode, ignore `voice-guide.md`.

## Component reads

`rubric.md` defines the checks (Check, Evidence, Pass condition, Weight) and
the verdict rule. `references/evidence-guide.md` says where each evidence
family lives and what good looks like. `procedure.md` is the operating order:
execute it exactly as written. Where the procedure is silent on a step, note
the gap in your summary instead of inventing a step. If `rubric.md` has no
checks or `procedure.md` has no steps (only template comments), refuse to grade
and say which file is empty.

## Verdict and output

The verdict is binary: `accept` (ready to submit) or `reject` (hold). There is
no third verdict; put reservations in a check's evidence line. Before the
block, print one line per check: name, grade, and the deciding fact. End your
reply with the fenced JSON block below, valid and last, with nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: every grade names the diff line, output line, or quote that decided it.
- Proof of the fix is a line in the diff or real printed output from re-running
  the plan's repro or test plan. A sentence saying it works, or a `# note`
  describing what output would look like, is not proof. The rubric's
  `repo-checks-run` check says what counts for the repo's own suite.
- A claim that something was not added passes if nothing in the diff adds it.
- Grade the thing, not the polish: a terse, complete PR can pass; a confident one can hide drift.
- The rubric decides. If a check passes by its stated condition but feels wrong, it passes.
- The procedure decides how. Follow it as written.
- Unclear defaults to fail where the verdict rule is silent: a claim you cannot verify from the package is a failing one.
