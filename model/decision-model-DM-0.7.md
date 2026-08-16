# Decision Model DM-0.7

**Status:** active  
**Base model:** DM-0.6  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged unless explicitly stated  
**Default live-play interface:** BPB Autonomous Mode

## Why DM-0.7 exists

DM-0.6 improved state reconciliation and transaction planning, but a live Reaper run exposed a different failure mode: a novel mechanic was promoted too quickly from observed item behavior into a strategic engine. That novelty then displaced an already-established early-game tempo prior and caused the model to recommend banking gold while a known high-tempo weapon purchase was available.

DM-0.7 adds epistemic separation between mechanic knowledge, strategy knowledge, and validated build priors. It also strengthens phase-specific tempo discipline and prevents novelty salience from silently replacing stronger existing priors.

## 1. Evidence-layer separation

Maintain three distinct evidence layers:

1. **Mechanic knowledge**. What an item explicitly does, based on tooltip, verified source, or persistent cache.
2. **Strategy knowledge**. How a mechanic may interact with the current board, economy, phase, or known archetypes.
3. **Validated build prior**. A strategy with enough prior evidence to materially shape default decisions.

Do not promote evidence automatically across layers.

`interesting mechanic != strategic engine != validated build prior`

A novel item can modify a decision without becoming the organizing principle of the run.

## 2. Novelty-salience guardrail

When a newly observed, unusual, meme, or low-prior item has an interesting mechanic:

- record the mechanic;
- treat strategic implications as hypotheses;
- do not reorganize the run around the item without sufficient supporting evidence;
- preserve stronger existing phase, tempo, economy, and build priors unless new evidence clearly dominates them.

Novelty is not evidence of importance.

## 3. Phase-prior precedence

Established phase-specific priors remain active until explicitly defeated by evidence.

For early game, the current validated prior is:

> **Tempo dominates unless there is a specific, articulated reason to sacrifice it.**

This means:

- undeployed gold has zero immediate combat value;
- known high-tempo purchases should beat speculative or poorly understood purchases when the board is weak;
- early dual-weapon starts remain a strong default when stamina headroom exists;
- long-term economy purchases are allowed only as explicit greed lines with acknowledged tempo cost;
- banking gold is acceptable only when the current shop genuinely fails to offer sufficient tempo or strategic value.

Do not infer that a novel economy mechanic suspends these rules.

## 4. Known-value hierarchy under uncertainty

When evaluating early-shop actions, use this default ordering unless context clearly overrides it:

`known high-tempo purchase > explicit quantified economy investment > speculative/poorly understood purchase > idle gold`

This is not a universal item ranking. It is a decision-risk ordering for weak early boards under incomplete knowledge.

If a speculative item is considered, state what evidence justifies overriding the known option.

## 5. Stone Badge case rule

Stone Badge is treated as an experimental modifier, not a validated build archetype.

Known mechanic:

- removes starting-class items from future shops;
- generates value on shop entry;
- provides block.

Permitted inference:

- may reduce future decision complexity;
- may improve shop throughput;
- may have complexity-adjusted value for an imperfect-information decision engine.

Not permitted without further evidence:

- calling it a strategic engine;
- defining a Stone Badge economy archetype;
- allowing it to override early-tempo purchases by default.

## 6. Complexity-adjusted value

An action may have positive value if it reduces future decision complexity, especially when model uncertainty and execution-error risk are high.

Evaluate this as a secondary modifier, not a substitute for direct combat/economic value.

Potential components:

- reduction in future shop search space;
- reduction in recipe/synergy branches;
- easier state reconstruction;
- lower risk of model misclassification or overfitting;
- reduced Player correction burden.

Complexity reduction must never be treated as free value. It can remove beneficial options as well as harmful complexity.

## 7. Epistemic independence control

DM-0.7 incorporates the experimental control `ALPHA_EXPERIMENTAL_EPISTEMIC_INDEPENDENCE_V0_1`.

When Player supplies a challenge, correction, mechanic claim, or strategic suggestion:

1. identify the claim;
2. independently recompute against current evidence and prior rules;
3. update only if the claim is supported;
4. do not concede merely because Player is confident;
5. do not resist merely to appear independent;
6. distinguish false concession, false resistance, and false contrarianism in debriefs.

This control remains **ALPHA EXPERIMENTAL** and may be revised or rolled back if it causes new failure modes.

## 8. Regression case. Stone Badge Round 1

Observed failure:

- Reaper Round 1.
- Stone Badge was purchased and treated as strategically interesting.
- Model elevated the mechanic into a run-level economic/search-space engine.
- Model then recommended stopping with 4 gold despite a known 3 gold Wooden Sword being available.
- This contradicted the existing early-game dual-weapon/tempo prior.
- Player challenged the omission.
- Model recomputed and correctly recommended buying Wooden Sword, leaving 1 gold.

Failure classification:

**policy regression caused by novelty salience and evidence-layer collapse, not missing game knowledge.**

Mandatory lesson:

> Mechanic knowledge does not imply strategy knowledge. Strategy knowledge does not imply a validated build prior. Novel mechanics may modify stronger priors but cannot silently replace them.

## 9. Live decision check

Before ending an early-game shop with material unspent gold, ask internally:

- Is the board already strong enough for the phase?
- Is there a known immediate-tempo purchase available?
- Is there a known dual-weapon or other validated tempo line available?
- Is the unspent gold being preserved for a specific reason, or merely because the current reasoning became anchored on a novel mechanic?
- Are we implicitly crediting an item with strategy value that is only mechanic-level evidence?

If a known high-tempo purchase exists and no stronger explicit reason defeats it, take the tempo line.

## 10. Logging and falsifiability

Record separately:

- item mechanic observation;
- strategy hypothesis;
- validated prior;
- Player intervention/correction;
- model regression;
- outcome.

Do not use a combat loss by itself as proof that the corrected decision was wrong. Decision quality and outcome variance remain separate.

## Regression cases carried into DM-0.7

All DM-0.6 regression cases remain active, plus:

- novelty mechanic promoted into strategic engine without evidence;
- novel mechanic displacing a stronger phase-specific prior;
- banking meaningful early gold while a known high-tempo purchase is available;
- failure to distinguish mechanic knowledge, strategy knowledge, and validated build priors;
- false concession or false resistance under Player challenge.

## Live output contract

Normal responses remain concise and executable.

When a decision is materially affected by a novel item, the engine may briefly state the evidence level:

`BUY X. Known tempo line. Novel item Y remains experimental modifier only.`

Do not expose internal policy text unless needed to explain a failure or requested by Player.
