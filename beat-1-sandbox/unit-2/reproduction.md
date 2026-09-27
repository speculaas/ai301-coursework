# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

speculaas

---

## Posted upstream

**Claim comment**

<!-- TODO after live-check + post: replace this block with the comment permalink, then the exact posted body. -->

Permalink: _not posted yet — draft below; run_ `claude "repro-check: grade my draft claim comment in beat-1-sandbox/unit-2/claim-draft-issue-73.md for issue https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73"` _until accept, then post, then paste the permalink here._

Draft text (copy source of truth: `claim-draft-issue-73.md`):

I'd like to investigate the docs mismatch on this issue: `README.md` Quick Start says to add `OPENROUTER_API_KEY` when copying `.env.example` to `.env`, but `.env.example` does not list `OPENROUTER_API_KEY`, and its `LLM_PROVIDER` comment only offers `mock` and `openai`. `core/config.py` defines both `openai_api_key` and `openrouter_api_key`, so the setup docs and the example env file currently point a new contributor in different directions.

I'll set up from the repo docs and post a reproduction report with the exact file excerpts I compared (README, `.env.example`, and the relevant settings fields). I'm not claiming a root cause beyond that disagreement, and I'm not promising a fix or a timeline.

Note: `ocabezas95` already posted a claim and a repro on #73 (2026-09-26). Path Review house rules: a classmate's claim does not block posting your own; do not piggyback (“same as above”).

**Reproduction comment**

<!-- TODO after setup + live-check + post. Stub outline only — do not post this stub. -->

Permalink: _not posted yet._

Planned report shape (fill with real commit hash, paths, and quoted lines after you reproduce):

## Environment
- OS: macOS … (fill)
- Python: … (fill)
- Repo / fork: `speculaas/pathreview-ai301-fa26-s1` (or the sandbox clone you actually use)
- Commit: `<hash>` on `<branch>`, clean working tree (or note dirty files)
- Method: static comparison of `README.md`, `.env.example`, and `core/config.py` (running the app is not required to observe the docs mismatch)

## Steps
1. Clone / checkout the commit above.
2. Open `README.md` Quick Start and quote the `.env` / `OPENROUTER_API_KEY` instruction.
3. Open `.env.example` and quote the `LLM_PROVIDER` comment and listed API key variables.
4. Open `core/config.py` and quote the `openai_api_key` / `openrouter_api_key` fields.

## Expected vs actual
- Expected (from following README alone): `.env.example` would document `OPENROUTER_API_KEY` (and any provider values README assumes).
- Actual: `.env.example` documents only `mock` / `openai` and `OPENAI_API_KEY`; no `OPENROUTER_API_KEY` line. `core/config.py` still defines both keys.

## Artifact
Paste the exact excerpts (or a short terminal dump such as `rg -n 'OPENROUTER|LLM_PROVIDER|OPENAI_API_KEY' README.md .env.example core/config.py`) produced by the steps above.

---

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

<!-- TODO after harness runs. Last score must match committed eval-run.txt. -->

_No scored eval run yet._ Planned sequence: calibration `--only calib-01,calib-02,calib-03,calib-04 --include-calibration` → optional `--limit` smoke → full scored run → `--only` on disagreements with canaries → confirming full run with `--save-run eval-run.txt`. Replace this paragraph with the ordered agreement scores once those runs exist (example shape: `16/20 → 18/20 (final)`).

**Package analysis**

<!-- TODO after a full or targeted scored run. Use a real pkg-NN id, not calib-*. -->

_Pending eval._ After the first full run, pick one scored package (for example a wrong-target case such as those the draft rubric aimed at), name it by id, record your verdict vs gold, and explain which check decided it. Leave this marker until then.

**Check rationale**

Quote from the current starter/homework rubric (to upload into `tools/repro-check/rubric.md`):

> | Procedure is independently followable | Original issue trigger, candidate inputs, prerequisites, setup steps, and commands | A stranger can repeat the candidate's documented attempt without private files, unstated configuration, missing fixtures, or guesses. This check grades whether the candidate's own test is independently repeatable; whether that test targets the original issue is graded by Artifact proves the reported outcome. The steps preserve the issue's trigger unless a deviation is explicitly identified and justified. A one-character or other small textual difference is material when it changes the parser path, execution path, or resulting behavior (for example `=` versus `:` in HCL input). | required |

Why it reads this way: the live-session / worksheet cold-run on `calib-03` showed that a polished package can be followable while still testing the wrong trigger (`=` vs `:`). Splitting “repeatable procedure” from “artifact proves the reported outcome,” and calling out one-character material differences, keeps those failure modes on separate checks so a HOLD/reject is about fidelity, not formatting.

**Trade-offs**

This Procedure wording gives up treating “any runnable steps” as enough for accept: a package whose commands are clear but whose input silently changes the issue trigger still fails on Artifact (or on Procedure if the deviation is unstated). After the first full eval, re-check with `--only` including at least one wrong-target canary (for example a known wrong-target `pkg-*` from the harness README) whenever this check is loosened, so the split does not regress. Until that confirming run exists, the trade-off is documented from the calib-03 lesson rather than from a scored disagreement.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`; draft claim in `claim-draft-issue-73.md`.
