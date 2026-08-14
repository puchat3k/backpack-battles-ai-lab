# Backpack Battles AI Lab

> An experiment in AI-directed Backpack Battles play.

**Status:** pre-live experimental build  
**Canonical game patch:** 1.1.8  
**Strategy prior set:** H2  
**Decision model:** DM-0.2  
**Reasoning model:** GPT-5.6 Sol

## Purpose

This project asks a deliberately narrow question: **how effectively can a general-purpose reasoning model direct real Backpack Battles runs through screenshots and a human operator, without pretending to be a deterministic game engine?**

The objective is not to publish a magic tier list or claim solved optimal play. The experiment combines patch-aware strategy priors, adaptive decision making, probability and decision theory, human execution, and empirical run logging. The model makes the gameplay decisions. The operator supplies screenshots and executes instructions in the game.

The project may be controversial in parts of the community. Claims should therefore remain falsifiable, versioned, and evidence-calibrated. Strategy scores are hypotheses, not measured win rates. Community corrections and disagreements are useful inputs, not threats to the experiment.

## Current hypothesis

For each of the seven playable characters, **H2 contains three patch-relevant strategy priors, for 21 total**. These are starting hypotheses rather than forced build paths. The model may freestyle, converge toward a prior, discover an emergent line, pivot, or remain open depending on the actual shop and board state.

The current human benchmark is sustained high Platinum to low Diamond. This is a benchmark for practical performance, not a scientific control arm.

## System architecture

- **Neon Postgres** stores structured experimental data: runs, decisions, strategy hypotheses, decision-model versions, belief states, confidence, economic and transition variables, and outcomes.
- **Google Drive** stores run captures and source material. Raw patch notes are preserved separately from our interpretation so later strategy revisions remain traceable.
- **GitHub** is the publication layer for methodology, strategy documentation, decision logic, changelog, community discussion, and reproducibility material. The database remains the operational source of structured run evidence.

## Repository files

- [`patches/1.1.8.md`](patches/1.1.8.md): canonical raw balance changes used for H2
- [`model/strategies-H2.md`](model/strategies-H2.md): 21 active strategy priors
- [`model/decision-model-DM-0.2.md`](model/decision-model-DM-0.2.md): active decision policy
- [`docs/run-schema.md`](docs/run-schema.md): public logical experiment schema
- [`custom-gpt/SETUP.md`](custom-gpt/SETUP.md): community Custom GPT setup and reproduction guide
- [`custom-gpt/INSTRUCTIONS.md`](custom-gpt/INSTRUCTIONS.md): public behavioral instruction set
- [`PUBLICATION_BOUNDARY.md`](PUBLICATION_BOUNDARY.md): strict privacy and publication boundary

## Decision model

DM-0.2 uses a phase-dependent objective rather than one fixed policy.

### Early game: tempo
Useful board power is normally preferred to excessive rolling for ideal pieces. Rerolling must clear the opportunity cost of the best available purchase plus the value of preserving gold.

### Mid game: convergence
The model maintains comparative beliefs over known strategy priors, open play, and emergent lines. Strategic states are:

`OPEN -> LEANING -> COMMITTED -> ECONOMICALLY LOCKED`

Belief confidence and economic commitment are deliberately separate variables.

### Late game: system optimization
Synergy, matchup robustness, scaling and intervention risk matter more. A proven full board has an explicit status-quo option:

`HOLD / LOCAL OPTIMIZE / RESTRUCTURE`

The burden of proof for disruptive surgery rises as demonstrated board robustness and switching costs rise.

## Freestyling

When no destination is sufficiently dominant, the model generates candidate actions across exploit-current, preserve-optionality, pivot, exploratory and tempo modes. Candidate lines are compared using immediate power, future option value, synergy density, assembly friction, economy efficiency, space efficiency, timing coherence, counter coverage, transition cost and ceiling.

An unmodeled emergent line is allowed to beat the precomputed H2 priors when current evidence supports it. **H2 is a prior over useful destinations, not a rail system.**

## Convergence and commitment

Beliefs are updated Bayesian-style using heuristic likelihoods until sufficient empirical data exists for calibration. No universal percentage forces commitment. Phase, hearts, gold, board strength, missing components, switching cost and alternative quality all matter.

Commit when exploiting the leading strategy has greater expected value than preserving remaining options. Pivot when the expected incremental value after switching costs exceeds continuing. Economic lock occurs when switching is negative EV even if another theoretical destination has a higher ceiling. A committed state may reopen if shop evidence, missing pieces or board weakness materially reduce continuation EV.

## Economy and random value

Gold is both purchasing power and option value. Random-item mechanics are evaluated as conversions of agency or liquidity into stochastic value. Expected generated value is discounted by expected usability, lost agency, lost liquidity, space pressure and stranded-item risk. Random value is generally easier to exploit early, when the acceptance range is broad, than late, when synergy requirements are tighter.

## Combining

