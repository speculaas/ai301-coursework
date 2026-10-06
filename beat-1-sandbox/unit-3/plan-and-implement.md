# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

speculaas

**Plan comment**

Permalink: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-6014087169

Posted text:

### Plan for #73: add `OPENROUTER_API_KEY` to `.env.example`

**Diagnosis.** Building on my reproduction above (commit `f89c06f`, still upstream `main` today): the README Quick Start and `docs/SETUP.md` both tell you to set `OPENROUTER_API_KEY` in `.env`, and `core/config.py` has a matching `openrouter_api_key` field (pydantic-settings, case-insensitive), but `.env.example`, the file you `cp` to make `.env`, never names that variable. So the README is right and `.env.example` is the incomplete file.

**Scope.**
- In: add one `OPENROUTER_API_KEY=sk-or-your-key-here` placeholder line (with a one-line comment above it) to the `# LLM provider` block of `.env.example`, directly under `OPENAI_API_KEY`.
- Not in: `core/config.py` (no field or default changes), `README.md` / `docs/SETUP.md` (already correct), the `OPENAI_API_KEY` line, the `LLM_PROVIDER` default or its `Options:` list, `OPENROUTER_BASE_URL` / `OPENROUTER_MODEL`, or any Python code.

**Approach.** On my fork, branch `fix/73-openrouter-env-example` from `main`; edit only `.env.example`; check the diff is that one file, additions only.

**Test.** Re-run my repro steps before and after: `grep -n -i "LLM_PROVIDER\|API_KEY" .env.example`, and `cp .env.example /tmp/env73 && grep -c "^OPENROUTER_API_KEY=" /tmp/env73`. Before: no `OPENROUTER_API_KEY` line, count `0`. Expected after: the new line next to `OPENAI_API_KEY`, count `1`; README, SETUP and `core/config.py` unchanged.

**Unknowns.** I'm leaving the `LLM_PROVIDER` `Options:` comment as is: nothing in the repo branches on `llm_provider`'s value, so I can't show that an `"openrouter"` option does anything. If a maintainer wants that line changed too, I'd treat it as a follow-up.

---

## Your branch

**Branch**

fix/73-openrouter-env-example

Fork branch: https://github.com/speculaas/pathreview-ai301-fa26-s1/tree/fix/73-openrouter-env-example
(commit `4fdcf49` — `docs: document OPENROUTER_API_KEY in .env.example`, `Refs #73`; one file, `.env.example`, 2 insertions; `plan.md` is not on the branch)

**Evidence**

My Unit 2 repro steps (static comparison of `README.md` / `docs/SETUP.md`, `.env.example`,
and `core/config.py`), plus the `cp .env.example` step a newcomer actually runs, re-run as a
script (`repro.sh`, run with `bash` so each command is echoed with `+`) in my fork's clone.

Commands:

```bash
git rev-parse --abbrev-ref HEAD
git rev-parse --short HEAD
grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md
grep -n -i "LLM_PROVIDER\|API_KEY" .env.example
grep -n "api_key" core/config.py
cp .env.example /tmp/env73 && grep -c "^OPENROUTER_API_KEY=" /tmp/env73
git diff --stat main
```

Before (`main` at `f89c06f`):

```text
+ git rev-parse --abbrev-ref HEAD
main
+ git rev-parse --short HEAD
f89c06f
+ grep -n OPENROUTER_API_KEY README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
+ grep -n -i 'LLM_PROVIDER\|API_KEY' .env.example
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
+ grep -n api_key core/config.py
19:    openai_api_key: str = Field(default="")
20:    openrouter_api_key: str = Field(default="")
+ cp .env.example /tmp/env73
+ grep -c '^OPENROUTER_API_KEY=' /tmp/env73
0
+ git diff --stat main
```

After (`fix/73-openrouter-env-example` at `4fdcf49`):

```text
+ git rev-parse --abbrev-ref HEAD
fix/73-openrouter-env-example
+ git rev-parse --short HEAD
4fdcf49
+ grep -n OPENROUTER_API_KEY README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
+ grep -n -i 'LLM_PROVIDER\|API_KEY' .env.example
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
21:OPENROUTER_API_KEY=sk-or-your-key-here
+ grep -n api_key core/config.py
19:    openai_api_key: str = Field(default="")
20:    openrouter_api_key: str = Field(default="")
+ cp .env.example /tmp/env73
+ grep -c '^OPENROUTER_API_KEY=' /tmp/env73
1
+ git diff --stat main
 .env.example | 2 ++
 1 file changed, 2 insertions(+)
```

Result: before, the copied `.env` has no `OPENROUTER_API_KEY` line (count `0`) even though
README and SETUP tell you to set it; after, `.env.example` line 21 carries it next to
`OPENAI_API_KEY` and the copied `.env` has it (count `1`). README, SETUP and `core/config.py`
output is identical before and after, and the diff is `.env.example` only.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

