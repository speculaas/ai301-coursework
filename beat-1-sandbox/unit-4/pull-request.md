# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/181

**Branch**

`fix/73-openrouter-env-example`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Targeted `--only` run of 7 (`pkg-09,pkg-13,pkg-16,pkg-18,pkg-01,pkg-20,calib-04` with `--include-calibration`): 6 of 7 agreed. The miss was pkg-13 (gold accept, my verdict reject, `failed: repro-rerun-observable`). This was a partial run, so it has no agreement line of its own.
2. I revised `rubric.md` (see Check rationale) and sanity-checked pkg-13 and calib-04 by hand in Claude web with the exact harness prompt. pkg-13 came back accept and calib-04 still reject. Those were not harness runs, so they have no score.
3. Full run with `--save-run eval-run.txt`: `agreement: 18/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 5/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the `eval-run.txt` committed next to this file.

**Package analysis**

pkg-11. My rubric decided reject, and the gold label is accept. Every check passed except `repro-rerun-observable`. The grader's evidence for that check was: "After-state lines are comments ('# after: searches normally, no error', "# after: 'foo ' not listed (ignored)") not real pasted command output."

My rubric read it that way because of the fail clause I wrote: "if a `# note` or prose sentence ("works now", "verified on my machine", "colors work") stands in for the output". The PR's after-state is written as `# after:` lines describing the result rather than showing it, so to my rubric it looks exactly like the `# note` case. Compare pkg-13, which I fixed earlier: its `-- after: { code = 0 }` lines show the actual values, and my rubric counts those. pkg-11's `# after:` lines describe the outcome in words. A human reviewer would probably accept pkg-11, because the regression test `escaped_trailing_space_is_kept` and `cargo test -p ignore passes (214 tests)` cover the fix. My rubric doesn't let a test stand in for the re-run repro, though. pkg-05 failed the same check for a similar reason: its second named repro was reported only as "Same-key redefine prints the one-time warning".

**Check rationale**

From `rubric.md`, the `repro-rerun-observable` row, pass condition, as it reads now:

"Pass if every repro or failure mode the Test plan names is re-run on the changed code path, with real printed output (command output, error text, exit code, count, or a before/after table) showing the stated expected-after; the before may be reused from the repro. Output pasted with inline before/after labels (e.g. `-- before: { code = 1, ... }` / `-- after: { code = 0 }`) counts as printed output when it shows the actual values. A control or unchanged-behaviour check (a no-ampersand case, an existing flag still working) may be reported in one sentence; only the bug's own repro and named failure modes need pasted output."

The last two sentences are my revision after the first `--only` run. pkg-13 was rejected even though its gold label is accept. I read the grader's log, and the grader had accepted the inline `-- before/after` output. What it failed on was the control: "\"No-ampersand control unchanged.\" is a bare prose assertion with no command or output shown." I decided a control only needs a sentence, but I kept pasted output as a requirement for the bug's own repro and every named failure mode. That's the part the not-tested category depends on. I rejected the broader option of letting a passing regression test stand in for the re-run, because it would have let the not-tested rejects through.

**Trade-offs**

Loosening the control rule could have flipped rejects that fail this same check (pkg-04, pkg-07, pkg-14, calib-04). I re-checked calib-04 as the canary, since its second crash is the kind that could be mistaken for a control. It stayed reject, and the grader's evidence was "Repro 2 (`-j 200000 -x echo .`) named in the Test plan is never re-run". In the full run, all four not-tested packages still agreed (4/4).

What I gave up: the check is still strict about how the re-run is shown, so it misses two clear accepts, pkg-05 and pkg-11. Both describe a named repro's result in prose or `# after:` comments instead of pasting it. I'm accepting those two misses rather than loosening the check again. Loosening it to accept described results would make it the same "works now" prose that the not-tested category is built to catch, and I'd need another full run to show nothing else flipped.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
