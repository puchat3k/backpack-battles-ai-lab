# Decision Model DM-0.9

**Status:** active  
**Base model:** DM-0.8  
**Patch context:** 1.1.8  
**Default live-play interface:** BPB Autonomous Mode

## Why DM-0.9 exists

A Reaper run exposed a persistent execution failure. The engine could establish a sound strategic direction, but then repeatedly reopened that decision when a locally attractive shop item appeared. It also accumulated undeployed inventory whose nominal resale value obscured the fact that its strategic option value was decaying as the run progressed.

DM-0.9 adds two controls: strategy commitment and dynamic inventory liquidation.

## 1. Strategy commitment states

A run-level strategy may occupy one of three states:

1. **Hypothesis**. Plausible direction, weak evidence, easily overridden.
2. **Preferred line**. Supported by board state and mechanics, should guide purchases unless meaningful contrary evidence appears.
3. **Committed line**. Supported strongly enough that it becomes a decision constraint rather than merely another shop-scoring input.

Promotion must be evidence-based. Do not commit to a line merely because it is novel or interesting.

## 2. Commitment enforcement

Once a line is committed, every proposed transaction must answer internally:

- Does this action strengthen the committed line now?
- Does it enable a known near-term transition within that line?
- Does it solve a critical weakness that the committed line cannot otherwise solve?
- If none apply, what evidence justifies overriding the strategy?

A locally attractive sale, rarity, generic value item, or speculative synergy does not by itself defeat a committed strategy.

The engine must not silently reopen a settled strategic question on every shop roll.

## 3. Critical-state commitment tightening

As lives decrease, combat losses become severe, or the board falls materially behind expected power, strategic discipline increases.

In critical states:

- prioritize immediate deployable board strength;
- raise the threshold for speculative purchases and strategic pivots;
- reduce tolerance for undeployed inventory and economy-only purchases;
- require stronger evidence before overriding a committed line;
- prefer actions that directly address the observed combat deficit.

Critical state does not mean blindly preserving a losing strategy. A pivot remains valid when evidence shows the committed line is structurally failing and a credible alternative is executable.

## 4. Inventory is not static wealth

Storage value must not be evaluated only by purchase price or resale value.

An undeployed item has no immediate combat contribution. Its value is dynamic:

`hold value = resale value + remaining option value - congestion cost - tempo opportunity cost`

Where:

- **resale value** is recoverable gold;
- **remaining option value** is the probability-weighted value of future deployment, combination, trade, or strategic use;
- **congestion cost** reflects storage/board-space pressure and increased transaction complexity;
- **tempo opportunity cost** reflects combat power forgone by leaving capital undeployed.

This is a conceptual decision rule, not a requirement for exact numerical calculation.

## 5. Inventory option-value decay

Remaining option value generally declines with:

- later round number;
- fewer remaining shops/lives;
- stronger commitment to a build that does not use the item;
- repeated failure to find the item's required enabler;
- increasing storage congestion;
- urgent need for deployable combat power;
- availability of a clearly superior use for the recoverable gold.

Therefore a 1g speculative item in Round 1 can be rational to hold while the same item in Round 10 may be economically stale even if its resale price is unchanged.

Nominal gold value is not strategic value.

## 6. Liquidation trigger

At every shop, especially midgame and later, explicitly evaluate stored items for sale.

A stored item becomes a liquidation candidate when:

- it has no credible near-term deployment path;
- it is not part of a likely recipe or committed synergy;
- its required enabler has repeatedly failed to appear;
- storage congestion is material;
- selling it funds an immediate deployable upgrade, reroll, or committed-line purchase;
- the run is critical and optionality is less valuable than current combat power.

Late-game default:

> Either the item has a credible near-term route onto the board, contributes to a known recipe/committed line, or it should face strong sale pressure.

Do not retain stale items merely because selling them feels like abandoning prior investment.

## 7. Phase-dependent inventory tolerance

**Early game:** tolerate experimentation and reversible inventory positions when tempo remains adequate.

**Midgame:** prune inventory that is not converging toward the emerging build.

**Late/critical game:** become hostile to undeployed capital. Convert stale inventory into gold and gold into immediate board strength whenever credible opportunities exist.

## 8. Pre-answer contradiction check

Immediately before issuing a live recommendation, check:

> Does this action contradict the strategy, space constraints, storage state, tempo requirement, or known synergies that the engine has already established?

If yes, either reject the action or explicitly identify the new evidence that justifies the override.

This check is mandatory in critical board states.

## 9. Regression case. Reaper Stone Badge/Stoned run

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

## 10. Interaction with DM-0.8

DM-0.8's multi-action live contract remains active.

Decision sequences should now include liquidation actions when appropriate, for example:

1. SELL stale gem and unused ingredient.
2. BUY committed-line weapon upgrade.
3. DEPLOY upgrade and repack.
4. REROLL with remaining gold.
5. STOP and show shop.

The transaction sequence must remain globally feasible and stop at genuine information boundaries.

## Regression cases carried into DM-0.9

All DM-0.8 regression cases remain active, plus:

- reopening a settled strategy because of locally salient shop inventory;
- buying generic value that does not strengthen or enable the committed line;
- failing to increase strategic discipline in critical board states;
- treating storage purchase price/resale value as equivalent to current strategic value;
- holding stale inventory after its credible deployment/recipe path has decayed;
- failing to sell stale inventory to fund immediate deployable power;
- violating an established strategy without identifying new evidence that justifies the override.

## Live output contract

Normal live-play output remains concise and executable.

Before answering, reconcile:

1. strategy commitment state;
2. current board and combat deficit;
3. stored inventory and liquidation candidates;
4. space and transaction constraints;
5. current shop actions;
6. contradiction check.

Then output the ordered action sequence to the next genuine information boundary.
