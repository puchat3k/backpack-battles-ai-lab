# Decision Model DM-0.6 — STAGING

**Status:** staging / not active  
**Base model:** DM-0.5  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged

## Purpose

This staging document captures operational and decision-process observations from the current Ranger autonomous-mode run. It is intentionally incremental. It does not propose a new software architecture, database pipeline, or autonomous game client.

## 1. Idle-capital / deployed-capital accounting

A material failure was observed by approximately Round 4: a large amount of purchased value remained in storage while the deployed board was still losing decisively.

Add explicit separation between:

- displayed gold;
- stored liquidation value;
- stored strategic option value;
- **deployed combat capital**;
- stranded/idle purchased capital.

Purchasing an item does not create combat value unless it is deployed, stages a near-term combination, or has a deliberate economy/option role.

Candidate diagnostic:

`IdleCapitalRatio = stored purchased value without near-term purpose / total acquired item value`

Do not assign false precision yet. Use the concept to detect when the engine is accumulating assets faster than it can convert them into board strength.

## 2. Shop-phase transaction planning

The engine has been too item-local. Replace repeated `see item -> buy item` reasoning with a shop-phase plan.

Before the first material purchase in a shop, evaluate the reachable transaction set across:

- current board;
- storage;
- known combinations and recipe graph;
- available shop items;
- expected displacement;
- reroll budget;
- minimum liquidity reserve;
- current binding combat constraint.

Prefer issuing one coherent action batch to a series of locally attractive purchases.

A purchase should be evaluated on **incremental deployed-board value** and reachable transformations, not nominal item quality alone.

## 3. Eviction hierarchy

Maintain an internal, state-dependent ranking of currently deployed items by marginal contribution.

When a purchase requires space, the engine should already know likely displacement candidates. `Deploy best-fit` remains acceptable for routine geometry, but the strategic eviction hierarchy is engine-owned.

If a new purchase cannot beat the marginal value of the items it would displace, do not buy it merely because it is efficient in isolation.

## 4. Combination-first audit

Before issuing a purchase, reroll, or combat instruction, check whether any combination or recipe involving:

- owned board items;
- stored items;
- current shop items;
- staged components

creates a materially stronger reachable state.

The broom/pan exchange is retained as a regression case for missed combinatorial action space.

The persistent mechanics cache should increasingly include recipes/combinations, not only standalone item tooltips.

## 5. Autonomous placement authority

When `buy` is recommended, **deployment is implied when legal and strategically intended**.

If the board is spatially constrained:

- Player may perform routine best-guess packing without another screenshot;
- the engine must specify exact displacement/placement only when adjacency, stars, activation, combination setup, or eviction choice materially affects the strategic decision;
- routine packing should not create an observation boundary.

## 6. Action queue semantics

Deterministic commands are cumulative until explicitly superseded.

Example:

`buy Garlic -> buy Lamp -> reroll`

means both purchases remain intended unless the engine explicitly cancels or replaces an earlier instruction.

If two deployments genuinely cannot coexist and no exact displacement instruction was given, the engine should prevent the conflict before issuing the batch rather than relying on implicit recency precedence.

## 7. Voluntary Player guidance

Autonomous Mode means **zero required Player guidance**, not zero permitted guidance.

Player may voluntarily flag an apparent blunder, missing mechanic, recipe, or strategic consideration. Such input is additional evidence and should trigger independent recomputation.

It does not automatically become ground truth, and it does not transfer decision ownership unless Player explicitly overrides the action.

The system must remain usable if Player provides screenshots only.

## 8. Bounded external validation

Retain the KB-0.3 staging policy:

- local cache first;
- decision-material cache miss -> bounded external lookup;
- target roughly 10 seconds, operating ceiling roughly 15 seconds;
- stop when evidence remains unresolved;
- ask Player for the minimum tooltip/screenshot only after bounded research fails;
- persist verified mechanics so repeated lookup is unnecessary.

## 9. Battle-result severity

Treat combat outcomes as more than binary win/loss.

Classify at minimum:

- decisive win;
- close win;
- close loss;
- decisive loss.

A decisive loss should trigger a broader board-strength/binding-constraint reassessment before continuing incremental purchases. Do not respond to a structural loss merely by increasing a generic tempo preference.

## 10. Privacy-by-design firewall

Public BPB terminology uses **Player** for the human participant.

Prohibited public cross-context identifiers include unrelated project names, professional titles/callsigns, real-world identity, private operational terminology, customer/confidential data, and unrelated personal context.

Pre-publication scrub must check both content and metadata/filenames for cross-context contamination.

Internal conversational terminology from unrelated work must never be propagated into BPB public artifacts.

## 11. Architecture decision: do not overbuild yet

The current failures do **not** justify a dramatic architecture rewrite.

A purpose-built state engine, OCR/data-ingest pipeline, or full database could solve many bookkeeping problems, but that would materially change the scope of a lightweight side experiment.

Current minimum sufficient architecture remains:

- screenshot input;
- persistent patch-frozen mechanics/recipe cache;
- versioned decision model;
- concise per-run logs;
- explicit state/economy reconciliation;
- bounded external validation.

Reconsider heavier ingestion/state infrastructure only if repeated runs show that state-loss or combinatorial bookkeeping remains the dominant performance bottleneck after these lower-cost fixes.

## Promotion gate

Promote only after review at the next housekeeping/debrief point. Preserve DM-0.5 as the active model until then.
