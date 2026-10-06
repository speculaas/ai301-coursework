# Evidence guide: where evidence lives in a plan package

Eval bundles always have these sections: **Repo facts**, **Issue**,
**Thread highlights**, **Repro evidence**, **Candidate plan**,
**Candidate plan comment**. Plan sub-headings vary (Diagnosis, Background,
Summary, Scope, Files, Files and areas, Approach, Changes, Proposed
changes, Test plan); some plans are plain paragraphs with "Diagnosis:" or
"Scope:" inline. Look for the content, not the heading.

Live mode (my own package): plan is `plan.md`, comment is my draft comment
file, repro is my posted Unit 2 repro comment on the issue, thread is the
issue page on GitHub, repo facts are the repo's README, CONTRIBUTING, any
AI policy file, and the house rules in `scope.md`.

## Diagnosis and grounding

- **Where the cause is:** the plan's Diagnosis (or Background / Summary /
  first paragraph if there is no Diagnosis heading).
- **Where the proof is:** Repro evidence: Expected, Actual, the numbered
  steps, and especially any control run (flag off, input removed, colors
  off, no pager, older version). Live: my posted repro comment.
- **Good looks like:** the cause explains the Actual, and every control
  in the repro is consistent with it. A control that still shows the bug
  with the blamed component taken out, or shows it fixed with the blamed
  component left in, rules the cause out. A diagnosis copied from a
  thread comment is a claim until the repro backs it.

## Scope

- **Where:** the plan's Scope / In scope / Files lines, and the Not in
  scope / deferred / will-not-touch line (wording varies; "explicitly
  deferred, with reasons" counts).
- **Good looks like:** one change aimed at the reproduced behavior, plus a
  stated fence. A drive-by rewrite shows up as extra items that fix
  nothing in the repro: migrations, new options or settings, framework or
  module restructures, "unify across constructs", CI matrix work.

## Executability

- **Where:** Files or areas, plus Approach / Changes / Proposed changes.
- **Good looks like:** named files or functions and an ordered first step
  someone could start today. Undecided language ("investigate the stack",
  "somewhere", "upstream or vendored, whichever is easier", "not sure
  which layer") means the real decision is pushed to build time.

## Test plan

- **Where:** the plan's Test plan, read next to Repro evidence Steps and
  Expected.
- **Good looks like:** re-run the repro step (or add the issue's case as a
  test) and state what the result should be after the fix: an exit code,
  a timing, an output line, a color that flips. "Run the full suite",
  "nothing regresses", or "should feel fast" names no outcome for this
  fix. An automated test is welcome polish; a manual repro re-run with a
  stated expected-after is enough.

## Honesty

- **Where:** Risks / unknowns, any "open question" or deferral lines, and
  (after build) the Deviations section of `plan.md`.
- **Good looks like:** what has not been measured or checked is labeled as
  unchecked, with what I will do if it turns out badly. A deviation found
  mid-build is written into `plan.md` before any follow-up comment, not
  left for the reviewer to find in the diff.

## Comms

- **Where the words are:** Candidate plan comment (live: my draft comment).
- **Where the signals are:** Thread highlights, especially OWNER, MEMBER,
  and COLLABORATOR lines (live: the issue thread); Repo facts contribution
  policy and AI-use policy (live: CONTRIBUTING, AI_POLICY or similar, and
  `scope.md` house rules such as no piggyback plans and
  `fix/<issue-number>-<slug>` branches).
- **Good looks like:** the comment promises only what the plan contains,
  and it answers the thread: if a maintainer already pointed at a culprit,
  proposed or rejected an approach, or asked someone to test a patch, the
  comment says how my plan relates to that. If the policy requires AI-use
  disclosure in comments or for all AI use, the comment names the tool and
  how much it helped. If the policy only asks for disclosure in the PR, or
  only asks that comments be in my own words, the comment needs my words,
  not a disclosure line. Boilerplate that would fit any issue, or "same as
  above", is not a plan comment.
