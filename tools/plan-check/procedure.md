# Procedure: how this skill grades a plan package

## Read order

1. Eval mode: the bundle is the whole world. Do not fetch the live issue,
   even though the `source:` line names it. Live mode: read `scope.md`
   first and stop if the issue is outside it, then `voice-guide.md`.
2. Read the package in this order, writing one short note from each part
   before moving on:
   1. **Repo facts**: note the contribution policy and any AI-use rule,
      word for word, and whether it applies to comments, PRs, or all use.
   2. **Issue**: note the reported behavior in one line.
   3. **Thread highlights**: note every OWNER / MEMBER / COLLABORATOR line
      that isolates a cause, proposes or rejects an approach, or asks for
      testing. Note any non-maintainer "root cause" claim separately as a
      claim, not a fact.
   4. **Repro evidence**: note Expected, Actual, and every numbered step
      and control with its result (timings, exit codes, output). Mark
      which step or control isolates the trigger.
   5. **Candidate plan**: note the cause sentence, the in-scope and not-in
      lines, the named files, the approach steps, the test plan, and any
      risks or deferrals.
   6. **Candidate plan comment**: note each promise it makes and any
      disclosure line.
3. Why this order: the repro is read before the plan so its controls are
   already in my notes when I meet the plan's cause. Reading the plan
   first is how a confident thread diagnosis (calib-03's key-binding
   story) slides through: the plan sounds right until the timing matrix
   is next to it. Thread and repo facts come before the comment so the
   comment is read against what the maintainers and policy already asked.

## Evidence gathering

Use `references/evidence-guide.md` for where each family lives. For each
check, gather exactly this before grading anything:

1. **diagnosis-grounded**: put the plan's cause sentence on one line and
   the repro's Actual plus each control result directly under it. For
   every control, write one line: "consistent" or "rules it out, because
   ___". If the plan's cause came from a thread commenter, say so.
2. **scope-bounded**: list each in-scope item and each not-in or deferred
   item. Next to each in-scope item, write which reproduced behavior it
   fixes, or "none" if it is extra.
3. **executable**: copy the first approach step and the named files or
   areas. Underline any undecided word ("investigate", "somewhere",
   "maybe", "or", "whichever").
4. **test-observable**: copy the test plan, then the repro step it re-runs
   and the expected-after it states. If it re-runs nothing from the repro
   and names no fails-before/passes-after case, write "not tied to repro".
5. **comment-faithful**: two columns, comment promises vs plan contents;
   mark any promise with no match in the plan.
6. **thread-convention**: list the maintainer direction lines from Read order
   step 2.3, and for each write whether the comment follows it, explains a
   different path, or ignores it. Then quote the AI-use policy and say
   whether it requires disclosure in comments; if yes, quote the
   comment's disclosure line or write "none".
7. **unknowns-honest** and **automated-test**: copy the risks/deferral
   lines and any named automated test, or write "none".
8. Quote only from the package (eval) or from the live sources the
   evidence guide names (live). Never fill a gap with what the real
   project probably does.

## Check execution

1. Run checks in rubric table order: diagnosis-grounded, scope-bounded,
   executable, test-observable, comment-faithful, thread-convention, then
   the preferred checks. Grade every check even after one fails, so the
   summary shows the full picture.
2. Apply only the rubric's pass condition to the gathered lines for that
   check. Do not reread the whole package; go back only to a section a
   gathered line is missing from.
3. Grade pass, fail, or unclear:
   - pass: the gathered lines meet the condition.
   - fail: the gathered lines break the condition (name the line).
   - unclear: the section the check needs is absent from the package.
     Name the missing section in one line.
4. A long, confident plan gets no credit for length, and a terse one loses
   none for being short (calib-01 is ready as written). Grade the change,
   not the write-up.
5. If two readings of a pass condition both seem fair, grade the stricter
   one for required checks and say in the summary which line forced it.

## Verdict assembly

1. Apply the rubric's verdict rule: any required fail or unclear means
   reject; otherwise accept. Preferred grades go in the summary only and
   never flip the verdict.
2. In the readable summary, give one line per check, and quote the
   gathered evidence line from the deciding required check next to the
   verdict (for an accept, quote the diagnosis-grounded line).
3. Live mode: list any voice-guide rule the draft comment breaks, quoting
   the rule. This never changes the verdict.
4. End with the fenced JSON block from SKILL.md (verdict accept or reject;
   each check pass, fail, or unclear with its one-line evidence). Nothing
   after it.
