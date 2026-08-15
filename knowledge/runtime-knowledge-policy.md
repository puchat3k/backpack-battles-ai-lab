# Runtime knowledge policy

Version: KB-0.3-staging
Canonical game patch: 1.1.8

## Objective

Live play should resolve mechanics from a patch-frozen persistent reference before decision reasoning begins. Unknown decision-material mechanics must not be guessed.

Runtime path:

`screenshot -> state extraction -> persistent mechanics cache -> bounded external validation if needed -> minimum operator query if still unresolved -> DM reasoning -> operator command`

## Source precedence

1. Explicit in-game tooltip supplied during the run.
2. Patch 1.1.8 notes supplied for this experiment.
3. Patch-frozen persistent mechanics cache in `knowledge/mechanics-1.1.8.json` and maintained local references.
4. Current maintained external references, preferably Backpack Battles Wiki and other patch-current sources.
5. Other external/community sources with provenance.
6. Model memory is never authoritative for an exact mechanic.

## Unknown-mechanic rule

If an item, skill, recipe, combination, or mechanic is recognized but absent or uncertain in the persistent cache, mark it `UNKNOWN`.

Do not infer exact effects from visual design, item name, remembered historical versions, build archetype, or apparent star geometry.

If the unknown is decision-material:
1. Search external sources autonomously first.
2. Cross-check patch/version when conflicting values appear.
3. Persist verified mechanics and provenance into the patch-frozen cache so the same lookup is not required again.
4. If external research cannot resolve it with adequate confidence within the live research budget, stop searching and ask the operator for the minimum in-game tooltip/screenshot required.

## CEO/operator time cap

External validation is preferred over consuming operator time. The operator is not expected to explain mechanics, enumerate options, or repair the knowledge base. Requests for screenshots/tooltips are a fallback after autonomous lookup fails, and should be batched where possible.

Voluntary operator intervention remains useful evidence, especially for known model blind spots such as recipe/combinatorial option detection. It does not remove engine ownership of the decision unless explicitly stated as an override.

## Live research budget [STAGING]

External validation during live gameplay is bounded.

- Cache hit: no external search.
- Decision-material cache miss: target resolution within approximately 10 seconds.
- Hard operating budget: approximately 15 seconds of search effort.
- The budget is an operational ceiling rather than a guaranteed deterministic wall-clock timeout because external tool latency is not fully controllable.
- If sufficiently reliable, patch-current evidence is found within budget: use it, record provenance, and persist it to the cache.
- If evidence remains conflicting, weak, or unresolved at the budget: terminate research rather than continuing to search. Request the minimum operator tooltip/screenshot required.
- If the unknown is not decision-material: do not spend the research budget. Continue the gameplay loop.
- Repeated research for a previously resolved mechanic is a cache failure and should be treated as such.

Purpose: protect live-game latency and CEO/operator time while preventing unsupported mechanics guesses.

## Patch overlay

The base catalogue is frozen for a game patch. Explicit patch notes and in-game tooltips override older catalogue values. A later patch creates a new KB version rather than silently mutating historical data.

## Live latency rule

Ordinary decisions should hit the persistent cache. External web/API lookup is permitted during live play when a decision-material mechanic is missing or uncertain. This is an exception path, not the default path. Once resolved, cache the result.

## Provenance

Every exact mechanic in the cache carries a source label and verification date. Conflicting external values should be recorded or resolved against the current patch before use.
