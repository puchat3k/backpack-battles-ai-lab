# Run 001: Vampiress / False Life

## Result

- Class: Vampiress
- Final round: 10
- Wins: 4
- Final lives: 0
- Ranking: Platinum 94 -> 90
- Rating change: -4
- Final observed maximum health: 404
- Terminal opponent health: 153 / 157

This run is preserved as a decision-trace case study. It summarizes material board decisions, execution errors, state-management failures, and prediction mismatches rather than reproducing the full interaction transcript.

## Build trajectory

The run developed around Vampiress sustain, maximum-health scaling, Blood Amulet, Cap of Discomfort and supporting economy/tempo pieces. A late Round 10 skill choice moved the build further toward sustain through **False Life**.

False Life provided maximum-health scaling from items, converted overheal into maximum health, and periodically healed with additional scaling from starred skull items. This made Blood Amulet particularly relevant because it already supported the Vampiress sustain plan and contributed to the skull-item interaction.

By the final combat the build reached 404 maximum health. The terminal opponent nevertheless finished at 153 / 157 health, indicating that the build's durability did not translate into sufficient pressure against that matchup.

## Material decisions

### Blood Amulet

An earlier Blood Amulet was treated as a core build-path item because it added maximum health and Vampirism. A second Blood Amulet appeared in Round 10 for 8 gold and was purchased.

The purchase became more attractive after False Life because it reinforced an already coherent sustain/max-health axis and contributed to False Life's skull-item scaling.

### Box of Riches

Box of Riches remained in play despite creating substantial process complexity. It generated gemstones and altered shop composition by introducing stones. Its value therefore could not be evaluated only from the visible board state.

The item created several hidden-state burdens:

- generated gemstones could be socketed or sold;
- stored stones and other low-value items represented contingent future tempo rather than simply redundant inventory;
- effective available economy differed from the displayed gold total;
- keeping the Box changed future shop probabilities.

The run therefore exposed a distinction between an item being strategically viable in the game and being operationally tractable in a screenshot-mediated human/AI workflow.

### Corrupted Armor

Corrupted Armor was purchased at a substantial discount during Round 9. The engine initially over-weighted the discount and immediate tempo value. The player correctly challenged whether it belonged to the intended build path.

It was deployed for the next fight, but this decision remains a useful example of the difference between **cheap power** and **coherent power**. A discounted item is not automatically high expected value if it consumes scarce board space and strengthens the wrong strategic axis.

### Gemstones

A Chipped Amethyst generated during the run was socketed into the weapon. Its weapon effect provided a chance on hit to remove an opponent buff.

Another inexpensive Amethyst was purchased in Round 10 for 1 gold because socket capacity made it a low-cost incremental upgrade.

### Board space and storage

Board space became a binding constraint. Several decisions could not be evaluated as simple buy/sell choices because storage contained pieces with option value. Stones, dust, gemstones, bags and temporary tempo pieces could become useful after later purchases or rearrangements.

A discounted Fanny Pack was purchased in Round 10 for 2 gold to increase deployable space.

## Execution and state-management errors

### Accidental reroll

The player accidentally rerolled after an instruction not to do so. The run continued from the resulting state rather than attempting to reason from the counterfactual shop.

This establishes an important operating rule: **observed state overrides intended state**. When human execution diverges from the recommendation, the engine should immediately reconcile to the new board and continue optimization from reality.

### Gold-state discontinuity

The engine made an arithmetic/state-tracking error late in Round 10. After 7 gold, purchasing a 2-gold Fanny Pack and spending 1 gold on a reroll should leave 4 gold. The engine incorrectly projected 5 gold before the screenshot exposed the discrepancy.

The state was reconciled to 4 gold. This was not a game-mechanics failure. It was a bookkeeping failure.

### Repeated rediscovery of known items

The engine asked for a Blood Amulet tooltip even though the item and its strategic relevance had already been established earlier in the run.

This exposed a missing persistence rule. Verified item identity, mechanics, tags, dimensions and relevant synergies should persist as canonical run knowledge unless the game version changes, observed behavior conflicts with stored mechanics, or a required property remains unknown.

### Gameplay-loop interruption

At one point the engine detected and reconciled a state anomaly but failed to continue through the normal gameplay loop and issue the next action. The player had to prompt for an action.

