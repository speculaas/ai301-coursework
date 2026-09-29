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

Permalink: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5886021336

Posted body:

I'd like to investigate the docs mismatch on this issue: `README.md` Quick Start says to add `OPENROUTER_API_KEY` when copying `.env.example` to `.env`, but `.env.example` does not list `OPENROUTER_API_KEY`, and its `LLM_PROVIDER` comment only offers `mock` and `openai`. `core/config.py` defines both `openai_api_key` and `openrouter_api_key`, so the setup docs and the example env file currently point a new contributor in different directions.

I'll set up from the repo docs and post a reproduction report with the exact file excerpts I compared (README, `.env.example`, and the relevant settings fields). I'm not claiming a root cause beyond that disagreement, and I'm not promising a fix or a timeline.

Note: `ocabezas95` already posted a claim and a repro on #73 (2026-09-26). Path Review house rules: a classmate's claim does not block posting your own; do not piggyback (“same as above”).

**Reproduction comment**

Permalink: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5886156672

Posted body:

### Reproduction Report: README vs `.env.example` docs mismatch (#73)

**Environment.** macOS 15.1; fork `speculaas/pathreview-ai301-fa26-s1` at commit `f89c06f` on `main`. Method: static comparison of `README.md`, `.env.example`, and `core/config.py` (no app run required for this docs mismatch).

**Steps.**

1. Checked out commit `f89c06f`.
2. Read README Quick Start env instruction.
3. Read `.env.example` provider / API key lines.
4. Read `core/config.py` settings fields for OpenAI and OpenRouter keys.

**Expected.** Following the README alone, `.env.example` would list `OPENROUTER_API_KEY` for the contributor to add after `cp .env.example .env`.

**Actual.** README says to add `OPENROUTER_API_KEY` when configuring `.env`, but `.env.example` only shows `LLM_PROVIDER=mock` and `OPENAI_API_KEY=sk-your-key-here` — no `OPENROUTER_API_KEY` line. `core/config.py` defines both `openai_api_key` and `openrouter_api_key` (plus OpenRouter base URL / model defaults).

**Artifact.**

```text
README.md:
# Configure environment (add your OPENROUTER_API_KEY to .env)
cp .env.example .env

.env.example:
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here

core/config.py:
openai_api_key: str = Field(default="")
openrouter_api_key: str = Field(default="")
```

This confirms the docs disagreement described in the issue. I am not claiming a product bug beyond the documentation / example-env mismatch, and I am not proposing a fix in this comment.


---

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

<!-- TODO after harness runs. Last score must match committed eval-run.txt. -->

Confirming full run 2026-09-29 (`--save-run eval-run.txt`, Sonnet): **19/20 PASS** (bar 18/20). Categories: clear-accept 7/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. Sole disagreement: `pkg-09` (gold accept / clear-accept; harness reject on Procedure + Artifact — honest cannot-repro false negative; rubric left unchanged). Prior smoke `--limit 3`: 3/3. Last score matches committed `eval-run.txt` in this directory.

**Package analysis**

`pkg-09` (`sharkdp/fd#2033`, category `clear-accept`). Gold verdict: **accept**. Harness verdict on the confirming 2026-09-29 run: **reject** (disagreement). Deciding checks: **Procedure is independently followable** and **Artifact proves the reported outcome**. Gold treats this as an honest cannot-reproduce with a real attempt, marker-order artifacts, and named material limits (e.g. uniform name lengths, 2 MiB `ARG_MAX`). The model failed Procedure because the asymmetric probe (“pad the second command’s arguments ~40% longer”) was only described in prose—no exact modified command a stranger could re-run—and treated Artifact as unclear/fail because the pasted `order.log` matched the *symmetric* baseline while the padded run had no raw artifact of its own. I left the rubric unchanged so wrong-target / no-evidence packages would not loosen.

**Check rationale**

Quote from the current starter/homework rubric (to upload into `tools/repro-check/rubric.md`):

> | Procedure is independently followable | Original issue trigger, candidate inputs, prerequisites, setup steps, and commands | A stranger can repeat the candidate's documented attempt without private files, unstated configuration, missing fixtures, or guesses. This check grades whether the candidate's own test is independently repeatable; whether that test targets the original issue is graded by Artifact proves the reported outcome. The steps preserve the issue's trigger unless a deviation is explicitly identified and justified. A one-character or other small textual difference is material when it changes the parser path, execution path, or resulting behavior (for example `=` versus `:` in HCL input). | required |

Why it reads this way: the live-session / worksheet cold-run on `calib-03` showed that a polished package can be followable while still testing the wrong trigger (`=` vs `:`). Splitting “repeatable procedure” from “artifact proves the reported outcome,” and calling out one-character material differences, keeps those failure modes on separate checks so a HOLD/reject is about fidelity, not formatting.

**Trade-offs**

This Procedure wording gives up treating “any runnable steps” as enough for accept: a package whose commands are clear but whose input silently changes the issue trigger still fails on Artifact (or on Procedure if the deviation is unstated). After the confirming full run, the cost showed up on `pkg-09`: an honest cannot-reproduce that gold accepts was rejected because the decisive probe stayed in prose and lacked its own artifact. I did **not** loosen Procedure/Artifact afterward—`--only` with wrong-target canaries would be required before any softening, and 19/20 already clears the bar. Until a deliberate rewrite, the trade-off is: fewer false accepts on wrong-target packages, at the price of occasional false rejects on incomplete cannot-repro write-ups (lesson for live reports: show the exact probe command and its raw output).


---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`; draft claim in `claim-draft-issue-73.md`.
