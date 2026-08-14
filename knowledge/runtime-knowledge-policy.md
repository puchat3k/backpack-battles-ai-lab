# Runtime knowledge policy

Version: KB-0.1
Canonical game patch: 1.1.8

## Objective

Live play must not depend on external web lookups. Item mechanics should be resolved from a patch-frozen local reference before decision reasoning begins.

Runtime path:

`screenshot -> state extraction -> local item/mechanics cache -> DM reasoning -> operator command`

Neon logging, Drive capture storage, GitHub publication, and external research are outside the critical live-decision path.

## Source precedence

1. Explicit in-game tooltip supplied during the run.
2. Patch 1.1.8 notes supplied for this experiment.
3. Patch-frozen structured item reference compiled before live play.
4. Community/reference sources used during offline maintenance only.
5. Model memory is never authoritative for an exact mechanic.

## Unknown-mechanic rule

If an item is recognized but its mechanic is absent or uncertain in the local cache, mark the mechanic `UNKNOWN`.

Do not infer exact effects from:
- visual design
- item name
- remembered historical versions
- build archetype
- apparent star geometry

If the unknown mechanic is decision-material, request a tooltip rather than inventing a mechanic.

## Patch overlay

The base catalogue is frozen for a game patch. Explicit patch notes override older catalogue values. A later patch creates a new KB version rather than silently mutating historical data.

## Live latency rule

No external web/API lookup should be required for an ordinary shop decision. External research is a maintenance operation between runs or during an explicitly paused run.

## Provenance

Every exact mechanic in the cache should carry a source label such as `in_game_tooltip`, `patch_notes_1.1.8`, `bpb_builds_snapshot`, or `wiki_snapshot`.
