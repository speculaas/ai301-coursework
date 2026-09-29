# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment is placed | Original issue, repository bug-report requirements, and candidate reproduction report | The report identifies the tested software version or commit, operating system, installation or build method when relevant, and every environment condition needed to interpret the result. Version or environment differences from the issue are stated rather than silently ignored. | required |
| Procedure is independently followable | Original issue trigger, candidate inputs, prerequisites, setup steps, and commands | A stranger can repeat the candidate's documented attempt without private files, unstated configuration, missing fixtures, or guesses. This check grades whether the candidate's own test is independently repeatable; whether that test targets the original issue is graded by Artifact proves the reported outcome. The steps preserve the issue's trigger unless a deviation is explicitly identified and justified. A one-character or other small textual difference is material when it changes the parser path, execution path, or resulting behavior (for example `=` versus `:` in HCL input). | required |
| Artifact proves the reported outcome | Issue-defining behavior, candidate expected and actual behavior, raw logs, output, trace, screenshot description, or other artifact | The artifact is traceable to the documented procedure and supports the candidate's stated outcome. For a claimed reproduction, the issue-defining signals match the issue: triggering input, command and arguments, material environment conditions, error or behavior type, exit code when available, and point of failure. For a cannot-reproduce report, the artifact supports the failed attempt and the report identifies material environment or trigger differences. Prefer raw artifacts over characterizations such as "confirmed" or "exactly reproduced." | required |
| Claim is specific and honest | Candidate claim comment read against the report and issue | The claim names the concrete issue behavior or investigation, does not overstate what the evidence proves, and promises only a next action or artifact under the contributor's control. Generic assignment requests, unsupported certainty, invented root causes, guaranteed fixes, or unsupported deadlines fail. | required |
| Repository communication policy is satisfied | Repo-facts contribution and AI policy plus candidate comments | The candidate follows every communication or disclosure requirement stated in the package. When disclosure is required, the comment names the AI use with sufficient scope. When no disclosure is required, its absence does not fail this check. | required |

## Verdict rule

Return `accept` only when every required check is `pass`.

Return `reject` when any required check is `fail` or `unclear`.

Use `unclear` only when evidence required to grade the check is genuinely absent. Do not use `unclear` merely because the package is complicated. Any `fail` or `unclear` on a required check produces `reject`.

Grade the proof rather than its formatting, confidence, detail, or length. A terse but complete package may be accepted. A polished package must be rejected when its artifact shows a different behavior.

A faithfully documented cannot-reproduce result may be accepted when the environment, procedure, evidence, limitations, and material differences are all explicit.
