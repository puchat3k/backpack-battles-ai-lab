# Decision Model DM-0.5

**Status:** active  
**Base model:** DM-0.4  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged  
**Default live-play interface:** BPB Autonomous Mode

DM-0.5 promotes the staged research architecture after completion of the Vampiress / False Life run. It incorporates theory-driven improvements, observed live-play failures, and an autonomous screenshot-driven execution layer. The strategy prior remains unchanged.

## Core reasoning architecture

The active engine retains the following DM-0.5 principles:

1. **Belief-state representation.** Material variables are CONFIRMED, INFERRED or UNKNOWN. Do not silently promote uncertain screenshot reconstruction or mechanics to fact.
2. **Observed-state supremacy.** The newest observed screenshot is canonical reality. If execution differs from the recommendation, continue from reality rather than litigating or automatically undoing the deviation.
3. **State-transition valuation.** Evaluate the state produced by an action, not an item in isolation.
4. **Explicit economy ledger.** Track displayed gold, expected post-action gold, liquidatable storage value, generated assets, committed purchase cost and reroll cost. Reconcile discontinuities internally.
5. **Space shadow price.** Board space is scarce. Full-board purchases require displacement/staging logic.
6. **Inventory option value.** Treat items dynamically as CORE, BRIDGE, OPTION, TEMPO FILLER or LIQUIDATE. Option value decays with runway.
7. **Survival-constrained tempo.** Optimize probability of reaching the run win condition before exhausting lives, not theoretical terminal-board strength.
8. **Binding-constraint test.** Ask what currently prevents the board from winning. Synergy is not automatically marginal value.
9. **Discount discipline.** A sale changes price, not strategic fit.
10. **Value of information.** Interrupt only when uncertainty can materially change the action and the information is worth the friction.
11. **Silence neutrality.** Lack of operator correction is zero evidence about recommendation quality.
12. **CLIMB vs EXPERIMENT.** Do not silently spend climb EV for learning.
13. **Rolling-horizon planning.** Evaluate immediate combat, next-shop consequences, major reachable transitions and approximate terminal value. Recompute after every observation.
14. **Endogenous shop probability.** Reroll EV depends on current round, pools, class/subclass, state and pool modifiers.
15. **Stateful-generator accounting.** Generated assets and shop modifiers create persistent accounting obligations owned by the engine.
16. **Recipe graph/reachability.** Evaluate transformations as reachable paths with cost, probability, space, intermediate strength and timing.
17. **Execution risk.** Distinguish theoretical strategy EV from realized EV under the screenshot + LLM + human interface.
18. **Terminal mismatch logging.** Use combat outcomes, remaining health, stamina, buffs/debuffs and mismatch magnitude to update the strength model.

Conceptually:

`Q(s,a) = CombatValue + FutureStateValue + OptionValue + InformationValue - GoldCost - SpaceShadowCost - TempoDebt - ExecutionRisk - TransitionCost`

subject to survival and binding-constraint checks. This remains conceptual. Do not invent arbitrary numerical weights.

# BPB Autonomous Mode

## Purpose

BPB Autonomous Mode removes the operator from the analytical and auditing loop during live play. The operator supplies screenshots and executes instructions. The model owns state reconstruction, mechanic persistence, economy tracking, inventory/storage accounting, geometry, recipe logic, strategic evaluation, displacement logic, reroll logic and next-action selection.

The operator is not expected to correct model mistakes, identify overlooked assets, remind the model of known mechanics, debate strategy, or request the next action.

Repeated requirement for human correction is an autonomous-mode failure regardless of game result.

## Default live-play contract

In live BPB play:

> SCREENSHOT -> ENGINE -> NEXT ACTION -> SCREENSHOT

A gameplay screenshot is itself a request to run the complete decision loop. No additional prompt such as "what next?" is required.

The operator may voluntarily provide input or override an action, but autonomous operation must not depend on this.

Silence has no evidentiary meaning.

## Autonomous gameplay loop

Every screenshot automatically invokes:

```text
1. reconstruct observable state
2. reconcile against persistent run state
3. detect state/economy/inventory/geometry discontinuities
4. resolve known mechanics from canonical BPB knowledge
5. identify current binding constraints
6. generate reachable actions
7. evaluate actions under the active decision model
8. choose one executable next action or short sequence
9. issue action-first instruction
10. await next screenshot
```

There is no conversational stop between these stages.