In live play, a new screenshot should be treated as an event that automatically invokes:

```text
observe
-> reconcile state
-> detect anomalies
-> evaluate board/shop
-> issue next action
-> await next observed state
```

Anomaly detection is part of the loop, not an endpoint.

## Prediction mismatches

### Sustain scaling did not solve the terminal problem

The most important strategic mismatch appeared in the final combat.

The late build strongly increased durability. Maximum health reached 404, yet the opponent ended at 153 / 157 health. The result suggests that the build had become heavily optimized along a sustain/health axis without enough ability to convert survival time into damage, disruption or another win condition.

The second Blood Amulet and False Life were internally coherent. The mistake may therefore be subtler than either decision being individually bad. They may have increased the marginal value of an axis that was already sufficiently supplied while leaving the binding combat constraint unresolved.

This motivates a broader decision rule:

> Evaluate upgrades by the probability that they relieve the current binding constraint, not only by their synergy with the existing build.

### Discount bias

The Corrupted Armor decision exposed a tendency to treat a large discount as evidence of high expected value. Discount improves the economics of an item, but does not eliminate board-space cost, transition cost or strategic incoherence.

A better evaluation is:

```text
net upgrade value
= combat contribution
+ synergy contribution
+ future option value
+ resale/economic value
- board-space opportunity cost
- transition/rearrangement cost
- strategic dilution
- process/state complexity
```

## Process learnings

### Silence is not confirmation

During time pressure the player stopped correcting some recommendations. Lack of correction cannot be interpreted as agreement or validation. It may reflect limited time, attention or willingness to debate.

The engine should therefore update confidence from explicit evidence and observed outcomes, not from the absence of user correction.

### Some viable game objects create unacceptable state complexity

Box of Riches is the clearest example. A sufficiently complete software engine could track generated items, storage value, shop-pool effects and contingent deployment options. In a screenshot-mediated human/AI workflow, the same item imposes substantial bookkeeping overhead and increases error probability.

This creates a real experimental question: whether deliberately avoiding high-complexity items materially caps achievable rank, or whether sufficiently optimized lower-complexity strategies can compensate.

The current run does not answer that question.

## Candidate engine updates from this run

These are observations for staging rather than claims that every rule is already validated:

1. **Observed-state supremacy.** Human execution errors immediately become the new canonical state.
2. **Known-item persistence.** Do not repeatedly request mechanics already verified in the run knowledge base.
3. **Automatic screenshot loop.** Every gameplay screenshot should terminate in an actionable recommendation unless the engine explicitly requires one missing decision-critical fact.
4. **Explicit economy ledger.** Track displayed gold, stored liquidatable value, committed purchases and expected post-action gold separately.
5. **Complexity cost.** Include state-tracking/process burden as a real cost when choosing between strategically similar lines.
6. **Binding-constraint test.** Before reinforcing an existing synergy, ask whether the build actually needs more of that property to win the next plausible fights.
7. **Discount discipline.** Evaluate sale items against strategic fit and board opportunity cost, not purchase price alone.
8. **Silence neutrality.** Absence of correction supplies no positive evidence that a recommendation was correct.
9. **Storage option value.** Low-value stones and similar filler should not automatically be liquidated because open board space can make them useful temporary tempo later.
10. **Terminal mismatch logging.** Record not only win/loss but the surviving opponent state when visible, because it helps distinguish close variance losses from structural build failures.

## Terminal assessment

The run ended at 4 wins with a -4 rating change, from Platinum 94 to 90.

The final fight provides stronger evidence than the raw loss alone. The opponent retained almost all health while the Vampiress build had scaled to 404 maximum health. This is consistent with a build that became durable without developing an adequate mechanism for converting that durability into victory.

The strongest learning from the run is therefore not simply "buy less sustain." It is that **synergy coherence and marginal win contribution are different variables**. Once an axis is sufficiently developed, the next coherent upgrade can still be inferior to an upgrade that addresses the build's actual bottleneck.

## Evidence limitations

This case is reconstructed from sequential screenshots and the associated live decision trace. It does not contain complete combat telemetry, exact damage timelines, every shop state, or a counterfactual replay of alternative purchases. Causal conclusions should therefore remain provisional until repeated across additional runs.