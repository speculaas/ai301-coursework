# Plan: Path Review #73 — document `OPENROUTER_API_KEY` in `.env.example`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73
Repro this plan builds on (my Unit 2 report):
https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5886156672

## Repro evidence (quoted from my posted Unit 2 report)

> **Environment.** macOS 15.1; fork `speculaas/pathreview-ai301-fa26-s1` at commit `f89c06f` on `main`. Method: static comparison of `README.md`, `.env.example`, and `core/config.py` (no app run required for this docs mismatch).
>
> **Steps.** 1. Checked out commit `f89c06f`. 2. Read README Quick Start env instruction. 3. Read `.env.example` provider / API key lines. 4. Read `core/config.py` settings fields for OpenAI and OpenRouter keys.
>
> **Expected.** Following the README alone, `.env.example` would list `OPENROUTER_API_KEY` for the contributor to add after `cp .env.example .env`.
>
> **Actual.** README says to add `OPENROUTER_API_KEY` when configuring `.env`, but `.env.example` only shows `LLM_PROVIDER=mock` and `OPENAI_API_KEY=sk-your-key-here` — no `OPENROUTER_API_KEY` line. `core/config.py` defines both `openai_api_key` and `openrouter_api_key` (plus OpenRouter base URL / model defaults).

Artifact from that report:

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

Live re-check on 2026-10-06: upstream `main` is still `f89c06f`, and
`docs/SETUP.md` step 2 says the same thing as the README ("Edit .env and set
your OPENROUTER_API_KEY (required for AI features)").

## Diagnosis

The README Quick Start (and `docs/SETUP.md`) tell a contributor to put
`OPENROUTER_API_KEY` in `.env`, but the file they are told to copy,
`.env.example`, never names that variable. `core/config.py` already has an
`openrouter_api_key` field (pydantic-settings, `case_sensitive = False`, so
`OPENROUTER_API_KEY` in `.env` maps onto it). So the README is right and
`.env.example` is the incomplete file: this is a docs / example-env gap, not a
missing config field. Nothing in the repro contradicts this: all four steps
are static reads and each one is consistent with it.

## Scope

**In:** add an `OPENROUTER_API_KEY` placeholder line (with a one-line comment
saying it is the key the README Quick Start asks for) to the `# LLM provider`
block of `.env.example`, so `cp .env.example .env` produces a `.env` that
already has the line the README tells you to fill in.

**Not in:** `core/config.py` (no field, default, base URL, or model changes),
`README.md` and `docs/SETUP.md` (they are already correct), the
`OPENAI_API_KEY` line, the `LLM_PROVIDER` default or its `Options:` list,
`OPENROUTER_BASE_URL` / `OPENROUTER_MODEL` entries, mock provider logic, any
Python/runtime code, and any wider docs cleanup.

## Files / areas

- `.env.example` — the `# LLM provider` block (only file changed)

## Approach

1. On my fork, branch `fix/73-openrouter-env-example` from `main` at `f89c06f`.
2. In `.env.example`, directly under `OPENAI_API_KEY=sk-your-key-here`, add:
   ```text
   # OpenRouter key — the README Quick Start asks you to set this in .env
   OPENROUTER_API_KEY=sk-or-your-key-here
   ```
   (placeholder style matches the existing `sk-your-key-here` /
   `ghp_your-token-here` lines; no new defaults are invented).
3. Review the diff: exactly one file (`.env.example`), additions only, no
   Python files. Commit as `docs: document OPENROUTER_API_KEY in .env.example`
   with `Refs #73`. Keep this `plan.md` out of the branch.

## Test plan

Re-run my Unit 2 repro steps on the branch, before (`main` at `f89c06f`) and
after (`fix/73-openrouter-env-example`):

1. `git rev-parse --short HEAD`
2. `grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md`
3. `grep -n -i "LLM_PROVIDER\|API_KEY" .env.example`
4. `grep -n "api_key" core/config.py`
5. `cp .env.example /tmp/env73 && grep -c "^OPENROUTER_API_KEY=" /tmp/env73`
   (what `cp .env.example .env` hands a newcomer)

**Fails before:** step 3 shows no `OPENROUTER_API_KEY`; step 5 prints `0`.
**Expected-after:** step 3 shows the new `OPENROUTER_API_KEY=` line next to
`OPENAI_API_KEY`; step 5 prints `1`; steps 2 and 4 are unchanged (README,
SETUP and `core/config.py` untouched). `git diff --stat main` lists only
`.env.example`.

No automated test: there is no test that reads `.env.example`, and adding one
would be outside this docs-only scope.

## Risks / unknowns

- Not verified: whether `LLM_PROVIDER` should also list an `"openrouter"`
  option. `core/config.py` defines `llm_provider` but nothing in the repo
  branches on its value (grep finds only the field), so I am leaving the
  `Options:` comment alone rather than documenting a value I cannot show the
  app reads. A maintainer may want that line changed too; that would be a
  follow-up.
- Not verified: that the app actually uses `openrouter_api_key` at runtime
  (no caller of `settings.openrouter_*` turned up in grep). This change only
  makes the example env match the documented setup; it does not claim the
  OpenRouter path works.
- Comment wording in `.env.example` may need to match a maintainer's
  preferred style.

## Deviations

Nothing changed; the plan held. The branch `fix/73-openrouter-env-example`
(commit `4fdcf49`) adds exactly the two lines in Approach step 2 to
`.env.example`, under `OPENAI_API_KEY`, and `git diff --stat main` shows that
one file with 2 insertions. README, `docs/SETUP.md`, `core/config.py`, and the
`LLM_PROVIDER` `Options:` comment are untouched, and the before/after re-run
matched the expected-after (count `0` before, `1` after). The posted comment's
intent still matches the build, so no thread update is needed.
