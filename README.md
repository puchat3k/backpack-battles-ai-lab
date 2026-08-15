# Backpack Battles AI Lab

> An experiment in AI-directed Backpack Battles play.

**Status:** paused indefinitely after Run #003  
**Canonical game patch:** 1.1.8  
**Strategy prior set:** H2  
**Decision model:** DM-0.6  
**Reasoning model:** GPT-5.6 Sol

## Purpose

This project asks a deliberately narrow question: **how effectively can a general-purpose reasoning model direct real Backpack Battles runs through screenshots and a human operator, without pretending to be a deterministic game engine?**

The objective is not to publish a magic tier list or claim solved optimal play. The experiment combines patch-aware strategy priors, adaptive decision making, probability and decision theory, human execution, and empirical run logging. The model makes the gameplay decisions. **Player** supplies screenshots and executes instructions in the game.

Claims should remain falsifiable, versioned, and evidence-calibrated. Strategy scores are hypotheses, not measured win rates. Community corrections and disagreements are useful inputs, not authority signals.

## Current hypothesis

For each of the seven playable characters, **H2 contains three patch-relevant strategy priors, for 21 total**. These are starting hypotheses rather than forced build paths. The model may freestyle, converge toward a prior, discover an emergent line, pivot, or remain open depending on the actual shop and board state.

## Experiment architecture

- Structured experimental records track runs, decisions, strategy hypotheses, model versions, belief states, confidence, transition variables, and outcomes.
- Run captures and source material are retained separately from interpretation so later strategy revisions remain traceable.
- This repository contains the public methodology, strategy documentation, decision logic, anonymized run logs, community discussion, and reproducibility material.

## Repository files

- [`patches/1.1.8.md`](patches/1.1.8.md): canonical raw balance changes used for H2
- [`model/strategies-H2.md`](model/strategies-H2.md): 21 active strategy priors
- [`model/decision-model-DM-0.6.md`](model/decision-model-DM-0.6.md): latest decision policy tested before pause
- [`model/decision-model-DM-0.3.md`](model/decision-model-DM-0.3.md): earlier decision policy
- [`model/decision-model-DM-0.2.md`](model/decision-model-DM-0.2.md): earlier decision policy
- [`docs/run-schema.md`](docs/run-schema.md): public logical experiment schema
- [`docs/run-001-anonymized-log.md`](docs/run-001-anonymized-log.md): anonymized Run #001 checkpoint
- [`docs/run-002-vampiress-live-log.md`](docs/run-002-vampiress-live-log.md): Run #002 live decision log and review
- [`docs/run-003-ranger-debrief.md`](docs/run-003-ranger-debrief.md): Run #003 debrief, failure analysis, Player assessment, and pause rationale
- [`docs/process-observations.md`](docs/process-observations.md): cross-run process observations
- [`custom-gpt/SETUP.md`](custom-gpt/SETUP.md): community Custom GPT setup and reproduction guide
- [`custom-gpt/INSTRUCTIONS.md`](custom-gpt/INSTRUCTIONS.md): public behavioral instruction set

## Decision model

DM-0.3 retained the phase-dependent objective from DM-0.2 while adding explicit reachable-state reasoning. Later models extended the framework through DM-0.6 with stronger state reconciliation, deployment accounting, transaction planning, Player-input discipline, and autonomous execution rules.

Run #003 showed that documenting those safeguards did not reliably enforce them during live inference. The experiment is therefore paused rather than proceeding automatically to DM-0.7.

### Early game: tempo

Useful board power is normally preferred to excessive rolling for ideal pieces. Rerolling must clear the opportunity cost of the best available reachable state plus the value of preserving gold.

### Mid game: convergence and critical mass

The model maintains comparative beliefs over known strategy priors, open play, and emergent lines. Strategic states are:

`OPEN -> LEANING -> DIRECTIONAL -> COMMITTED -> ECONOMICALLY LOCKED`

Directionality is not forced by a fixed round. It increases when independent signals converge: anchor quality, recipe progress, support density, affordability, intermediate tempo, reconstruction cost of alternatives, and remaining runway.

### Late game: system optimization

Synergy, matchup robustness, scaling, and intervention risk matter more. A proven full board has an explicit status-quo option:

`HOLD / LOCAL OPTIMIZE / RESTRUCTURE`

The burden of proof for disruptive surgery rises as demonstrated board robustness and switching costs rise.

## Reachable-state reasoning

DM-0.3 evaluates **reachable board states**, not only visible shop items.

The candidate set can include:

- owned + owned recipes
- owned + shop recipes
- shop + shop recipes
- lock/unlock of pending recipes
- deploy/store of owned items
- sell/replace actions
- reroll
- hold

This matters because individually weak components can create a strong immediate transformation, and deferred recipes can remain live for several shops.

A prior **NOT NOW** remains open until completed, deliberately abandoned, or made impossible.

## Freestyling

When no destination is sufficiently dominant, the model generates candidate actions across exploit-current, preserve-optionality, pivot, exploratory, and tempo modes. Candidate lines are compared using immediate power, future option value, synergy density, assembly friction, economy efficiency, space efficiency, timing coherence, counter coverage, transition cost, ceiling, and execution complexity.

An unmodeled emergent line is allowed to beat the precomputed H2 priors when current evidence supports it. **H2 is a prior over useful destinations, not a rail system.**

## Pivot and transition accounting

A pivot is evaluated as a full transition rather than a comparison of theoretical endpoints:

`alternative expected value - transition cost - temporary tempo loss - stranded investment`

Sunk cost is a switching-cost modifier, not a veto. Its significance depends on current economy and phase.

