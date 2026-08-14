# Decision Model DM-0.3

**Status:** active  
**Patch context:** 1.1.8  
**Strategy prior context:** H2

DM-0.3 extends DM-0.2 after live-run structural failures exposed gaps in how the engine represented the action space. The changes below are intentionally Player-agnostic: they are general reasoning corrections, not attempts to imitate Player strategy preferences.

## 1. Base policy

DM-0.2 remains the base policy: phase-dependent tempo and option value, Bayesian-style strategy convergence, explicit commitment states, agency-adjusted random-value handling, non-automatic combining, conservative late-board intervention, and ex-ante decision evaluation.

## 2. Reachable-state search

At each shop, evaluate **reachable board states**, not only visible items in isolation.

Generate a lightweight candidate set from:

- owned + owned recipes
- owned + shop recipes
- shop + shop recipes
- lock/unlock of pending recipes
- deploy/store of owned items
- sell/replace actions
- reroll
- hold

Compare resulting states using:

- immediate power
- end-state/path value
- transition cost
- space delta
- stamina delta
- dependency burden
- liquidity after action
- reversibility

The engine should not miss a Poison Dagger because Dagger and Poison Potion were individually weak, or forget a Torch transformation because it was deferred several shops earlier.

## 3. Persistent option recheck

A prior **NOT NOW** decision remains open until the option is completed, deliberately abandoned, or made impossible.

At every shop, re-score:

- pending recipes
- locked transformations
- stored anchor items
- deferred deployment decisions
- known upgrade edges involving owned items

Earlier decisions are not permanent defaults.

## 4. Neutral Player-input handling

Player questions are **audit triggers**, not evidence that an error exists.

Input weighting:

- verified mechanic correction: high weight
- direct observation/missing variable: expand the state, then recompute
- strategic recommendation: very low prior weight
- challenge/question: neutral re-scan only

Do not manufacture a correction because Player asks whether something was missed. Agreement with Player is not an objective.

## 5. Dynamic reroll threshold

Reroll EV must be recomputed whenever the economic state changes.

Compare:

`best current reachable state`

against

`expected next-shop improvement - reroll cost - lost purchasing capacity`

The hurdle for rerolling rises when:

- reroll price rises
- liquid gold falls
- the current shop already contains strong universal value
- the current board is adequate enough that search urgency is low

A 2g reroll at 8g liquidity is not evaluated using the same threshold as a 1g reroll at 23g.

## 6. Directionality as critical mass

Do not force commitment on a fixed round number. Directionality should rise when independent signals converge.

Track:

- anchor quality
- bridge/recipe progress
- supporting synergy density
- transition affordability
- intermediate tempo
- reconstruction cost of alternatives
- remaining runway

Strategic states become:

`OPEN -> LEANING -> DIRECTIONAL -> COMMITTED -> ECONOMICALLY LOCKED`

A single attractive component may create a lean. Several independently useful purchases pointing toward the same endpoint can create critical mass.

## 7. Pivot accounting

A pivot is evaluated using the **full transition**, not merely endpoint strength.

`Pivot value = alternative expected value - transition cost - temporary tempo loss - stranded investment`

Sunk cost is a switching-cost modifier, not a veto. Its importance is state-dependent: stranding 4g early can be severe; the same 4g may be minor in a gold-rich mid/late shop.

## 8. Deterministic-node lookahead

Known future forced-choice events affect current valuation before they occur.

Examples include subclass choices, skill choices, or other scheduled decision nodes.

Track distance to the node and apply a forward-looking compatibility term that increases as the node approaches. Do not prematurely force a subclass path simply because the node exists.

## 9. Transformation and lock protocol

Before issuing **COMBAT**, explicitly check:

- any pending owned recipes
- whether components are locked or unlocked
- whether a desired fusion should be enabled or suppressed
- whether a stored item should be deployed
- whether the transformation changes geometry or stamina enough to alter the decision

Fusion timing is part of the action model, not housekeeping.

## 10. Combat forecast and capture policy

Before each combat issue a win-probability forecast.

Default data capture is lightweight:

- predicted win probability
- actual win/loss

Request/share a combat-result screenshot when:

- the outcome contradicts the forecast
- the result is unusually close
- the fight tests a major build transition
- repeated unexplained patterns emerge

Forecast calibration is evaluated separately from decision correctness. One fight does not prove a purchase or strategy right or wrong.

## 11. State reconciliation

Use lightweight reconciliation rather than forensic reconstruction.

Confirmed state persists unless contradicted by strong evidence. Visual recognition must not overwrite a confirmed identity without checking transaction history, shop availability, and gold when cheap to do so.

## 12. Revision discipline

DM-0.3 is justified by repeated structural reasoning failures across different decision types: missed reachable transformations, stale deferred decisions, state-insensitive reroll thresholds, and conversational contamination from audit questions.

Future changes still require repeated evidence, a strong independent theoretical case, or verified mechanics. Player preference alone is never sufficient.