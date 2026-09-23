# Rubric: is this a good first issue?

<!--
Student-authored checks for AI301 Unit 1 issue-select.
Covers the four lecture families (maintainer alive, repo in use, newcomer
scope, not already taken) plus AI contribution policy (course workflow is
AI-assisted). Thresholds use the bundle "captured" date in eval mode and
today in live mode.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: `last 5 default-branch commits` and `maintainer first-response sample` (live: default-branch history + recent issue reply latency; see `references/evidence-guide.md`) | **Pass** if at least one of the last 5 default-branch commits is within **90 days** of the capture/today date, **or** at least one sample shows a numeric maintainer (Owner/Member/Collaborator) first-response ≤ **45 days**. **Fail** if the repo is archived, or if every listed commit is older than 90 days **and** every sample line is “no maintainer comment” / has no numeric response. | required |
| repo-in-use | Repo facts: `archived`, `last push to any branch`, `latest release` (live: archive banner, Releases sidebar, newest push) | **Pass** if `archived` is no/false **and** (`last push to any branch` within **180 days** **or** a `latest release` date within **180 days**). **Fail** if archived, or if both push and release (when present) are older than 180 days with no other activity signal in the facts block. Missing release alone does not fail if push is recent (commits can prove use without releases). | required |
| scope-fits-newcomer | Issue title + body + labels + comment thread | **Pass** if the ask is **one coherent first contribution**: a single bounded change (docs, tests, small bugfix, one UI/API behavior, or a concrete config/CI tweak) **or** a docs/update package that still serves **one feature or workflow** (e.g. add one permanent docs page and update a few related pointers/pages). Need enough detail to start (repro, acceptance criteria, or concrete proposed changes). Terse writeups can still pass when the work is clearly sized. **Fail** if any of these hold: (1) **umbrella / tracking / mega-issue** — a list of unrelated sub-issues or a self-described mega/tracking issue meant to be split into separate tickets (a multi-file docs update for *one* workflow is **not** this); (2) **unsettled design** — long debate or abandoned PR history with no maintainer-settled spec for *this* ticket; (3) **wish / product call** — a one-line feature request or product preference with no concrete acceptance criteria; (4) pure usage/support question; (5) maintainer says the fix needs core internals / large cross-cutting work. Labels like `good first issue` do not override these fails. | required |
| not-already-taken | Repo facts `this issue: assignees` / `linked PRs` + Comments (live: Assignees + Development + thread) | **Pass** if assignees are none/empty **and** there is no **open** linked PR. Closed or merged linked PRs alone do not fail. Stale “I’ll take it” comments without an assignee/open PR do not fail. In Path Review **live** mode, classmate claim comments do not fail (house rule in `scope.md`). **Fail** if assigned, or any linked PR is **open**, or a maintainer-confirmed active claim is clearly in progress with an open PR/assignee. | required |
| ai-policy-ok | Repo facts `contribution policy` (live: `CONTRIBUTING.md` / AI policy docs) | **Pass** unless the policy **explicitly bans** AI-generated or AI-assisted contributions. Disclosure / “understand and test your changes” conditions still **pass**. Silence **passes**. | required |
| good-first-signal | Labels and opener role | **Preferred** if labeled `good first issue` / `help wanted` / `docs`, or filed by a maintainer/member with a concrete ask. Never changes accept/reject; only helps rank accepted live candidates. | preferred |

## Verdict rule

**Accept** only if every **required** check is `pass`.

Treat `unclear` as **fail** for required checks (a first issue you cannot verify is not one you should take).

**Preferred** checks never change the verdict; on an accepted issue, use them only to rank live candidates against each other.