## Reroll economics

Gold is both purchasing power and option value. DM-0.3 recomputes the reroll threshold whenever reroll price, liquidity, board strength, or shop quality changes.

A 2g reroll with low liquidity is not evaluated using the same threshold as a 1g reroll with abundant gold.

## Future decision-node lookahead

Known deterministic decisions such as subclass or skill choices can affect current item valuation before they arrive. Their influence increases as the node approaches, but the model should not force a future path prematurely.

## Combining and locks

Combining is a commitment decision, not an automatic upgrade. Available actions are:

`COMBINE NOW / HOLD COMPONENTS / ABANDON RECIPE`

Before combat, the engine explicitly checks pending recipes, lock/unlock state, whether a fusion should be enabled or suppressed, stored-item deployment, and resulting geometry/stamina implications.

## Economy and random value

Random-item mechanics are evaluated as conversions of agency or liquidity into stochastic value. Expected generated value is discounted by usability, lost agency, lost liquidity, space pressure, and stranded-item risk. Random value is generally easier to exploit early, when the acceptance range is broad, than late, when synergy requirements are tighter.

## Placement and human execution

The experiment accepts imperfect human execution. The model does heavy strategic work and medium placement work rather than turning Player into a coordinate-following robot.

Placement instructions use **Critical / Target / Minimum acceptable** tolerances. An extra screenshot is requested only when uncertainty about the resulting board could materially affect expected outcome.

## Combat forecasting

Before combat, the model issues a pre-outcome win-probability estimate. Default capture is lightweight: forecast plus actual win/loss.

Combat screenshots are most useful when:

- the outcome contradicts the forecast
- the result is unusually close
- a major build transition is being tested
- repeated unexplained patterns emerge

Forecast calibration is evaluated separately from decision correctness. One combat does not prove the preceding decision right or wrong.

## Run protocol

Each run is initialized with the game patch, strategy-prior version, decision-model version, reasoning-model version, character, starting rank, and selection mode (`random` or `targeted`).

The default live interface is intentionally concise:

`Buy / Sell / Roll / Combine / Lock-Unlock / Deploy-Store / Hold / Placement priority / Why`

Detailed reasoning is logged rather than repeatedly dumped into the operator interface.

## Player-input discipline

Player gameplay judgment is deliberately not treated as an authority signal.

- verified mechanics can update the state immediately
- observations can add missing variables
- strategic recommendations remain low-weight hypotheses
- questions trigger a neutral audit rather than an assumption that the model is wrong

The model then recomputes independently. Meaningful Player/model divergences are retained for analysis.

Run #003 showed that the live model did not reliably maintain this distinction. Player hypotheses could still be absorbed too readily as model state, which is now treated as a first-class experimental failure rather than a solved policy problem.

## Measurement

Decision quality is evaluated **ex ante** separately from stochastic combat outcomes. Major decisions may record confidence so calibration can later be tested.

Results should be segmented by `patch x H-version x DM-version x reasoning-model version x character x selection mode`. Small-N swings should not trigger policy revisions.

## Policy-change rule

Do not revise the decision model because of isolated outcomes or Player preference. A material change requires repeated evidence, a clear independent theoretical justification, or a strong externally verified correction. Every material logic change receives a new DM version.

The Run #003 pause adds an additional practical constraint: another prompt-level revision is not justified merely because DM-0.6 failed. Any resumption should first test whether structured game state, deterministic validation, and a stronger item/mechanics database are required.

## Reproducibility

The public package includes methodology, versioned decision logic, strategy priors, run schema, patch source notes, anonymized run logs, and Custom GPT setup files so others can reproduce or fork the workflow. This is **not** represented as a standalone trained Backpack Battles model.

## Privacy boundary

This repository is public and **Backpack Battles only**. The human participant is referred to publicly as **Player**. No real-world identity data, unrelated private project information, third-party confidential data, private correspondence, credentials, or incidental operational metadata belongs here.

## Changelog

### DM-0.1
Initial adaptive decision policy. Added phase-dependent play, tempo-first early game, strategy beliefs and commitment states, pivot economics, conservative late-board intervention, survival logic, combining, stochastic-value treatment, and best-effort human placement.

### DM-0.2
Added explicit freestyling candidate generation and the intermediate convergence model. Preserved belief confidence separately from economic commitment and allowed emergent strategies to compete with precomputed priors.

### DM-0.3
Added reachable-state search, persistent re-check of deferred transformations, neutral Player-question handling, dynamic reroll thresholds, critical-mass directionality, full pivot-transition accounting, deterministic-node lookahead, explicit lock/unlock checks, and pre-combat forecast calibration.

### DM-0.4 to DM-0.6
Successive revisions added stronger economic-state accounting, board-space and deployment checks, staged transformations, generator valuation, tempo-debt tracking, full-state gates, transaction planning, storage discipline, autonomous action semantics, and recovery/surrender logic. Run #003 demonstrated that several of these documented safeguards did not reliably execute in live play.

### H2
Recalculated for patch 1.1.8. Twenty-one active strategy priors across seven characters. Patch-sensitive deltas applied without claiming simulated or empirical optimality.

## Current experiment state

**The experiment is paused indefinitely after Run #003.** Run #003 used DM-0.6 and was terminated voluntarily by Player after repeated state-management, space-accounting, action-completeness, and evidence-calibration failures. The approved debrief concludes that another natural-language policy revision is not currently justified. A future resumption should first test whether the experiment needs a verified item/mechanics database, structured symbolic game state, deterministic legality/geometry validation, and a lower-friction execution interface.