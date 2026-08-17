# Decision Model DM-0.9

**Status:** active  
**Base model:** DM-0.8  
**Patch context:** 1.1.8  
**Default live-play interface:** BPB Autonomous Mode

## Why DM-0.9 exists

A Reaper run exposed a persistent execution failure. The engine could establish a sound strategic direction, but then repeatedly reopened that decision when a locally attractive shop item appeared. It also accumulated undeployed inventory whose nominal resale value obscured the fact that its strategic option value was decaying as the run progressed.

DM-0.9 adds strategy commitment, dynamic inventory liquidation, and a prior-first decision loop. The shop is no longer the starting point for strategic reasoning. Validated priors generate scalable build paths first; the current board determines feasibility and commitment; the shop is then interrogated for actions that advance those paths.

## 1. Governing order of operations

Default decision order:

`validated priors → scalable build paths/shared backbones → current board/state → build needs → shop actions → fallback alternatives`

The shop proposes actions. The existing strategy and reconciled state evaluate them. The shop does not reset the strategy.

The objective is not to force a predetermined build. It is to maximize expected repeatable scaling subject to the items actually offered.

## 2. Prior-first build path selection

At run start and major strategic transitions, identify the best-supported candidate build paths from validated priors before optimizing individual shop items.

Prefer paths with high repeatable scaling potential rather than merely the strongest currently visible item.

Maintain multiple plausible paths while evidence is weak. Shop RNG and board development update their relative attractiveness. As one path accumulates the necessary pieces and enablers, commitment increases.

If all prior-backed paths are weak or unavailable, evaluate the best generic tempo/economy alternatives rather than forcing a build.

## 3. Shared build backbones and branch optionality

Candidate priors must not be modeled as isolated archetypes. Multiple scalable paths may share early components, recipes, infrastructure, stamina support, bags, sustain, or other enabling pieces.

Represent pathing conceptually as:

`prior set → shared backbone → branch points → mature builds`

Classify important path items as:

- **Shared backbone:** advances multiple viable scalable paths and preserves optionality.
- **Branch item:** disproportionately strengthens one path and begins narrowing alternatives.
- **Commitment item:** materially locks resources/space into one path or closes competing branches.
- **Dead-end item:** has little value outside a weak, unsupported, or obsolete branch.

Shared-backbone items can deserve higher early priority than equally strong single-path items because they simultaneously create tempo/path progress and preserve future strategic options.

Do not double-count shared-backbone value. Its advantage is cross-path optionality, not multiple copies of the same combat contribution.

As evidence accumulates, the value of broad optionality should decline relative to completing the strongest branch. The engine should not remain permanently uncommitted merely because shared pieces exist.

## 4. Human-in-the-loop prior calibration

At the start of a run, and whenever the preferred build path materially changes, surface the intended prior set to the Player before relying heavily on it.

Compact format:

**Leading build prior:** [path/archetype]

**Adjacent priors sharing the backbone:** [paths, if material]

**Why:** [brief scaling/fit rationale]

**Shared backbone I currently believe matters:**
- item / recipe component / infrastructure
- item / recipe component / infrastructure

**Likely branch/commitment pieces:**
- item → path
- item → path

**Uncertain/missing:** [only where material]

The engine chooses the candidate prior set. The Player acts as a low-cost knowledge-quality check, correcting missing, wrongly weighted, or misunderstood dependencies and cross-path relationships. Player correction does not mean the Player chooses the strategy by default.

Once calibrated, the resulting dependency assumptions become the working build map for the run until evidence materially changes them.

This mechanism exists because prior-first play has greater reliance on accurate item combinations, build pathing, recipes, subclass dependencies, stamina, space, enabling infrastructure, and overlap between viable paths than purely local shop optimization.

## 5. Dependency-map reasoning

For each serious build path, reason in terms of:

`current pieces → shared backbone/enabling items → branch items → combinations/transitions → mature board`

Relevant dependency types include:

- core build items;
- shared cross-path components;
- recipe components;
- enabling bags/space;
- stamina/economy requirements;
- sustain/defense requirements;
- subclass dependencies;
- bridge items that preserve tempo while pathing;
- credible substitutes;
- branch and commitment points.

Do not pretend the dependency map is complete when it is not.

If a potentially important dependency or cross-path relationship is unknown, request Player calibration or retrieve/verify the mechanic before making a high-impact commitment.