Combining is a commitment decision, not an automatic upgrade. Available actions are:

`COMBINE NOW / HOLD COMPONENTS / ABANDON RECIPE`

Evaluation includes immediate power, synergy, space effect, scaling, destroyed option value, component opportunity cost and transition risk. Owning part of a recipe does not justify destructive rerolling. Branching recipes retain option value. The operator should be warned when placement could cause an unwanted combination.

## Placement and human execution

The experiment deliberately accepts imperfect human execution. The model does heavy strategic work and medium placement work rather than turning the operator into a coordinate-following robot.

Placement instructions use three tolerances where useful: **Critical / Target / Minimum acceptable**. Three of four stars may be explicitly acceptable if forcing the fourth breaks a more valuable interaction or imposes excessive rearrangement cost.

An extra screenshot is requested only when uncertainty about the resulting board could materially damage expected outcome. Obvious execution errors are tagged separately from strategic decision quality.

A theoretical maximum-performance mode could later reconstruct the backpack as a coordinate lattice and use a dedicated spatial solver for rotations, star patterns and weighted interactions. This is deliberately parked until positioning is demonstrated to be the binding performance constraint.

## Survival and proven boards

At ten wins, the model estimates whether to bank or continue using demonstrated robustness, remaining hearts, board coherence, scaling potential, reachable upgrades and their realization probabilities. A board that has already survived ten rounds carries evidence of viability. **Inaction is therefore a legitimate decision, not a failure to optimize.**

## Run protocol

Each run is initialized with the game patch, strategy-prior version, decision-model version, reasoning-model version, character, starting rank, and selection mode (`random` or `targeted`). Random character selection is allowed and is the default natural-play mode. Targeted character runs may later rebalance sample sizes or test a specific hypothesis.

A normal decision screenshot should expose the full backpack, shop, gold, hearts and wins, subclass state, and relevant tooltip text when an item's effect cannot be reliably inferred from the image alone.

The default live interface is intentionally concise:

`Buy / Sell / Roll / Combine / Hold / Placement priority / Why`

Detailed reasoning is logged rather than repeatedly dumped into the operator interface. Intermediate screenshots are requested only when they have material expected value.

## Measurement

Decision quality is evaluated **ex ante** separately from stochastic combat outcomes. Major decisions may record confidence so calibration can later be tested. Opponent and loss context should be retained where observable so systematic matchup weaknesses can be distinguished from generic underperformance.

Results should be segmented by `patch x H-version x DM-version x reasoning-model version x character x selection mode`. Small-N swings should not trigger policy revisions.

## Policy-change rule

Do not revise the decision model because of isolated outcomes. A material change requires repeated evidence, a clear theoretical justification, or a strong externally verified correction. Every material logic change receives a new DM version rather than silently rewriting the prior model.

Likewise, strategy-prior changes receive a new H version. This allows later analysis of whether a change actually improved effectiveness rather than relying on retrospective narrative.

## Community feedback

Community feedback should be classified where possible as mechanic correction, strategy disagreement, decision-theory critique, reproducibility issue, or observed failure. Mechanic corrections supported by authoritative evidence can warrant immediate correction. Strategy disagreements remain hypotheses until evidence justifies changing the model.

## Reproducibility

The public package includes methodology, versioned decision logic, strategy priors, run schema, patch source notes and Custom GPT setup files so others can reproduce or fork the workflow in their own ChatGPT environment. This is **not** represented as a standalone trained Backpack Battles model.

## Privacy boundary

This repository is public and **Backpack Battles only**. No unrelated project information, private CRM data, email, messages, contacts, credentials, connected-source data or personal material belongs here. See [`PUBLICATION_BOUNDARY.md`](PUBLICATION_BOUNDARY.md).

## Current patch: 1.1.8

The raw 1.1.8 patch notes supplied to the experiment are preserved separately as source evidence. H2 carries forward H1 archetypes and applies explicit patch deltas where 1.1.8 materially affects their core, support or bridge pieces. Unaffected strategies are not artificially rewritten merely to create a new meta narrative.

## Changelog

### DM-0.1
Initial adaptive decision policy. Added phase-dependent play, tempo-first early game, strategy beliefs and commitment states, pivot economics, conservative late-board intervention, Survival logic, combining, stochastic-value treatment, and best-effort human placement.

### DM-0.2
Added explicit freestyling candidate generation and the intermediate convergence model. Preserved belief confidence separately from economic commitment and allowed emergent strategies to compete with precomputed priors.

### H1
Initial set of three strategy priors per playable character.

### H2
Recalculated for patch 1.1.8. Twenty-one active strategy priors across seven characters. Patch-sensitive deltas applied without claiming simulated or empirical optimality.

### Pre-live protocol update
Standardized screenshot requirements, run initialization metadata, Random-versus-targeted selection tagging, and concise live command format.

## Next step

**Run #001.** Stop designing until live evidence demonstrates a specific missing capability or bottleneck.
