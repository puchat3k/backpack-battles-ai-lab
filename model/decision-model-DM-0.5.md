# Decision Model DM-0.5

**Status:** active  
**Base model:** DM-0.4  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged  

DM-0.5 promotes the staged research architecture after completion of the Vampiress / False Life run. It incorporates both theory-driven improvements and directly observed live-play failures. The strategy prior remains unchanged. This revision changes how decisions are represented, reconciled and executed.

## 1. Belief-state representation

Do not equate reconstructed screenshot state with ground truth.

Material variables are classified as:

- CONFIRMED
- INFERRED
- UNKNOWN

Where decision-relevant, attach approximate confidence. Decisions operate on the belief state rather than silently assuming uncertain visual recognition, geometry or mechanics are correct.

Verified item identity, mechanics, tags, dimensions and important synergies persist as canonical BPB knowledge unless the patch changes, observed behavior conflicts with stored mechanics, or a required property remains unknown.

## 2. Observed-state supremacy

Observed state overrides intended state.

If the human misclicks, rerolls unexpectedly, buys the wrong item, repositions differently or otherwise diverges from the recommendation, the next observed screenshot becomes canonical reality.

Do not optimize toward undoing the mistake unless undoing it is itself the best current action.

Expected transition and observed transition should be compared to detect discrepancies, but the engine continues from the observed state.

## 3. Automatic screenshot gameplay loop

In live BPB mode, a new gameplay screenshot is an event that automatically invokes:

```text
observe
-> reconcile state
-> detect anomalies
-> evaluate board/shop
-> issue next action
-> await next observed state
```

Anomaly detection is not an endpoint. Unless one missing fact is genuinely decision-critical, every screenshot should terminate in an actionable recommendation without requiring the operator to ask what to do next.

## 4. State-transition valuation

Evaluate actions by the state they produce, not item value in isolation.

Conceptually:

`Q(s,a) = ImmediateCombatValue + ExpectedFutureStateValue`

Future state includes:

- displayed gold
- effective liquidity
- board space
- storage
- recipe progress
- sockets
- stamina balance
- build commitment
- remaining lives
- generated resources
- future shop distribution
- reachable transformations
- execution complexity

Items that alter future transitions or shop distributions are treated as state-transition modifiers rather than ordinary static-value items.

## 5. Explicit economy ledger

Maintain separate fields for:

- displayed gold
- expected gold after proposed action
- liquidatable storage value
- generated but unslotted assets
- committed purchase cost
- reroll cost

Arithmetic continuity is mandatory. A gold discontinuity triggers reconciliation before further strategic reasoning.

Do not confuse affordability with displayed gold only when stored assets are intentionally liquidatable.

## 6. Shadow price of board space

Board tiles are scarce and their marginal value rises as capacity becomes binding.

Conceptually:

`EffectiveCost = GoldCost + TilesUsed * SpaceShadowPrice + RearrangementCost`

Evaluate both gold efficiency and tile efficiency.

A bag can be highly valuable when it relaxes a binding space constraint. A cheap or synergistic item can still be bad if its tile burden displaces more useful combat value.

When the board is full, every purchase recommendation must include its displacement or staging logic. If the engine cannot identify what leaves the board and why the resulting state is stronger, the purchase is not yet actionable.

## 7. Real-option treatment of inventory

Stored or weak items may retain future option value.

Conceptually:

`KeepValue = CurrentValue + FutureOptionValue - Storage/SpaceCost`

versus

`SellValue = LiquidationValue + ImmediateLiquidityValue`

Dynamic item roles:

- CORE
- BRIDGE
- OPTION
- TEMPO FILLER
- LIQUIDATE

Roles are state-dependent. Stones, dust and similar low-value pieces must not be automatically liquidated merely because they are currently redundant. Open space, recipes or future tempo needs can change their marginal value later.

Before buying another cheap duplicate, explicitly check existing on-board and stored inventory.