## 6. Shop classification against the build map

After the path set and current needs are established, classify relevant shop actions conceptually as:

1. **Shared backbone:** advances multiple viable prior-backed paths.
2. **Core:** directly part of the leading intended mature build.
3. **Enabler:** required infrastructure, ingredient, space, stamina, or support.
4. **Bridge:** immediate deployable tempo that keeps the run viable while pathing.
5. **Branch/commitment signal:** strengthens one path enough to justify narrowing alternatives.
6. **Alternative-path signal:** sufficiently strong evidence that another scalable path may now dominate.
7. **Noise:** does not materially advance the path set or current survival requirement.

Shared-backbone/core/enabler items should receive greater salience when the existing strategy predicts their importance. Random cheap/sale items should receive less salience merely because they are visible.

## 7. Strategy commitment states

A run-level strategy may occupy one of three states:

1. **Hypothesis**. Plausible direction, weak evidence, easily overridden.
2. **Preferred line**. Supported by board state and mechanics, should guide purchases unless meaningful contrary evidence appears.
3. **Committed line**. Supported strongly enough that it becomes a decision constraint rather than merely another shop-scoring input.

Promotion must be evidence-based. Do not commit to a line merely because it is novel or interesting.

Early shared-backbone accumulation may intentionally delay commitment when several high-scaling branches remain live. This is distinct from indecision. Once branch evidence becomes sufficiently strong, complete the best branch rather than preserving optionality for its own sake.

## 8. Commitment enforcement

Once a line is committed, every proposed transaction must answer internally:

- Does this action strengthen the committed line now?
- Does it enable a known near-term transition within that line?
- Does it solve a critical weakness that the committed line cannot otherwise solve?
- If none apply, what evidence justifies overriding the strategy?

A locally attractive sale, rarity, generic value item, or speculative synergy does not by itself defeat a committed strategy.

The engine must not silently reopen a settled strategic question on every shop roll.

## 9. Critical-state commitment tightening

As lives decrease, combat losses become severe, or the board falls materially behind expected power, strategic discipline increases.

In critical states:

- prioritize immediate deployable board strength;
- raise the threshold for speculative purchases and strategic pivots;
- reduce tolerance for undeployed inventory and economy-only purchases;
- require stronger evidence before overriding a committed line;
- reduce the premium assigned to cross-path optionality when immediate branch completion is needed;
- prefer actions that directly address the observed combat deficit.

Critical state does not mean blindly preserving a losing strategy. A pivot remains valid when evidence shows the committed line is structurally failing and a credible alternative is executable.

## 10. Inventory is not static wealth

Storage value must not be evaluated only by purchase price or resale value.

An undeployed item has no immediate combat contribution. Its value is dynamic:

`hold value = resale value + remaining option value - congestion cost - tempo opportunity cost`

Where:

- **resale value** is recoverable gold;
- **remaining option value** is the probability-weighted value of future deployment, combination, trade, or strategic use across still-viable branches;
- **congestion cost** reflects storage/board-space pressure and increased transaction complexity;
- **tempo opportunity cost** reflects combat power forgone by leaving capital undeployed.

This is a conceptual decision rule, not a requirement for exact numerical calculation.

## 11. Inventory option-value decay

Remaining option value generally declines with:

- later round number;
- fewer remaining shops/lives;
- stronger commitment to a build that does not use the item;
- collapse of adjacent branches that previously shared the item;
- repeated failure to find the item's required enabler;
- increasing storage congestion;
- urgent need for deployable combat power;
- availability of a clearly superior use for the recoverable gold.

Therefore a 1g speculative item in Round 1 can be rational to hold while the same item in Round 10 may be economically stale even if its resale price is unchanged.

Nominal gold value is not strategic value.

## 12. Liquidation trigger

At every shop, especially midgame and later, explicitly evaluate stored items for sale.

A stored item becomes a liquidation candidate when:

- it has no credible near-term deployment path;
- it is not part of a likely recipe, shared backbone, or committed synergy;
- branches that gave it option value are no longer viable;
- its required enabler has repeatedly failed to appear;
- storage congestion is material;
- selling it funds an immediate deployable upgrade, reroll, or committed-line purchase;
- the run is critical and optionality is less valuable than current combat power.

Late-game default:

