# Decision Model DM-0.5 — STAGING DRAFT

**Status:** staging / not active  
**Base model:** DM-0.4  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged  

## Purpose

This document stages candidate changes discovered during the current live run and subsequent research into sequential decision-making under uncertainty. It MUST NOT be treated as the active decision policy until the current run is completed, remaining observations are incorporated, and the revision is explicitly promoted.

## Research basis

The proposed architecture draws on established work in partially observable Markov decision processes (POMDPs), approximate dynamic programming, rolling-horizon optimization, value of information, real-options reasoning, and exploration/exploitation. These are conceptual transfers to Backpack Battles, not game-specific empirical findings.

## 1. Belief-state representation

Do not equate the reconstructed screenshot state with ground truth. Material variables should be classified as:

- CONFIRMED
- INFERRED
- UNKNOWN

Where decision-relevant, attach an approximate confidence or probability distribution. Decisions should operate on the belief state rather than silently assuming uncertain visual recognition or mechanics are correct.

## 2. State-transition valuation

Evaluate actions by the state they produce, not item value in isolation.

Candidate formulation:

`Q(s,a) = ImmediateCombatValue + ExpectedFutureStateValue`

Future state includes gold, board space, storage, recipe progress, sockets, stamina balance, build commitment, remaining lives, generated resources, future shop distribution, and reachable transformations.

Items that alter future transitions or shop distributions should be represented as state-transition modifiers rather than ordinary static-value items.

## 3. Shadow price of board space

Board tiles are a scarce resource whose marginal value rises as capacity becomes binding.

Candidate formulation:

`EffectiveCost = GoldCost + TilesUsed * SpaceShadowPrice + RearrangementCost`

Evaluate both gold efficiency and tile efficiency. Bag purchases can have value above their nominal capacity because they relax a binding constraint. Conversely, persistent low-density items incur an implicit opportunity cost even after purchase cost is sunk.

## 4. Real-option treatment of inventory

Stored or currently weak components may retain future option value.

Candidate formulation:

`KeepValue = CurrentValue + FutureOptionValue - Storage/SpaceCost`

versus

`SellValue = LiquidationValue + ImmediateLiquidityValue`

Dynamic item roles:

- CORE
- BRIDGE
- OPTION
- TEMPO FILLER
- LIQUIDATE

Roles are state-dependent rather than permanent.

## 5. Option expiry and remaining runway

Future option value should decay as the probability of exercising the option before run termination falls.

Candidate formulation:

`OptionValue = P(exercise before termination) * expected payoff`

Round, remaining lives, gold, missing dependencies, board congestion, and expected completion time should therefore affect whether latent recipe/build options remain valuable.

## 6. Survival-constrained tempo

The objective is not simply maximum theoretical final-board strength. The operational objective is closer to maximizing the probability of reaching the run's win condition before exhausting lives.

Introduce explicit near-term survival probability:

`P(survive | state, action)`

As remaining lives fall, future-value-heavy actions should face a progressively higher hurdle. Tempo debt therefore becomes a survival constraint rather than merely a soft scoring penalty.

## 7. Value of Information protocol

Human questions, additional screenshots, mechanic lookups, and other information-gathering actions have costs.

Candidate formulation:

`NetVoI = ExpectedDecisionImprovement - InformationAcquisitionCost`

Do not interrupt play to resolve an uncertainty when all plausible interpretations produce the same action. Escalate when different plausible interpretations materially change the recommendation.

Unknown high-impact mechanics should be verified externally where feasible or explicitly carried at reduced confidence before irreversible recommendations.

## 8. Separate climb and experiment objectives

Across runs, distinguish:

- CLIMB: maximize current-run performance; learning value has minimal weight.
- EXPERIMENT: controlled deviations may be justified where expected information gain is high.

Do not silently sacrifice climb EV for exploratory learning during a run classified as CLIMB.

## 9. Rolling-horizon planning

Do not attempt exhaustive full-run optimization through an LLM interface. Use bounded lookahead and repeatedly recompute as observations arrive.

Suggested horizon:

1. immediate action and combat effect
2. likely next-shop consequences
3. major reachable transformation / subclass / economy consequences
4. approximate terminal build value

Candidate approximation:

`Q ≈ Immediate + NextShop + MajorTransition + TerminalApproximation`

## 10. Endogenous shop probability

Reroll EV must depend on the current shop-generating state rather than a stationary generic item distribution.

Conceptually:

`P(item | reroll) = f(round, rarity distribution, class, subclass, conditional pools, deployed pool modifiers, current state)`

Items that change future shop eligibility or probability distributions must carry an explicit `opportunity-distribution effect`.

Candidate reroll formulation:

`EV(roll) = Σ P(item_i | state) * V(item_i,state) - roll cost - lost purchasing capacity`

## 11. Recipe graph and reachability

Represent recipes and transformations as a directed graph rather than isolated remembered combinations.

Each edge should carry, where relevant:

- acquisition cost
- missing-component probability
- required board/storage space
- intermediate combat strength
- completion time
- opportunity cost
- transformation geometry

Powerful endpoints should be discounted when realistic reachability is low.

## 12. Human/LLM execution risk

Distinguish theoretical strategy EV from realized EV under the current screenshot + LLM + human interface.

Candidate formulation:

`RealizedEV = TheoreticalEV - ExecutionRiskCost`

Execution risk includes accidental actions, state-memory failures, difficult persistent accounting, geometry complexity, ambiguous screenshots, time pressure, and correction burden.

This is an interface/system limitation, not evidence that a mechanically complex game strategy is intrinsically inferior. Preserve the distinction between an unconstrained theoretical engine and the currently executable human/LLM policy.

## 13. Candidate integrated action score

A staged conceptual objective is:

`Q(s,a) = CombatValue + FutureStateValue + OptionValue + InformationValue - GoldCost - SpaceShadowCost - TempoDebt - ExecutionRisk - TransitionCost`

subject to a state-dependent survival constraint.

This is a conceptual decomposition, not yet a calibrated numerical formula. Do not create false precision by assigning arbitrary weights without evidence.

## 14. Candidate decision procedure

1. Reconcile state and confidence.
2. Identify binding constraints: gold, space, stamina, lives, dependencies.
3. Generate reachable actions including purchases, sells, deployment/storage, recipes, transformations, locks, and rerolls.
4. Eliminate clearly dominated actions.
5. Evaluate immediate survival and tempo.
6. Evaluate bounded future-state consequences.
7. Add option value and opportunity-distribution effects.
8. Subtract space, transition, execution, and liquidity costs.
9. Acquire additional information only when expected VoI justifies interruption.
10. Select action and record forecast/confidence.
11. Update beliefs after shop/combat observations.

## 15. Promotion gate

DM-0.5 remains STAGING until:

- the current run is completed;
- remaining run observations and prediction mismatches are incorporated;
- proposed rules are checked for contradictions with DM-0.4;
- any game-mechanic claims used operationally are verified where necessary;
- complexity is reviewed to ensure the model remains executable in live screenshot play;
- an explicit decision is made to promote, revise, or reject the staged model.

No H-series strategy prior change is proposed at this stage. The current evidence primarily concerns decision-process quality under uncertainty and constrained execution, not sufficient evidence that the underlying strategic prior is wrong.