All runs on Tue Oct 6, 2026 (MDT), from the Unit 3 starter `eval/` folder, grading
`~/.claude/skills/plan-check/`, model pinned to Sonnet by the harness:

1. ~3:38 AM — first smoke attempt: no package finished (harness / `claude -p` errors), no score.
2. ~3:40 AM — partial run, 1 package (`pkg-01`): 1/1 agree (reject / reject).
3. ~3:50 AM — second smoke attempt: no package finished, no score.
4. ~3:55 AM — partial run, 3 packages (`pkg-04`, `pkg-05`, `pkg-06`): 3/3 agree.
5. ~4:03–4:06 AM — confirming full run with `--save-run eval-run.txt`: **20/20** scored items
   (bar 18/20: PASS); categories clear-accept 7/7, scope-creep 4/4, thread-convention 2/2,
   unbuildable 3/3, wrong-cause 4/4.

No rubric, procedure or evidence-guide edits were made between these runs. The partial runs
were never saved (the harness refuses `--save-run` on them). The last score, 20/20, matches
the `agreement: 20/20 scored items  (bar: 18/20: PASS)` line in the committed `eval-run.txt`.

**Package analysis**

`pkg-20` (`ghostty-org/ghostty#11261`, category `thread-convention`). Gold label: **reject**.
My rubric: **reject** (agree). The plan itself passed every other required check: the run
graded diagnosis-grounded, scope-bounded, executable, test-observable and comment-faithful all
`pass`, and noted the comment "correctly engages mitchellh's proposed direction (follows the
generation-counter approach he suggested over the rejected unconditional-recompute)". It still
failed on thread-convention part (b). The package's Repo facts say "All AI usage in any form
must be disclosed, stating the tool used and the extent of the assistance", and my check says
"In eval mode every candidate comment counts as AI-assisted". The candidate comment has no
disclosure line, so the run's evidence was: "Repo facts require disclosure of 'all AI usage in
any form, stating the tool used and the extent'; candidate plan comment contains no disclosure
line." Since thread-convention is required, that one fail is a reject. This is the case my
rubric was built to catch: a strong, bounded plan whose comment would still get bounced by the
repo's stated policy.

**Check rationale**

From `tools/plan-check/rubric.md`, as it reads now:

> | thread-convention | Candidate plan comment read against Thread highlights (especially OWNER / MEMBER / COLLABORATOR lines) and Repo facts (contribution policy, AI-use policy). Live: the issue thread plus the repo's CONTRIBUTING / AI policy docs and `scope.md` house rules. | Pass if (a) when a maintainer in the thread has already isolated a cause, proposed or rejected an approach, or asked for testing, the comment engages that direction (follows it, or says why not), and (b) when the repo's stated policy requires disclosing AI use in comments or "all AI usage", the comment carries that disclosure naming the tool and extent. In eval mode every candidate comment counts as AI-assisted. A policy that only requires disclosure in the PR, or only asks that comments be in the contributor's own words, does not demand disclosure in the comment. Pass if neither signal is present. | required |

Why it reads that way: in the Unit 3 activity (Phase 2 draft), thread and policy signals only
showed up inside a **preferred** check, "comment-faithful (preferred): look in Candidate plan
comment; passes if it promises only what the plan contains and engages thread or repo-policy
signals when present." A preferred check never changes the verdict, so that draft would have
accepted `pkg-04` (the comment ignores the owner's in-thread direction) and `pkg-20` (no
required AI disclosure). Both are reject in gold, and the category floor counts. I split it out
as its own **required** check with two named triggers: (a) a maintainer line that isolates a
cause, proposes or rejects an approach, or asks for testing, and (b) a stated policy requiring
AI disclosure in comments or for "all AI usage". I added the
carve-out ("A policy that only requires disclosure in the PR, or only asks that comments be in
the contributor's own words, does not demand disclosure in the comment") so PR-only policies
don't flip clear accepts. "Pass if neither signal is present" keeps it from punishing quiet
threads.

**Trade-offs**

This check gives up leniency on otherwise-ready plans: `pkg-20` is a plan that passes every
other required check, and it is rejected only for a missing disclosure line, so a reader who
wanted "is the plan good?" alone would call that a false hold. It also leans on a literal read
of maintainer lines. A thread where the direction comes from a non-maintainer, or is only
implied, will not trigger part (a), and I accept that it can miss that case. Nothing elsewhere
changed: the confirming full run scored clear-accept 7/7, so the PR-only carve-out did not
turn any clean accept into a reject, and I made no edits after that run, so no `--only` canary
re-run was needed.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