> Either the item has a credible near-term route onto the board, contributes to a known recipe/committed line/live adjacent branch, or it should face strong sale pressure.

Do not retain stale items merely because selling them feels like abandoning prior investment.

## 13. Phase-dependent inventory tolerance

**Early game:** tolerate experimentation and shared-backbone positions when tempo remains adequate.

**Midgame:** prune inventory and branches that are not converging toward the strongest scalable builds.

**Late/critical game:** become hostile to undeployed capital and excessive optionality. Convert stale inventory into gold and gold into immediate board strength whenever credible opportunities exist.

## 14. Pre-answer contradiction check

Immediately before issuing a live recommendation, check:

> Does this action contradict the strategy, live branch set, space constraints, storage state, tempo requirement, dependency map, or known synergies that the engine has already established?

If yes, either reject the action or explicitly identify the new evidence that justifies the override.

This check is mandatory in critical board states.

## 15. Regression case. Reaper Stone Badge/Stoned run

Observed failures included:

- establishing a coherent Stone Badge + Stoned direction, then repeatedly recommending unrelated cheap/sale items;
- missing Bag of Stones in storage even though it was the key stone enabler;
- recommending a jewel as if it were a Stone and buying based on false synergy;
- accumulating roughly 5g of stale storage inventory while combat power lagged;
- recommending another speculative 4g purchase after Hungry Blade had finally converted gold into immediate board strength;
- recommending Hexblade at subclass selection without verified mechanics and despite stronger existing Hungry Blade/Vampiress linkage.

The run ultimately suffered a catastrophic loss after the Hungry Blade upgrade and was surrendered.

Failure classification:

**strategy-execution drift plus failure to discount aging undeployed inventory.**

Mandatory lessons:

> Once a strategy is established on reliable evidence, enforce it strongly enough that local shop salience cannot casually override it.

> Stored items are not timeless assets. Their option value decays, and stale inventory should be liquidated when current combat power becomes more valuable than remaining optionality.

> Build intent should be generated from validated priors before the shop is evaluated. The shop supplies evidence and opportunities; it does not define the objective from scratch.

> Candidate priors can share meaningful build backbones. Early decisions should recognize cross-path optionality and delay irreversible branching when doing so preserves several high-scaling futures at acceptable tempo cost.

## 16. Interaction with DM-0.8

DM-0.8's multi-action live contract remains active.

Decision sequences should include liquidation actions when appropriate, for example:

1. SELL stale gem and obsolete branch ingredient.
2. BUY shared-backbone or committed-line upgrade.
3. DEPLOY upgrade and repack.
4. REROLL with remaining gold.
5. STOP and show shop.

The transaction sequence must remain globally feasible and stop at genuine information boundaries.

## Regression cases carried into DM-0.9

All DM-0.8 regression cases remain active, plus:

- starting strategic reasoning from shop salience rather than validated priors/build paths;
- treating candidate priors as isolated when they share a meaningful build backbone;
- failing to value a shared-backbone item for preserving multiple strong branches;
- double-counting cross-path optionality as duplicate combat value;
- preserving optionality too long after one branch clearly dominates;
- failing to surface and calibrate materially relied-upon shared/branch dependencies with the Player;
- treating an incomplete dependency map as known;
- missing a core/enabler item because unrelated shop inventory is more visually salient;
- reopening a settled strategy because of locally salient shop inventory;
- buying generic value that does not strengthen or enable the committed line;
- failing to increase strategic discipline in critical board states;
- treating storage purchase price/resale value as equivalent to current strategic value;
- holding stale inventory after its credible deployment/recipe/branch path has decayed;
- failing to sell stale inventory to fund immediate deployable power;
- violating an established strategy without identifying new evidence that justifies the override.

## Live output contract

At run start or material build-path change, first surface the leading build prior, adjacent priors sharing its backbone, and the key shared/branch dependency assumptions for Player calibration.

For normal live shops, output remains concise and executable.

Before answering, reconcile in this order:

1. validated priors and candidate scalable paths;
2. shared backbone and live branch set;
3. current preferred/committed build path;
4. current board and combat deficit;
5. build needs and dependency map;
6. stored inventory and liquidation candidates;
7. space and transaction constraints;
8. current shop actions classified against the build map;
9. fallback alternatives only if the preferred path/path set cannot be advanced sufficiently;
10. contradiction check.

Then output the ordered action sequence to the next genuine information boundary.