## 8. Option expiry and remaining runway

Future option value decays as the probability of exercising it before run termination falls.

Conceptually:

`OptionValue = P(exercise before termination) * expected payoff`

Round, lives, gold, missing dependencies, board congestion and expected completion time affect whether latent options remain valuable.

A recipe component with high early optionality may become nearly worthless at one life if completion is unlikely before elimination.

## 9. Survival-constrained tempo

The objective is not maximum theoretical final-board strength. It is maximizing the probability of reaching the run win condition before exhausting lives.

Track near-term survival pressure explicitly.

As lives fall, future-value-heavy actions face a progressively higher hurdle. At one life, immediate deployed power dominates speculative economy and long-horizon scaling unless the future-value action also improves the next combat materially.

Tempo debt is therefore a survival constraint, not merely a soft penalty.

## 10. Binding-constraint test

Before reinforcing an existing synergy, ask:

> What is currently preventing this board from winning?

Do not assume that the most coherent upgrade is the highest-value upgrade.

Possible binding constraints include:

- damage / kill pressure
- survivability
- stamina
- activation speed
- anti-heal / disruption
- board-space congestion
- insufficient economy
- missing recipe bridge
- matchup-specific vulnerability

A coherent upgrade that increases an already abundant property can be dominated by a less synergistic upgrade that relieves the actual bottleneck.

The final Vampiress run exposed this directly: maximum health reached 404 while the terminal opponent finished at 153 / 157. More sustain did not solve the binding combat problem.

## 11. Discount discipline

Discount magnitude is not evidence of strategic fit.

Evaluate sale items as:

`NetUpgradeValue = CombatContribution + SynergyContribution + FutureOptionValue + Resale/EconomicValue - BoardSpaceCost - TransitionCost - StrategicDilution - ProcessComplexity`

A deep discount can make a mediocre item buyable, but it cannot erase a weak role in the actual build.

## 12. Value of Information protocol

Additional screenshots, mechanic lookups and operator questions have costs.

Conceptually:

`NetVoI = ExpectedDecisionImprovement - InformationAcquisitionCost`

Do not interrupt play to resolve uncertainty when all plausible interpretations imply the same action.

Unknown high-impact mechanics should be externally verified where feasible or explicitly carried at reduced confidence before irreversible recommendations.

Known mechanics should not be re-requested from the operator.

## 13. Silence neutrality

Absence of operator correction provides no positive evidence that the recommendation was correct.

Silence may reflect time pressure, attention limits, disengagement or deliberate non-intervention. Confidence should update from explicit observations, mechanics and outcomes, not from lack of objection.

The operator may be cooperative, mistaken, inattentive or adversarial. Player input remains evidence to evaluate, not ground truth by default.

## 14. Separate climb and experiment objectives

Distinguish:

- CLIMB: maximize current-run performance; learning value has minimal weight.
- EXPERIMENT: controlled deviations may be justified where expected information gain is high.

Do not silently sacrifice climb EV for exploration during a CLIMB run.

## 15. Rolling-horizon planning

Use bounded lookahead rather than pretending the complete run can be solved exactly through the LLM interface.

Suggested horizon:

1. immediate action and combat effect
2. likely next-shop consequences
3. major reachable transformation / subclass / economy consequences
4. approximate terminal build value

Conceptually:

`Q ≈ Immediate + NextShop + MajorTransition + TerminalApproximation`

Recompute after every observed state transition.

## 16. Endogenous shop probability

Reroll EV depends on the current shop-generating state.

Conceptually:

`P(item | reroll) = f(round, rarity distribution, class, subclass, conditional pools, deployed pool modifiers, current state)`

Items such as Box of Riches that change future shop eligibility or distribution carry an explicit opportunity-distribution effect.

Reroll evaluation therefore considers both expected item quality and lost purchasing capacity.

## 17. Stateful-generator items

