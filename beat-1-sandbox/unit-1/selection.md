# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73

**Verdict output**

<!-- Full live-mode skill output from Claude Code session b8970ca5 (2026-09-22 ~23:04 MT). -->

```
All required checks pass. Here's the graded evaluation for issue #73 in `codepath/pathreview-ai301-fa26-s1`:

| Check | Grade | Evidence |
|---|---|---|
| maintainer-alive | pass | 3 of last 5 default-branch commits are from 2026-09-16 (6 days before today, 2026-09-22), well within 90 days |
| repo-in-use | pass | `archived: false`, last push 2026-09-16 (6 days ago), within 180 days |
| scope-fits-newcomer | pass | Single bounded doc/config fix: README says set `OPENROUTER_API_KEY`, but `.env.example` only documents `mock`/`openai` providers and `OPENAI_API_KEY`; confirmed by reading `.env.example` directly. Concrete files named, 1–2hr estimate, no design debate |
| not-already-taken | pass | `assignees: []`, `comments: []`, no linked/mentioned PRs found via timeline or search |
| ai-policy-ok | pass | `docs/CONTRIBUTING.md` has no AI-use restriction — silence passes per rubric |
| good-first-signal (preferred) | pass | Labeled `good first issue`, `tier-1` (starter difficulty), `bug`, `docs` |

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "3 of last 5 commits dated 2026-09-16, 6 days before today"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false, pushed_at: 2026-09-16"},
    {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "Bounded doc/config-agreement fix between README.md and .env.example, files named, 1-2hr estimate"},
    {"name": "not-already-taken", "grade": "pass", "evidence": "assignees: [], comments: [], no linked/referenced PRs found"},
    {"name": "ai-policy-ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI-use ban; silence passes"},
    {"name": "good-first-signal", "grade": "pass", "evidence": "labels: good first issue, tier-1, bug, docs"}
  ],
  "verdict": "accept"
}
```

**Recommendation: accept.** This fits your profile well — it's a small, reproducible docs/config-hygiene bug (README vs. `.env.example` disagreement on the LLM provider env var) with clear acceptance criteria, no assignee/claim conflicts, and a live maintainer/repo. Good candidate to practice the contribution loop without sprawling scope.
```

**The verdict must record `accept` for this issue.**

---

## Eval iterations

**Run history**

1. Offline smoke (`simulate_rubric.py`) while iterating `scope-fits-newcomer` — heuristic estimate only, not Claude.
2. Cheap Claude `--limit 3` — agreement 2/3; `issue-01` gold accept / Claude reject on `scope-fits-newcomer` (false reject after over-tight wording).
3. Loosened `scope-fits-newcomer` so one coherent multi-file docs package still passes; kept true umbrella/wish fails.
4. Re-check `--only issue-01` → accept/accept; spot-check reject canaries `--only issue-10,issue-15,issue-20` → all agree reject.
5. Full confirming run with `--save-run` — **agreement 18/20 (PASS)**. This is `eval-run.txt` (written 2026-09-23T04:23:51Z, rubric sha256 prefix `11479186aa255626`). Category floors OK; disagreements: `issue-15` (gold reject / graded accept), `issue-19` (gold accept / failed `scope-fits-newcomer`).

**Issue analysis**

`issue-19` — gold **accept**, my skill **reject** (`failed: scope-fits-newcomer`). On the final 18/20 run the model treated this bundle as too large or under-specified for a newcomer even though gold marks it accept. That is the miss I am willing to keep at the pass bar: the same wording that stopped earlier false-rejects on coherent docs work (`issue-01`) still occasionally over-rejects a gold accept. I did not loosen further because a wider net would risk more false accepts on scope canaries. Counterpart disagreement: `issue-15` gold reject / graded accept (false accept) — also left as-is once the overall bar passed.

**Check rationale**

Quoted from uploaded `tools/issue-select/rubric.md` (`scope-fits-newcomer` Pass column):

> **Pass** if the ask is **one coherent first contribution**: a single bounded change (docs, tests, small bugfix, one UI/API behavior, or a concrete config/CI tweak) **or** a docs/update package that still serves **one feature or workflow** (e.g. add one permanent docs page and update a few related pointers/pages). Need enough detail to start (repro, acceptance criteria, or concrete proposed changes). Terse writeups can still pass when the work is clearly sized. **Fail** if any of these hold: (1) **umbrella / tracking / mega-issue** — a list of unrelated sub-issues or a self-described mega/tracking issue meant to be split into separate tickets (a multi-file docs update for *one* workflow is **not** this); (2) **unsettled design** — long debate or abandoned PR history with no maintainer-settled spec for *this* ticket; (3) **wish / product call** — a one-line feature request or product preference with no concrete acceptance criteria; (4) pure usage/support question; (5) maintainer says the fix needs core internals / large cross-cutting work. Labels like `good first issue` do not override these fails.

I wrote it this way after the cheap-run false reject on `issue-01`: “several docs files” must not equal “umbrella,” but tracking lists and wish-only tickets must still fail. Explicit fail bullets keep Claude from treating any multi-file body as automatic reject.

**Trade-offs**

That wording fixed `issue-01` (coherent docs package → accept) and left reject canaries `issue-10` / `issue-15` / `issue-20` agreeing on cheap re-checks. On the full run it still costs `issue-19` (false reject vs gold) and allows `issue-15` (false accept vs gold). I accepted those two disagreements rather than another full-run iteration: 18/20 already meets the bar, and further loosening would likely buy `issue-19` at the price of more scope false accepts.

---

## Selection rationale

**Selection rationale**

1. Fit to interests / time:
   Matches my fit profile (Python/backend-comfortable, docs/config hygiene, bounded ~1–2h). #73 is a concrete README vs `.env.example` disagreement on the LLM provider env var (`OPENROUTER_API_KEY` vs documented `mock`/`openai`), labeled `good first issue` / `tier-1`, with clear files and acceptance criteria — same shape as the AI201 dependency-scan chore I preferred (tooling/docs, not UI redesign).
2. What the verdict got right vs what you weighed by hand:
   Live skill correctly **accept**ed after reading scope/rubric, `gh` issue/repo facts, and `.env.example`. I independently wanted a small docs/config ticket that is unassigned and teaches the PR loop; the accept JSON matched that. I did not need to override the verdict.
3. Anticipated difficulty claiming it:
   Low for Unit 1: choosing is not claiming. Assignees empty and no open linked PR at grade time. Unit 2 claim/PR may still race classmates under Path Review house rules; credit follows the PR, so a shared issue is acceptable.

---

Related paths: `eval-run.txt` in this directory; skill files in `tools/issue-select/`; accept backup `live-transcript-issue-73-accept.txt`.
