# Decision Model DM-0.4

**Status:** active  
**Patch context:** 1.1.8  
**Strategy prior context:** H2

DM-0.4 extends DM-0.3 after a live Reaper/Vampiress run exposed additional failures in state accounting, process complexity, tempo control, and human-execution handling.

## 1. Base policy

DM-0.3 remains the base policy. DM-0.4 adds the rules below.

## 2. Full economic state, not displayed gold

Displayed gold is not total purchasing power. Track separately:

- liquid gold
- stored sellable value
- deployed sellable value that is plausibly disposable
- generated assets not yet deployed
- reserved components/options

Do not recommend a sale merely because an item is currently redundant. Cheap filler such as Stones can have future tempo value when geometry opens.

## 3. Board-space accounting is explicit

Before recommending a purchase, verify the exact footprint and a reachable placement. A full board means a purchase requires one of:

- repositioning
- storage
- sale/removal
- bag/space acquisition
- a resolving transformation that frees space

Never describe storage or visual gaps as usable board slots without verification.

## 4. Staged transformations

Items arranged for a recipe are not yet transformed. They remain separate components until the next combat resolves the combination.

Represent recipe states as:

`AVAILABLE -> STAGED -> RESOLVED`

A STAGED recipe affects current geometry, current combat, and post-combat expected geometry differently from a RESOLVED item.

## 5. Generator / shop-pool effects

Items such as Box of Riches must be valued as stateful systems, not one-shot accessories. Track:

- generated asset per shop entry
- expected socket/combine/tempo/sale uses
- cumulative economic value
- board/storage burden
- modification of future shop offer probabilities
- management complexity

Generated gemstones follow a default evaluation order:

`high-value socket -> meaningful combine/upgrade -> useful board tempo -> retain option -> sell`

This is a heuristic, not an automatic disposal rule.

## 6. Process-complexity penalty

Some mechanically strong items create bookkeeping/state-reconstruction burdens in screenshot-mediated play. Add a **process-complexity penalty** to practical decision value, but keep it separate from intrinsic game value.

`practical value = intrinsic game EV - execution/error burden`

Do not conclude that a complex item is strategically weak merely because the current human/model interface handles it poorly. If rank-climb performance is the experiment objective, retain complex items when their expected game value clearly exceeds the execution penalty.

## 7. Tempo debt

Track whether the build is spending current combat strength on future value.

Tempo-debt signals include:

- multiple unresolved components
- economy/generator purchases while behind the lobby curve
- excessive stored value with weak deployed power
- speculative upgrades consuming board space
- repeated losses despite increasing theoretical end-state quality

When tempo debt rises, increase the hurdle for economy, speculative recipes, and long-horizon pivots. Prefer actions that improve the next combat without destroying the established path.

## 8. Discount is conditional on usability

Absolute discount is not value by itself. A heavily discounted item receives little or negative strategic value if it is incompatible with the class/build, creates severe stamina/geometry problems, or strands the current path.

Evaluate:

`discount value x probability of useful deployment - transition/space/stamina cost`

## 9. External-mechanics escalation

When an unfamiliar item or mechanic could materially alter the run, uncertainty must be explicit. Do not make a high-confidence purchase recommendation from tooltip intuition alone.

Escalation order:

1. inspect tooltip/current evidence
2. identify uncertainty
3. use reliable external mechanics/build evidence when permitted
4. downgrade confidence if still unresolved
5. prefer reversible actions when EV is unclear

## 10. Operator-error recovery

Human execution errors are part of the observed system. If the operator accidentally rerolls, buys, sells, combines, or misplaces an item:

- do not reconstruct the counterfactual as if it still exists
- immediately reconcile the new actual state
- classify the event as `operator execution error`
- optimize forward from the new state
- preserve the mismatch in the run log

Operator mistakes are not automatically decision-model failures, but repeated error-prone instructions can indicate an interface/process failure.

## 11. Silence is non-evidence

Absence of correction is not confirmation. The operator may be time-constrained, distracted, uncertain, or choosing not to intervene.

Therefore:

- do not increase confidence because a recommendation went unchallenged
- do not infer approval, frustration, agreement, or adversarial intent from silence
- only explicit corrections or observable actions update the state

## 12. Instruction compactness under time pressure

When the operator signals time pressure, switch to execution mode:

- one recommended action sequence
- only essential contingencies
- no unnecessary debate
- confidence/uncertainty only where decision-relevant

This reduces execution errors without treating operator silence as validation.

## 13. Pre-combat gate

Before COMBAT, reconcile:

1. displayed gold and economic reserves
2. exact board occupancy
3. storage
4. generated assets
5. staged recipes
6. sockets
7. stamina status
8. pending shop lock/reroll decision
9. immediate-power versus tempo-debt state
10. forecast

## 14. Revision rationale

DM-0.4 is justified by multiple distinct failures/observations in one live run: incorrect space accounting, incomplete storage/economy accounting, confusion between staged and resolved combinations, under-modeling Box of Riches, an overconfident unfamiliar-item recommendation, operator misclick recovery, and evidence that late construction value can coexist with insufficient immediate combat power.

These are general state/action-model corrections rather than Player-specific strategy preferences.