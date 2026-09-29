# Evidence guide: where proof lives in a reproduction package

Read the complete package before grading any check. In eval mode, the bundle is the entire evidence universe. Do not fetch the live issue. In live mode, gather the matching signals from the issue thread, repo docs, and the student's draft comment(s) as named below.

## Environment

**Where it lives**

- Eval: original-issue environment claims and the repo-facts / bug-report requirements block; then the candidate repro report's environment section.
- Live: the issue body and labels, the repo's bug-report template / CONTRIBUTING / README, then the draft repro report.

**What good looks like**

Look for reported software version or commit; OS and architecture; installation or build method; shell, browser, driver, build profile, or toolchain when relevant; configuration that participates in the trigger; and the repository's requested bug-report fields. Compare those facts with the candidate report. A different environment does not automatically fail. It fails when the difference is material and silently ignored, or when required information is absent so the attempt cannot be placed. A stated version or environment difference may pass when the candidate shows faithful behavior and explains the difference.

## Steps

**Where it lives**

- Eval: the issue's defining trigger in the issue section; then the candidate's preparation, execution, and input/command blocks in the repro report.
- Live: the issue's reproduction steps or minimal example; then the draft report's commands and fixtures.

**What good looks like**

Record exact input or fixture, command and arguments, prerequisite state, configuration, action order, and any controls needed to distinguish the issue. Inspect the candidate's steps character by character where syntax matters. Small differences such as `=` versus `:`, a prefix range versus an offset-from-end range, or Debug versus Release can change the execution path and are therefore material. The candidate's procedure must be independently repeatable: private files, undisclosed configuration, unavailable fixtures, and paraphrased commands are not independently followable. Repeatability of the candidate's documented test is separate from whether that test targets the original issue.

## Behavior shown

**Where it lives**

- Eval: issue actual/expected behavior and stack or output; then the candidate's raw artifact (command output, log, trace, screenshot description) plus expected/actual claims.
- Live: the same signals on the issue page and in the draft report's artifact blocks.

**What good looks like**

Build a short target signature from the issue (output, panic vs graceful error, error type, exit code, stack location, visual state, timing, control-run behavior, process liveness). Compare the raw candidate artifact with that signature. Do not treat every nonzero exit or error as equivalent: a graceful HCL syntax error does not prove a decoder panic. Prefer raw artifacts over "confirmed," "exactly reproduced," or "100% reproducible." For cannot-reproduce, verify a real followable attempt, shown result, conclusion limited to the tested environment, and named material differences or missing trigger conditions.

## Honesty

**Where it lives**

- Eval: candidate claim comment read against the repro report and issue.
- Live: draft claim (and later the full package) against the issue and the evidence actually shown.

**What good looks like**

A good claim identifies the concrete behavior or investigation; accurately states whether reproduction succeeded; distinguishes observations from hypotheses; names a next action or artifact under the contributor's control; and avoids guarantees about fixes, merges, deadlines, or root causes. Fail unsupported certainty even if it sounds professional. Repeating an incorrect test many times increases consistency, not relevance. An unsupported root-cause diagnosis fails when there is no trace, experiment, source reading, or artifact supporting it.

## Comms

**Where it lives**

- Eval: repo-facts contribution / AI policy and bug-report template rules; then candidate claim and repro comments.
- Live: CONTRIBUTING, templates, and any stated AI-disclosure policy on the repo; then the draft comments.

**What good looks like**

Apply only policies stated in the package or scoped repo docs. Do not invent a disclosure requirement where none exists. When the package requires AI disclosure, absence of disclosure fails. When AI assistance is permitted without issue-comment disclosure, lack of disclosure passes. Also check that the claim is issue-specific: generic praise, "+1" comments, assignment demands, guaranteed timelines, and paste-anywhere boilerplate do not establish a credible, bounded contribution intent.
