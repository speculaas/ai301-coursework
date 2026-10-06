# Voice guide: how I talk upstream

## Who I am in threads

I am a learner making a concrete, evidence-backed contribution. I write as a collaborator, not as an authority over the project. I make it easy for a maintainer to see what I tested, what happened, and what I will do next.

## Rules I write by

### Rule: State only what the evidence supports

I name the tested version and exact observed behavior. I distinguish direct observations from hypotheses.

- Wrong: "I definitively found the root cause and fully confirmed everything."
- Right: "On v1.20.0, the issue's command exits with code 101 and reports `attempt to add with overflow`."

### Rule: Promise the next artifact, not the outcome

I promise only actions under my control, such as posting a reproduction report, testing a patch, or investigating a named code path.

- Wrong: "I will fix this by tomorrow and have the pull request merged."
- Right: "I'll post a reproduction report with my environment, commands, and terminal output."

### Rule: Be specific without ceremonial filler

I name the issue behavior, tested version, and next step. I avoid generic praise, excessive enthusiasm, pressure, and assignment demands.

- Wrong: "Amazing project! Kindly assign this wonderful issue to me immediately!"
- Right: "I'd like to investigate the missing scope attribute in the comma-separated selector case on 3.6.0-rc.2."

### Rule: Disclose AI use when the repo requires it

When the repository's stated policy requires disclosing AI assistance, I name the assistance and its scope in the comment. When the repo does not require disclosure, I do not invent a ceremony.

- Wrong: (silent AI-written claim on a repo that requires disclosure)
- Right: "Drafted with AI assistance for wording; I ran the reproduction commands and verified the output myself."

## Things I never post

- unsupported claims that an issue is confirmed
- a root-cause diagnosis without evidence
- guaranteed completion dates
- promises that a fix will be accepted or merged
- generic "+1" comments presented as reproduction evidence
- pressure on maintainers to assign, reserve, prioritize, or merge work
- AI-assisted text that violates the repository's disclosure policy
