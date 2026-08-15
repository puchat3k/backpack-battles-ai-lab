# Run 002 — Reaper / Vampiress live decision log

**Privacy:** sanitized public experiment record. Contains only Backpack Battles gameplay information. No unrelated personal/project context is included.  
**Patch:** 1.1.8  
**Decision model at start:** DM-0.3  
**Decision model produced by review:** DM-0.4  
**Strategy prior:** H2  
**Character:** Reaper  
**Subclass:** Vampiress  
**Observed checkpoint:** Round 9, 4 wins, 1 life remaining after consecutive losses.

## Purpose

This is a compact reconstruction of consequential decisions, observed outcomes, and prediction/reasoning mismatches. It is intentionally not a transcript. Screenshots remain the preferred evidence for board states; this document records only the decision-relevant interpretation.

## Run summary

The run developed from a flexible Reaper board into a Vampiress direction with Hungry Blade and Blood Amulet support. The board accumulated several strong future-value assets and recipe paths, but by Round 9 the deployed build was behind the observed combat-power curve. The run therefore exposed a recurring tension between end-state construction and immediate tempo.

## Consequential decisions

### 1. Subclass: Vampiress

Vampiress was selected from the offered subclasses. The choice aligned with the existing melee/vampirism direction and preserved a coherent scaling path.

### 2. Hungry Blade integration

Hungry Blade was acquired and deployed rather than treated as the previously misidentified item. This strengthened the Vampirism direction and created future upgrade optionality.

Learning: item identity must be reconciled from tooltip/transaction evidence before strategy is recomputed.

### 3. Recipe staging and geometry

Several components were arranged for combinations that would resolve only after the next combat. This exposed an important state distinction: a recipe can be staged without yet being a transformed item.

Learning: current board geometry, current combat effects, and expected post-combat geometry must be represented separately.

### 4. Garlic as cheap tempo filler

Garlic placement displaced lower-value filler in order to improve immediate combat contribution while preserving the broader path.

Learning: cheap items should not be labelled permanent garbage. Their value depends on available board geometry and immediate tempo needs.

### 5. Blood Amulet purchase

Blood Amulet was purchased despite significant cost because it supplied immediate Vampirism/max-health value and is a component for multiple Vampiric support upgrades.

This was one of the cleaner path-consistent purchases of the session.

### 6. Box of Riches

Box of Riches was retained/deployed despite its two-slot footprint and bookkeeping burden.

Its value is multi-part:

- generates a chipped gemstone on shop entry
- creates socket/combine/sale options
- changes future shop availability by introducing gemstones
- compounds value over subsequent rounds

Its cost is also multi-part:

- two board slots
- generated-asset tracking
- storage/economy accounting
- altered shop-pool reasoning

Learning: distinguish intrinsic game EV from process complexity in screenshot-mediated play.

### 7. Accidental reroll

A reroll occurred despite the intended plan to proceed to combat.

Classification: operator execution error.

The correct response was not to continue optimizing the previous hypothetical shop. The state had changed and needed immediate reconciliation.

A subsequent economy recovery used stored sellable value to obtain additional bag space, demonstrating that displayed gold alone understated purchasing capacity.

### 8. Gemstone management

Box generated gemstones. A Chipped Amethyst was eventually socketed into the weapon, providing an on-hit chance to remove an opponent buff.

Learning: generated gems require an explicit decision hierarchy: socket, combine/upgrade, tempo placement, retain, or sell. Automatic selling loses option value; automatic retention creates clutter.

### 9. Corrupted Armor / Holy Armor component decision

A discounted armor opportunity was considered. The operator judged the Corrupted Armor direction unsuitable for the intended build path, while acknowledging that discount/tempo value could still matter.

The episode exposed a model failure: discount magnitude had been overweighted relative to path compatibility and full transition cost.

### 10. Final observed deployment changes

Before the final recorded combat:

- Star was removed and Dust deployed
- Chipped Amethyst was socketed into the weapon
- Box of Riches remained in play
- the board included substantial constructed/future-value material

The subsequent combat was lost decisively enough to strengthen the hypothesis that the run had accumulated **tempo debt**: theoretical scaling and construction value were not translating into enough immediate combat power.

## Prediction / reasoning mismatches

### Mismatch A — Time Dilator confidence

The model made an aggressive recommendation on Time Dilator without sufficient mechanical/build knowledge. The operator correctly challenged the epistemic basis of the recommendation.

Correction: unfamiliar high-impact mechanics trigger external verification or a low-confidence/reversible policy.

### Mismatch B — board-space accounting

The model repeatedly treated storage/visual space as though it implied deployable board capacity and miscounted removable filler.

Correction: exact footprint + reachable placement must be checked before recommending a purchase.

### Mismatch C — Stone identity/count

The model conflated ordinary Stones with gemstones and later recommended another Stone despite two Stones already being stored.

Correction: maintain typed inventory state and reconcile counts before marginal filler purchases.

### Mismatch D — Box valuation

Initial reasoning treated Box too much like a static accessory. Its generated assets, cumulative economy, and effect on future shop offers were under-modelled.

Correction: generator/shop-pool items require stateful valuation.

### Mismatch E — displayed gold

The model temporarily reasoned from liquid gold while ignoring immediately sellable stored value.

Correction: economic state separates liquid gold, disposable stored value, generated assets, and reserved components.

### Mismatch F — silence / feedback

Under time pressure, the operator stopped correcting every questionable recommendation. Lack of correction therefore could not be treated as evidence of recommendation quality.

Correction: silence is non-evidence; only explicit feedback and observed actions update confidence.

## Strategic interpretation

The run does **not** establish that Vampiress, Box of Riches, Blood Amulet, or the broader path is weak. The sample is one partially observed run with operator and model execution errors.

It does provide evidence that the decision engine needs stronger control of the **tempo versus construction** trade-off. When the board is losing while carrying unresolved recipes, generators, stored value, and speculative future power, the hurdle for additional scaling investments should rise.

## Engine changes generated

This run produced DM-0.4, adding:

- full economic-state accounting
- explicit board-space accounting
- staged transformation states
- generator/shop-pool valuation
- process-complexity penalty separated from intrinsic EV
- tempo-debt tracking
- conditional discount valuation
- external-mechanics escalation
- operator-error recovery
- silence-as-non-evidence
- compact execution mode under time pressure
- expanded pre-combat reconciliation

## Screenshot policy for future run logs

For future runs, retain screenshots only at consequential nodes rather than every exchange:

1. major shop/commitment decision
2. subclass selection
3. important recipe staging/resolution
4. major pivot
5. forecast mismatch or informative combat result
6. final board/result

Each screenshot should be scrubbed of anything outside the game window before publication. Public filenames should contain only run/round/decision identifiers, never local usernames, machine paths, account identifiers, or unrelated application context.

## Privacy boundary

Backpack Battles remains a firewalled experiment. Public run logs contain only game state, model decisions, mechanics evidence, experimental metadata, and sanitized screenshots. No unrelated conversation content, personal profile information, other projects, private communications, credentials, or account data may be transferred into this repository.