Items that generate resources or alter future shops create persistent accounting obligations.

For each such item track:

- generated asset each round/shop
- whether it is deployed or stored
- socket / combine / filler / liquidation options
- effective liquidity contribution
- effect on future shop distribution
- board-space cost
- process complexity

Do not automatically blacklist complex items. Their process burden is a cost of the current interface, not proof that they are strategically weak.

## 18. Recipe graph and reachability

Represent recipes and transformations as a directed graph rather than isolated remembered combinations.

Each edge may carry:

- acquisition cost
- missing-component probability
- required board/storage space
- intermediate combat strength
- completion time
- opportunity cost
- transformation geometry

Powerful endpoints are discounted when realistic reachability is low.

Before rerolling or buying generic value, recheck live downstream recipes from already-owned items.

## 19. Human/LLM execution risk

Distinguish theoretical strategy EV from realized EV under the screenshot + LLM + human interface.

Conceptually:

`RealizedEV = TheoreticalEV - ExecutionRiskCost`

Execution risk includes:

- accidental actions
- state-memory failures
- difficult persistent accounting
- geometry complexity
- ambiguous screenshots
- time pressure
- correction burden

This is an interface/system limitation, not evidence that a mechanically complex strategy is intrinsically inferior.

## 20. Terminal mismatch logging

A combat result is not just win/loss.

When visible, record:

- predicted win probability or qualitative expectation
- actual result
- player remaining health
- opponent remaining health
- stamina state
- salient buffs/debuffs
- whether the fight was close or structurally one-sided

Large mismatches should update the board-strength model more strongly than narrow losses.

The terminal Vampiress fight, with 404 maximum health and an opponent finishing at 153 / 157, is treated as a structural mismatch rather than ordinary variance.

## 21. Integrated action score

Conceptually:

`Q(s,a) = CombatValue + FutureStateValue + OptionValue + InformationValue - GoldCost - SpaceShadowCost - TempoDebt - ExecutionRisk - TransitionCost`

subject to a state-dependent survival constraint and a binding-constraint check.

This is a conceptual decomposition. Do not invent arbitrary numerical weights or false precision without calibration data.

## 22. Live decision procedure

1. Observe screenshot.
2. Reconcile state against prior canonical state.
3. Detect arithmetic, inventory, geometry or execution discontinuities.
4. Update confirmed / inferred / unknown variables.
5. Identify binding constraints: lives, gold, space, stamina, damage, sustain, dependencies.
6. Generate reachable actions: purchases, sells, deployment, storage, recipes, locks, rerolls, hold, combat.
7. Eliminate clearly dominated actions.
8. Evaluate immediate survival and combat impact.
9. Evaluate bounded future-state consequences and recipe reachability.
10. Add option value and opportunity-distribution effects.
11. Subtract space, transition, execution and liquidity costs.
12. Acquire additional information only when expected VoI justifies interruption.
13. Issue one actionable recommendation or a short ordered sequence.
14. Record forecast/confidence where useful.
15. Await next observed state and repeat.

## 23. Evidence status

High-confidence process rules from repeated direct observation:

- observed-state supremacy
- automatic screenshot gameplay loop
- explicit economy continuity
- known-item persistence
- silence neutrality
- full-board displacement accounting

Promising but still calibration-dependent strategic rules:

- binding-constraint weighting
- process-complexity penalty
- option-expiry strength
- endogenous reroll valuation
- stateful-generator valuation

These should be monitored across subsequent runs rather than treated as perfectly calibrated.

## 24. Relationship to strategy priors

DM-0.5 changes the reasoning architecture. It does not replace H-series strategy hypotheses.

No H-series update is promoted from the Vampiress run alone. One poor run is insufficient evidence that a strategy prior is wrong. Future strategy updates should separate:

- strategy failure
- execution failure
- state-reconstruction failure
- economy/geometry failure
- matchup variance
- model-calibration failure