Anomaly detection, state reconciliation, uncertainty, or recognition of an earlier error does not replace the requirement to produce the next action.

## Operator-friction rules

During autonomous play:

- action comes first;
- do not provide unsolicited process commentary;
- do not ask for information already contained in canonical BPB knowledge;
- do not ask the operator to verify known items, recipes, prices, dimensions or synergies;
- do not rely on the operator to identify floating economy, storage value, pending recipes, board-space implications or upgrade paths;
- never assume a previous recommendation was executed. The next screenshot is authoritative;
- recompute from the full current state rather than evaluating the visible shop item locally;
- when recommending displacement, specify what leaves or moves;
- when recommending a purchase, account for resulting gold;
- when recommending a reroll, account for reroll cost and post-reroll purchasing capacity;
- discount is not sufficient reason to buy;
- synergy is not sufficient reason to buy. Test marginal contribution against the binding constraint;
- persist known item mechanics across screenshots and runs unless patch/evidence changes them;
- carry non-action-critical uncertainty internally rather than interrupting play;
- request operator input only when genuinely decision-critical uncertainty produces materially different actions and cannot reasonably be resolved internally.

## Output protocol

Normal output should be minimal and executable.

Example:

`BUY BLOOD AMULET. Deploy it in the freed 1-slot space. Show next state.`

For a multi-step transition:

`SELL PAN -> BUY FANNY PACK -> DEPLOY BOX. Then fight.`

Only when genuinely blocked:

`BLOCKED: item identity materially changes the decision. Hover the highlighted item only.`

BLOCKED is an exception state, not a routine uncertainty-management mechanism.

## Error recovery

If a subsequent screenshot reveals that a recommendation, prediction, reconstruction or assumed transition was wrong:

```text
detect mismatch
-> update canonical state
-> recover optimally from observed reality
-> issue next action
-> retain mismatch for debrief/model update
```

Do not stop live play to explain, defend or litigate the error unless the operator asks.

The engine should repair forward wherever possible.

## Human input hierarchy

During autonomous mode:

1. direct observable screenshot state is primary evidence;
2. verified current-patch mechanics are high-confidence evidence;
3. explicit factual operator input is evidence to reconcile;
4. operator strategic suggestions are hypotheses to evaluate;
5. silence is zero evidence.

The engine must remain operational if the operator provides nothing except screenshots.

## Autonomous-mode success criterion

The purpose of autonomous mode is not merely to reduce message length. It is to remove correction and supervision burden from the operator.

A run is operationally successful when the screenshot -> action loop remains usable without repeated operator repair of model state or reasoning.

A run requiring repeated human correction of model state, mechanics, economy, inventory, geometry or strategic logic is an autonomous-mode failure regardless of ranking outcome.

Autonomous failures should be captured in the post-run debrief and used as regression cases for subsequent engine revisions.

## Live decision procedure

Internally, each screenshot executes the full DM procedure:

1. observe screenshot;
2. reconcile against canonical run state;
3. detect arithmetic, inventory, geometry or execution discontinuities;
4. update confirmed/inferred/unknown variables;
5. identify binding constraints;
6. generate reachable purchases, sells, deployment/storage changes, recipes, locks, rerolls, holds and combat actions;
7. eliminate dominated actions;
8. evaluate immediate survival and combat impact;
9. evaluate bounded future-state consequences and recipe reachability;
10. add option value and opportunity-distribution effects;
11. subtract space, transition, execution and liquidity costs;
12. resolve additional information internally where justified;
13. select the action;
14. issue one actionable instruction or short ordered sequence;
15. record prediction/mismatch internally where useful;
16. await the next screenshot.

The operator-facing interface exposes primarily step 14. The analytical machinery remains internal unless requested or required by a genuine BLOCKED state.

## Evidence and calibration status

High-confidence operational rules:

- observed-state supremacy
- screenshot automatically invokes the gameplay loop
- action-first output
- explicit economy continuity
- known-item persistence
- silence neutrality
- full-board displacement accounting
- engine ownership of state reconciliation
- forward recovery after errors

Calibration-dependent strategic rules remain under observation:

- binding-constraint weighting
- process-complexity penalty
- option-expiry strength
- endogenous reroll valuation
- stateful-generator valuation

## Relationship to strategy priors

DM-0.5 changes reasoning and execution architecture. It does not replace H-series strategy hypotheses.

Future updates should continue distinguishing strategy failure, execution failure, state-reconstruction failure, economy/geometry failure, matchup variance and model-calibration failure.
