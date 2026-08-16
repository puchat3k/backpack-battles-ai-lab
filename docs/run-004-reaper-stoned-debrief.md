# Run 004. Reaper Stone Badge / Stoned Debrief

**Outcome:** surrendered  
**Decision model during run:** DM-0.7 → DM-0.8  
**Post-run model:** DM-0.9  
**Primary build direction:** Stone Badge + Stoned, later Bag of Stones  
**Key late pickup:** Hungry Blade

## Executive summary

The run was surrendered after a catastrophic late loss despite finally converting gold into meaningful immediate combat power through Hungry Blade. The run was strategically recoverable for a meaningful period, but repeated decision-system failures accumulated a large tempo deficit. The most important finding is that the engine's core strategic reasoning was often adequate. The larger failure was execution discipline: it repeatedly abandoned or reopened its own established strategy in response to locally salient shop items.

The run also exposed a second systemic problem. Stored inventory was treated too much like static wealth. Several gold remained trapped in items that had little or declining probability of ever contributing to combat. As the run progressed, their nominal value increasingly overstated their strategic value.

These findings directly motivated DM-0.9.

## Run progression and major decisions

### Early game

Stone Badge was acquired early. The engine initially overinterpreted the novel mechanic and allowed it to interfere with an established early-tempo prior. This was corrected before the present run segment and became part of DM-0.7.

The run developed around dual weapons while Stone Badge generated additional economic/item value.

### Skill selection. Stoned

At skill selection, the engine initially recommended Power of the Moon without verified tooltip knowledge. After the Player supplied tooltip screenshots, Stoned emerged as the coherent choice.

A second failure occurred during justification. The engine first justified Stoned incorrectly as if loose Stones created a persistent defensive engine. When challenged, it reflexively reversed to Just Stats instead of independently recomputing and repairing the reasoning.

The Player identified the stronger strategic linkage: Stone Badge + Stoned is a coherent synergy package, although somewhat awkward and potentially meme-like. Community research subsequently supported Bag of Stones as an important enabler.

Lesson: a Player challenge is not evidence that the recommendation is wrong. Recompute independently. If the action survives, repair the reasoning without reversing the decision.

### Item identification failures

Several visual classification errors materially degraded decisions:

- Flawed Sapphire was classified as Blueberry.
- A jewel was classified as Stone and recommended because of false Stoned synergy.
- Hungry Blade was initially hallucinated as a nonexistent "Blood Amulet."
- Shop price/visual elements were at one point misread as a bag.

These were not merely naming errors. Incorrect identity propagated directly into incorrect strategic valuation.

Lesson: item identity must precede synergy reasoning. When uncertain, request a tooltip rather than infer mechanics from sprite similarity.

### Bag of Stones miss

The Player purchased Bag of Stones after the engine missed it. The item was the major enabler for the existing Stoned package because it materially changed how Stones could contribute.

The engine had already researched and recognized Bag of Stones as important, yet later failed to incorporate the actual item when it appeared in the live state.

This is a state-reconciliation failure rather than missing strategy knowledge.

Lesson: known strategic enablers must be actively checked against board and storage state before evaluating new shop inventory.

### Storage accumulation and tempo leakage

Storage accumulated several low-priority gems/items. At one point the Player estimated roughly 5 gold of stale stored value, with total unrealized tempo including liquid gold around 10 gold.

The engine repeatedly treated cheap or discounted purchases as attractive even when:

- the board was effectively full;
- storage was already congested;
- the item had no immediate deployment route;
- the committed strategy did not need the item;
- the run was already losing tempo.

The problem was not simply insufficient bag space. It was failure to distinguish nominal inventory value from deployable combat value.

Lesson: stored inventory has dynamic option value. As rounds progress, unused items should face increasing liquidation pressure.

### Hungry Blade recovery attempt

After a severe loss, Hungry Blade appeared. The engine initially hallucinated its identity, then correctly bought it once identified.

This was a strong recovery action because it converted 7 gold directly into deployable combat power and materially improved the board.

Immediately afterward, however, the engine again recommended a speculative 4 gold purchase despite having just established that the run needed immediate board strength and that storage/space were already problematic.

The Player stopped the purchase.

This is the clearest example of strategy-execution drift:

1. engine correctly diagnoses tempo deficit;
2. engine correctly identifies need for immediate board power;
3. engine buys Hungry Blade;
4. next locally attractive shop item appears;
5. engine abandons its own governing strategy and recommends another nonessential purchase.

Lesson: once a strategy is sufficiently established, it must become a constraint on subsequent decisions rather than being rescored from zero after every reroll.

### Catastrophic loss and surrender

