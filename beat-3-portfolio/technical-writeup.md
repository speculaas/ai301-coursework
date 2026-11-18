# Zotero Notes Now Get Indexed as Searchable Local Items in Nimbus

## Overview

[Nimbus](https://github.com/nimbus-agent/Nimbus) is a local-first AI agent whose Zotero connector syncs a user's reference library into Nimbus's search index. In issue [#952](https://github.com/nimbus-agent/Nimbus/issues/952), one line in `SKIP_ITEM_TYPES` (`new Set(["attachment", "note"])`) threw away all Zotero reading notes, so a search for notes you wrote about a paper returned nothing. I claimed the issue, built the `zotero-note-mapping.ts` mapper, routed `note` items in `zotero-sync.ts`, and registered `zotero:note` in `LOCAL_ONLY_PROSE_TYPES` so notes keep a 16,384 UTF-16 code unit body cap while staying on the local embedder. Alongside the contribution, I patched my `issue-scout` rubric to separate feasibility from prerequisite blockers. [PR #1965](https://github.com/nimbus-agent/Nimbus/pull/1965) merged on November 16, 2026.

## What happened

### Oct 15 – Nov 10: Finding the issue

My Unit 5 `issue-scout` scan (Oct 15) missed #952 because every `gh search` query hit GitHub's 100-result cap (397 unique results, 255 after filters). On Nov 10, I crawled 6,788 Issue Selection listings across Python, JavaScript, and TypeScript, then enriched candidates using timeline `cross-referenced` events (`enrich_gh2.py`) rather than search fields. It surfaced `nimbus-agent/Nimbus#952`.

### Nov 10: Claim and build

At 11:20 PM MST on Nov 10, I posted my claim on [#952](https://github.com/nimbus-agent/Nimbus/issues/952#issuecomment-6788695823):

> "My plan: add a `zotero-note-mapping.ts` sibling mapper (read-only, `hitlRequired: []`), drop `"note"` from `SKIP_ITEM_TYPES` while leaving `attachment` skipped, and dispatch on `data.itemType` in the `map` callback of `zotero-sync.ts`."

Reading `routing.ts` and `body-caps.ts` changed that plan: simply removing `"note"` from `SKIP_ITEM_TYPES` in `zotero-reference-mapping.ts` would have turned notes into `reference` rows. Instead, I kept `"note"` in `SKIP_ITEM_TYPES` with a comment and routed notes in `zotero-sync.ts` to `zotero-note-mapping.ts`, committing the implementation and tests in `732a3b8`:

```diff
--- a/packages/gateway/src/connectors/zotero-reference-mapping.ts
+++ b/packages/gateway/src/connectors/zotero-reference-mapping.ts
+// Attachments stay unindexed. Notes are NOT dropped any more: `zotero-note-mapping.ts` maps them,
+// and `zotero-sync.ts` routes by `itemType`. This reference mapper still returns null for a note so
+// a note can never be indexed as a `reference` row.
 const SKIP_ITEM_TYPES: ReadonlySet<string> = new Set(["attachment", "note"]);
```

The updated fake-server integration test (`packages/gateway/test/integration/connectors/zotero-sync-fake-server.test.ts`) verified that notes index with plain-text titles while attachments stay skipped:

```ts
expect(result.itemsUpserted).toBe(2);
expect(rows.map((r) => [r.external_id, r.type])).toEqual([
  ["ABC1", "reference"],
  ["NOTE1", "note"],
]);
const note = rows.find((r) => r.external_id === "NOTE1");
expect(note?.title).toBe("Key idea & caveat");
expect(note?.body).toBe("Key idea & caveat Second paragraph");
```

I opened [PR #1965](https://github.com/nimbus-agent/Nimbus/pull/1965) at 11:39 PM MST and signed the Individual CLA so the `cla` check passed.

### Nov 11: My own change (Unit 6 rubric patch)

On Nov 11, I graded #952 twice with `issue-scout` and applied the Unit 6 rubric patch between runs. Before the patch, `scope-fits-newcomer` bundled scoping with prerequisite checks; after the patch, it split into `scope-feasible` and `not-blocked` plus a difficulty note:

```diff
-| scope-fits-newcomer | Issue title + body + labels + comment thread | **Pass** if the ask is **one coherent first contribution**... **Fail** if any of these hold: ... (6) **blocked on an open prerequisite** ... | required |
+| scope-feasible | Issue body: the work it asks for | The issue describes one identifiable piece of work someone could start... An open design edge, an "etc.", or several files involved never fails this check; that raises the difficulty note instead. | required |
+| not-blocked | Issue body + maintainer comments | **Pass** unless the issue body or a maintainer comment says it depends on another issue or PR that is still open and not implemented... | required |
```

Comparing the two grades (`scan-output.md`):
- **12:11 AM MST (pre-patch):** `reject` — failed only `not-already-taken` (because it saw my own open PR #1965); `scope-fits-newcomer` passed, with no difficulty note.
- **12:37 AM MST (post-patch):** `reject` — still failed only `not-already-taken` for my own PR, while `scope-feasible` and `not-blocked` both passed and the read-out added `Difficulty note: moderate` (flagging the embedding/body-depth privacy choice).

### Nov 11 – Nov 12: Review

CodeRabbit flagged two gaps in `zotero-note-mapping.ts`:
- `"Preserve text adjacency across inline tags."` (`<p>H<sub>2</sub>O</p>` became `H 2 O`, splitting searchable words).
- `"Decode numeric HTML character references."` (`caf&#233; &#x3B1;` stayed encoded).

On Nov 12, maintainer `asafgolombek` reviewed the PR:

> "Thanks a lot for this PR! It's carefully scoped. Keeping the reference mapper rejecting notes, storing `parent_key` for a future graph edge, and putting notes in `LOCAL_ONLY_PROSE_TYPES` were all good choices. Keeping notes on the local embedder is the right default, so please keep it."

He asked for both CodeRabbit fixes and a small `docs/roadmap.md` wording tweak before merge.

### Nov 16: The result that landed

On Nov 16, `asafgolombek` pushed the fixes directly to my branch in commits `b76d78f18` (a note-specific `stripNoteTags` so inline tags don't split words, numeric entity decoding, and the roadmap wording) and `294c9a663` (attribute-safe tag regex and surrogate-safe title truncation), then merged PR #1965:

> "I went ahead and pushed the two changes from my review to your branch (b76d78f18), so this can merge without another round trip... Your authorship stays on the PR."

## Limitations and what's next

My initial mapper had real gaps that the maintainer had to fix before merge: it split words at inline HTML tags (`<sub>`, `<em>`), did not decode numeric HTML entities, and was only tested against a fake Zotero server rather than a live Zotero library. Attachments and the optional `belongs_to` graph edge also remain out of scope, and I installed `voice-guard` and `ship-gate` after opening the PR, so they did not gate the original posts.

Next, I will add inline-tag adjacency, numeric HTML entities, and UTF-16 boundary cases to my connector test checklist before opening a PR, install repo guards before posting, and open the `belongs_to` graph-edge follow-up PR that `asafgolombek` invited.
