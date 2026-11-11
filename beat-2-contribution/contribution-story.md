# Contribution: Zotero connector: index notes (`zotero:note`)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Yie Sheng Chen
**GitHub Username:** speculaas
**Issue:** https://github.com/nimbus-agent/Nimbus/issues/952
**Pull Request:** https://github.com/nimbus-agent/Nimbus/pull/1965

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: issue-scout over my unit 1 skill, four queries, one scan, and a rubric fix**

I installed the scout over my unit 1 skill and wrote `search-strategy.md` with four queries traced to my fit profile. The 2026-10-15 scan went 397 unique results to 255 after filters, and graded the top five (four accept, one reject). Its weak spots were that every query hit the 100-result cap, the sort did not separate the top five (all had 0 comments and the bug label), and sandbox repos leaked into the list. I logged dated lines in `scan-notes.md` and edited only one thing in the rubric: after the scout accepted finos/opengris-scaler#235 on a loose reading, I added a fail condition for an issue that is blocked on an open prerequisite. Most of my other fixes are still logged as proposed, not applied. On 2026-11-04 I retargeted the queries at upstreams of my own projects, because `good first issue` returned 0 results there.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I chose Nimbus #952 late, on 2026-11-10, after my first pick went away. My 2026-10-15 scan accepted automatos-ai#1059 and #1057 and my Issue Selection site picks included finos/opengris-scaler#235 (which my scout rejected because it is blocked on #233 and a maintainer said #951 should come first). I did not take any of those. So on 2026-11-10 I searched the whole Issue Selection site again, with a crawler script I wrote with an AI assistant (it only sends GET requests, never clicks Claim, and the code is in my `fall26-board` repo under `tools/`). I crawled all 2,233 Python, 2,372 JavaScript and 2,183 TypeScript listings, filtered by keyword and by the language and kind of work in my profile, checked about 26 candidates on GitHub for a linked pull request (read from the timeline's cross-references) or a claim comment, and kept two: apache/mahout#1468, which needs Linux and an NVIDIA GPU, and nimbus-agent/Nimbus#952. This is outside my original Python-only profile. I widened it to TypeScript because my project bib-citation-graph can use the same idea (indexing notes and references from a reference manager).

Why #952 fits: it is a small feature with a written spec and acceptance criteria, it has a fake-server test pattern to copy, it had 0 comments, no assignee, and no linked PR when I checked, and the dev setup is light (Bun, no GPU). I hope to learn how a connector pipeline maps raw API items into an index, and how a maintainer wants embedding and privacy decisions explained in a PR. My claim comment, posted 2026-11-10: https://github.com/nimbus-agent/Nimbus/issues/952#issuecomment-6788695823. I asked to be assigned and did not wait for an answer, because the repo's CONTRIBUTING says that outside October, being assigned first is "the courteous default, not a hard gate" and tells contributors to comment and open the PR anyway.

