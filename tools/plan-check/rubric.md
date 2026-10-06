# Rubric: is this plan ready to post and build from?

Every check names where its evidence lives using the section names in
`references/evidence-guide.md`. In eval mode those are bundle sections
(Repo facts, Issue, Thread highlights, Repro evidence, Candidate plan,
Candidate plan comment). Plan sub-headings vary between packages, so a
check reads whichever heading or paragraph carries that content
(e.g. "Changes", "Proposed changes", or "Approach" all count as the
approach).

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (Diagnosis, Summary, or Background, wherever the cause sentence lives) read side by side with Repro evidence: Expected, Actual, each numbered step, and every control or toggle run with its timing or output. Live: my posted Unit 2 repro comment on the issue. | Pass only if the stated cause explains what the repro actually shows AND no step or control in the repro rules it out. A cause the plan adopts from the thread or the issue still has to survive the repro's controls; agreement with a commenter is not evidence. Fail if a control isolates a different component, if the cause targets a symptom the repro shows is downstream, or if the plan names no cause at all. | required |
| scope-bounded | The plan's in-scope statement (Scope, Files, Files and areas) plus its not-in, deferred, or will-not-touch line, read against the Issue and Repro evidence. | Pass if the plan names what it will change AND states something it will not touch or is deliberately deferring, AND every in-scope item is needed to fix the reproduced behavior. Fail if the fix arrives wrapped in a redesign, migration, new option, framework, or "while I'm here" cleanup the issue never asked for, even when the core fix inside it is correct. | required |
| executable | The plan's files or areas plus its approach (Approach, Changes, Proposed changes), in order. | Pass if a stranger could start the first step without asking me anything: at least one named file, function, or concrete area, and one chosen approach. Fail if the plan says "investigate", "somewhere", "maybe", or "whichever is easier" in place of a decision, or lists options without picking one. | required |
| test-observable | The plan's Test plan read against Repro evidence Steps and Expected. | Pass if the test names a specific before/after a stranger could run that would show THIS fix worked: re-running the repro steps with the expected-after stated, or a named regression case that fails before and passes after. A manual repro re-run is enough. Fail if the only test is "run the whole suite", "nothing regresses", "should feel fast", or any outcome not tied to the reproduced behavior. | required |
| comment-faithful | Candidate plan comment read against the plan's Scope and Approach. | Pass if every promise in the comment is something the plan actually contains (same approach, same files, same scope) and the comment is my own plan, not "same as above" or a pointer to someone else's. Fail if the comment promises work the plan does not contain, drops the plan's not-in line into a bigger promise, or piggybacks on another plan. | required |
| thread-convention | Candidate plan comment read against Thread highlights (especially OWNER / MEMBER / COLLABORATOR lines) and Repo facts (contribution policy, AI-use policy). Live: the issue thread plus the repo's CONTRIBUTING / AI policy docs and `scope.md` house rules. | Pass if (a) when a maintainer in the thread has already isolated a cause, proposed or rejected an approach, or asked for testing, the comment engages that direction (follows it, or says why not), and (b) when the repo's stated policy requires disclosing AI use in comments or "all AI usage", the comment carries that disclosure naming the tool and extent. In eval mode every candidate comment counts as AI-assisted. A policy that only requires disclosure in the PR, or only asks that comments be in the contributor's own words, does not demand disclosure in the comment. Pass if neither signal is present. | required |
| unknowns-honest | Risks / unknowns section, deferral notes, and any Deviations section. | Plan states what it has not verified as unverified (and records any build deviation in the plan, not only in the diff). | preferred |
| automated-test | The plan's Test plan. | Plan adds or names an automated regression test for the change. | preferred |

## Verdict rule

accept (ready to post and build from) only if every required check is
pass. Any required check graded fail or unclear means reject (hold).
unclear counts as fail: if the package lacks the section a required check
needs, that check is unclear and the package holds. Preferred checks are
reported in the summary but never change the verdict, so a terse plan
with no automated test and no risks section can still be ready.