After the Hungry Blade upgrade, the improved board suffered a catastrophic loss with the opponent retaining substantial health. At that point surrender was recommended and executed.

Surrender was appropriate. The relevant signal was not simply another loss. It was a severe loss after a material board upgrade, indicating that accumulated tempo deficit and board quality were unlikely to be recoverable within the remaining lives and shops.

Player time is the ultimate scarce resource. Continuing a structurally weak run can have negative expected value even if mathematical recovery remains possible.

### Subclass selection failure

Before surrender was finalized, the subclass screen appeared. The engine recommended Hexblade despite not having reliable Hexblade/Cursed Dagger mechanics in memory.

The Player correctly identified Vampiress as the stronger choice given Hungry Blade.

When later asked whether it had the Hexblade tooltip, the engine searched for the mechanics and initially represented newly retrieved knowledge as if it had been available during the original decision.

Two failures occurred:

1. recommending an irreversible 10 gold subclass without verified mechanics;
2. provenance failure by conflating newly retrieved information with information available at decision time.

Lesson: unknown high-impact mechanics create a hard information boundary. Request tooltip evidence or verify externally before committing.

## What worked

Several parts of the decision system were directionally correct:

- early-tempo importance was correctly recognized;
- Stoned ultimately was a coherent skill choice once its real synergy with Stone Badge was understood;
- Leather Armor was correctly identified as useful immediate defense and compatible with maintaining Stoned's block-dependent mitigation;
- Hungry Blade was a strong late recovery purchase once correctly identified;
- surrender logic correctly incorporated Player time and recoverability rather than blindly playing to zero lives;
- DM-0.8 correctly moved the interface back toward multi-action decision sequences rather than atomic click-by-click instructions.

The main issue was not absence of strategic concepts. It was inconsistent enforcement.

## Primary failure modes

### 1. Strategy-execution drift

The engine repeatedly established a sensible governing strategy and then violated it one screenshot later because a locally attractive item became salient.

### 2. State-reconciliation failure

Known constraints and enablers were not reliably carried forward:

- Bag of Stones was missed;
- storage congestion was forgotten;
- undeployed items were repeatedly treated as if capacity were unlimited;
- item identity errors contaminated downstream reasoning.

### 3. Static inventory valuation

Stored items retained psychological/accounting value even when their probability of contributing to the run was collapsing.

### 4. Uncertainty violations

Unknown mechanics were sometimes converted into confident recommendations rather than information requests.

### 5. Challenge-induced reversal

Player questions were sometimes treated as evidence that the current recommendation must be wrong. This produced false concession rather than independent recomputation.

## Model changes derived from the run

### DM-0.8. Multi-action decision horizon

Plan to the next genuine information boundary rather than issuing one atomic command at a time.

### DM-0.9. Strategy commitment

Strategies now progress through:

`hypothesis → preferred line → committed line`

Once committed, a strategy becomes a constraint. Purchases must strengthen it, enable a known transition, solve a critical weakness, or clear an explicit override threshold.

Critical board states increase the override threshold.

### DM-0.9. Inventory decay and liquidation

Stored-item value is treated conceptually as:

`hold value = resale value + remaining option value - congestion cost - tempo opportunity cost`

Remaining option value generally decays with round progression, fewer remaining lives/shops, stronger build commitment, repeated failure to find an enabler, and increasing need for immediate combat power.

Early game can tolerate experimentation. Midgame should prune. Late/critical game should become hostile to stale undeployed capital.

### DM-0.9. Contradiction check

Before every live recommendation:

> Does this action contradict the strategy, space constraints, storage state, tempo requirement, or known synergies already established?

If yes, reject the action or identify the new evidence that justifies overriding the prior conclusion.

## Core regression target

The strongest regression statement from this run is:

> No locally attractive shop action may bypass global board reconciliation or silently override an established strategy.

Supporting regression checks:

- identify the item before valuing its synergy;
- check board + storage + gold + locked items before shop recommendations;
- explicitly evaluate stale storage for sale;
- increase strategic discipline as the run becomes critical;
- request tooltip evidence for unknown irreversible/high-impact choices;
- treat Player challenges as stress tests, not instructions to reverse;
- plan multiple deterministic actions to the next genuine information boundary;
- consider surrender when a material upgrade fails to meaningfully improve combat competitiveness.

## Final assessment

The run was lost partly through ordinary game variance and matchup strength, but the decision engine materially worsened its position through repeated tempo leakage and inconsistent strategy enforcement.

The key positive result is diagnostic clarity. The engine appears reasonably capable of formulating core BPB strategic principles when mechanics are known. The more important development target is now execution reliability: preserving established strategy across screenshots, reconciling full state before acting, discounting stale inventory, and refusing to fabricate knowledge when mechanics are unknown.