[TODO before submitting: run `claude "issue-scout: grade https://github.com/nimbus-agent/Nimbus/issues/952"` (about $0.20), paste the read-out into scan-output.md, and write its verdict here. As of this draft the scout has not graded #952, so I do not know what it would say. If it rejects #952, say so and why I kept it.]

Honest read of the tool: the scout did not find this issue. My own crawler did, because the scout's `gh search` queries only see what GitHub returns for a query and my scan hit the 100-result cap on every query. What I would change in the scout is logged in `scan-notes.md` (a per-repo cap, a 60-day date computed at scan time, and checking linked pull requests and claim comments from the timeline rather than from search fields).

**Toolkit**

- Tool: issue-scout (my search strategy and rubric) for the Unit 5 scan, plus a separate crawler script, `tools/site_crawl.py` and `tools/enrich_gh2.py` in my fall26-board repo, for the whole-site hunt that found #952.
- What it produced: the Unit 5 scan (4 accepts, 1 reject, none of them #952), then a crawl of 6,788 site listings that narrowed to 14 candidates, then 2, then #952.
- Difficulties: the first enrichment script (`enrich_gh.py`) read linked-PR state wrongly, so Galaxy #20579 (already fixed by a merged PR), GRASS #3252 and Leaflet #6904 (both with open PRs) looked available. I fixed it by reading the issue timeline's cross-referenced events in `enrich_gh2.py`.
- What changed: I now trust the timeline cross-references, not the search result's PR field, for "is someone already working on this". That is not yet in my rubric's not-already-taken check, so I am logging it in scan-notes.md.

---

## Understanding the Issue

**Problem Description**

Nimbus is a local-first AI agent. Its Zotero connector syncs a user's Zotero library into Nimbus's search index, but it only indexes bibliographic references. Zotero notes (the user's own reading notes, either attached to a paper or standalone) are skipped, so a search for something the user wrote about a paper finds nothing.

**Expected Behavior**

Each Zotero note is indexed as a read-only `zotero:note` item with a readable title and a plain-text body, and a child note records which paper it belongs to. Attachments stay skipped.

**Current Behavior**

In `zotero-reference-mapping.ts`, `SKIP_ITEM_TYPES` is `new Set(["attachment", "note"])`, so the mapper returns null for notes and the sync never writes them. An existing fake-server test pins this: "skips attachment + note item types" expects `itemsUpserted` to be 1.

**Affected Components**

`packages/gateway/src/connectors/zotero-reference-mapping.ts` and `zotero-sync.ts` (the mapper and the sync's `map` callback), a new `zotero-note-mapping.ts`, `src/embedding/routing.ts` (where the embedding and body-cap rules per item type live), and `docs/roadmap.md`. The tests are `test/unit/connectors/` and `test/integration/connectors/zotero-sync-fake-server.test.ts`.

**Toolkit**

- Tool: none for this section. I read the issue body and the code directly.
- What it produced: n/a
- Difficulties: none met
- What changed: nothing

---

## Reproduction Process

**Environment Setup**

macOS on a Mac mini (Apple silicon), Bun 1.4.3 (the repo asks for Bun 1.2 or newer). I made a shallow clone of nimbus-agent/Nimbus at commit `b088d05` (main when I started) and ran `bun install --ignore-scripts`. No GPU, database server, or Zotero account is needed, because the integration test runs against a fake Zotero API served by `Bun.serve`. Challenge: when I ran the whole test suite, 5 tests failed because a built worker file, `dist/workers/embedding-worker.js`, is missing, which is a build artifact I did not produce. The same failures show up without my change. A separate issue: the repo's `audit:js-licenses` check fails on macOS because the arm64 Mac package `@img/sharp-libvips-darwin-arm64` is LGPL-licensed. That is unrelated to my change, and I did not try to fix it.

**Steps to Reproduce**

This is a missing-feature issue, so the reproduction is to show that notes are dropped:

1. Check out `b088d05` and look at `packages/gateway/src/connectors/zotero-reference-mapping.ts`: line 11 is `const SKIP_ITEM_TYPES = new Set(["attachment", "note"])`, and line 94 returns null for any type in the set.
2. Run `bun test packages/gateway/test/integration/connectors/zotero-sync-fake-server.test.ts -t "skips attachment"`.
3. Observed result: the test passes. It feeds one reference, one attachment and one note through the sync and asserts that only the reference is indexed (`itemsUpserted` is 1), so the note is dropped.

**Reproduction Evidence**

- **Commit showing reproduction:** I did not make a separate reproduction commit, because the existing test already pins the behavior. I ran it on the unmodified `b088d05` on 2026-11-10 (1 pass, 0 fail). That is the upstream commit my branch starts from: https://github.com/nimbus-agent/Nimbus/commit/b088d05c650f80661e05bc91af769d1af783c164. My fix is in my fork: https://github.com/speculaas/Nimbus/commit/732a3b81924b32d0053fe4781830dd22e03bcf5a
- **Screenshots/logs:** none
- **My findings:** I did not run this against a real Zotero library. The fake-server test is the only end-to-end evidence I have for the sync path, and no real Zotero library has been tried with my change.

**Toolkit**

- Tool: none beyond Bun's test runner. I did not use a reproduce tool from the course for this issue.
- What it produced: the passing "skips attachment + note item types" test on the unmodified commit.
- Difficulties: the 5 failures in the full suite because of the missing `dist` worker file. I confirmed they also fail without my change.
- What changed: nothing in the repo for this.

---

## Solution Approach

**Analysis**

Two things block notes: `SKIP_ITEM_TYPES` makes the reference mapper return null for them, and `zotero-sync.ts` has a single `map` callback that always calls `mapZoteroReferenceToItem`. Even without the skip, a note would not index well: a note has no title field, only HTML in `data.note`, and the index gives an item type a long body only if its type is registered in `embedding/routing.ts` (the `LONG_BODY_TYPES` in `index/body-caps.ts` is built from two sets there), otherwise the body is cut to 512 characters.

**Proposed Solution**

Add a sibling mapper, `zotero-note-mapping.ts`, that turns the note HTML into plain text with the repo's existing `html-plain-text.ts` helpers, takes the first line as the title, and stores `data.parentItem` as `metadata.parent_key`. Route each raw item by `data.itemType` in `zotero-sync.ts`. Register `zotero:note` in `LOCAL_ONLY_PROSE_TYPES`, so a note gets the long body cap but is embedded on-device only. I chose that because a note is the user's own writing and should not be sent to a remote embedder. I am deliberately not changing attachment handling, the pagination or the `MAX_PAGES` cap, the connector catalog, or secrets and rate-limit files, and I am not adding the optional `belongs_to` graph edge in this PR.

**Implementation Plan**

1. Create `zotero-note-mapping.ts` (title, body, `parent_key`, skip a missing or empty key).
2. Route by `itemType` in `zotero-sync.ts`.
3. Keep the reference mapper returning null for notes, so a note can never become a `reference` row.
4. Add `zotero:note` to `LOCAL_ONLY_PROSE_TYPES`.
5. Update the fake-server test deliberately (note indexed, attachment still skipped), and add `zotero-note-mapping.test.ts`.
6. Update the Zotero row in `docs/roadmap.md`.

Deviations: the issue said to remove `"note"` from `SKIP_ITEM_TYPES`. I kept it in that set, because otherwise the reference mapper would turn a note into a `reference` row, and the sync's dispatch already sends notes to the new mapper. I also left out the stretch goal (the `belongs_to` edge) and said so in the PR as a follow-up.

**Toolkit**

- Tool: an AI coding assistant (Grok Bot) read the connector, mapper, routing and body-cap files with me and wrote the first version of the code and tests, and I reviewed the diff. The repo's own `.claude` guides on index body depth and embedding routing were my reference.
- What it produced: the plan above and the first working mapper.
- Difficulties: the issue text said to remove `"note"` from the skip set, but doing that alone would have been a bug. I found that by reading the routing and body-cap code.
- What changed: I kept the skip in the reference mapper and added a comment explaining why.

---

## Testing Strategy

Before: on `b088d05` the fake-server test "skips attachment + note item types" passed, with `itemsUpserted` equal to 1 (the note was dropped).

After: I replaced it with "indexes notes as zotero:note, still skips attachments". With one reference, one attachment and one note (with a parent and HTML body) it now expects `itemsUpserted` to be 2, rows `ABC1` (type `reference`) and `NOTE1` (type `note`), a title of `Key idea & caveat`, and a body of `Key idea & caveat Second paragraph`. The attachment is not indexed. The new unit test `zotero-note-mapping.test.ts` covers a child note, a standalone note (`parent_key` null), an HTML body whose derived title has no markup, a long first line, an empty note, a missing or empty key, non-note items, and that `zotero:note` gets the 16,384-character body cap and is not prose-heavy (stays off the remote embedder).

Repo checks, run locally on macOS: `bun run typecheck` is clean; `bun run preflight:fast` passed every gate except `audit:js-licenses`, which fails on the macOS-only LGPL package described above and is unrelated to my change; the Zotero, routing and body-cap tests pass (74 tests, 0 fail). I started a full `bun test` from the repo root, but it also runs the UI package, where dozens of component tests (WatcherPane, QuickQuery and others) time out at 5 seconds on my Mac, so I stopped it and did not get a clean whole-repo result. I have not shown a full gateway-package run either; the Zotero-related tests above are the ones I can vouch for.

**Toolkit**

- Tool: Bun's test runner, Biome (via `bun run lint` and `preflight:fast`), and the repo's `preflight:fast` script.
- What it produced: the results above.
- Difficulties: the license audit and the missing `dist` worker failures are environment-specific, so I cannot show a fully green run on my Mac. The CI on the PR is the real check.
- What changed: nothing in my change. I wrote the failures down in the PR instead of hiding them.

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-11-10**

Crawled the Issue Selection site (GET only, no Claim), narrowed to Nimbus #952 and apache/mahout#1468 (backup, needs a GPU), and chose #952 because it needs no GPU and has a written spec. I did not decide on the embedding behavior until I had read `routing.ts` and `body-caps.ts`: a note needs to be in a long-body set to keep its text, and the safe choice for private notes is the local-only set.

**2026-11-10 (evening)**

Posted my comment on #952 asking to be assigned and describing the plan, then wrote the mapper, the dispatch, the routing change, the tests and the roadmap row, in one local commit. Ran typecheck, Biome and preflight, and opened PR #1965. The CLA bot failed the `cla` check until I signed the Individual CLA by comment, and it passed without a `recheck`.

**Toolkit**

- Tool: an AI coding assistant for reading the code and writing the first version, and `gh` for checking the issue and PR state.
- What it produced: the commit, the PR text, and the status checks above.
- Difficulties: the sandbox-style claim rules I relied on in Unit 5 do not apply here. The maintainer had not answered when I opened the PR.
- What changed: I recorded in the PR that I used an AI assistant, and the PR states the embedding decision up front.

---

## Pull Request

**PR Link:** https://github.com/nimbus-agent/Nimbus/pull/1965

**PR Description:** Titled "feat(gateway): index Zotero notes as zotero:note (#952)", "Closes #952". It lists what the mapper does and how the sync routes by `itemType`, and states the embedding decision (`zotero:note` is local-only, with the long 16 KB body cap) and that notes and references now share the 2,000-item cap of the `MAX_PAGES = 20` walk. It follows the repo's PR template and leaves two checklist boxes unticked on purpose (full lint and all tests) at the time I opened it. Notes for reviewers: no `belongs_to` graph edge yet (follow-up), the nimbus-mcp-servers README still says notes are skipped (a separate follow-up PR), and a one-line disclosure that I used an AI coding assistant.

**Maintainer Feedback:**

- 2026-11-10: none yet. The `cla` check failed at first and passed after I signed the CLA. The title lint and labeller passed, and CodeRabbit skipped itself because the repo has fewer than 10 stars.
- [Add the date and how you addressed each review comment, when one arrives.]

**Status:** Awaiting review.

**Toolkit**

- Tool: `gh` and the repo's PR template.
- What it produced: PR #1965 with passing CLA, title and label checks.
- Difficulties: the first PR comment from a bot is a CLA request, and signing it is a legal decision, so I made it myself.
- What changed: nothing yet.

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
