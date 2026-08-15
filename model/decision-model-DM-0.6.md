# Decision Model DM-0.6

**Status:** active  
**Base model:** DM-0.5  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged  
**Default live-play interface:** BPB Autonomous Mode

## Why DM-0.6 exists

DM-0.5 reduced operator friction but still produced repeated gameplay errors: local item evaluation, missed combinations, purchases that became idle storage capital, spatially impossible deployment assumptions, and unsupported item identification from screenshots. DM-0.6 changes the decision loop so the engine must prove it understands the current state before committing to a shop action.

This is a reasoning/process refactor, not a new software architecture. No OCR pipeline, full game-state database, or autonomous game client is introduced.

## 1. Full-state gate before action

Every gameplay screenshot triggers a mandatory state gate before any recommendation:

```text
OBSERVE
-> identify only high-confidence visible objects
-> reconcile board + storage + gold + round + health + wins/tries
-> mark uncertain identities UNKNOWN
-> inventory deployed vs stored capital
-> check recipes/combinations
-> check board capacity and eviction candidates
-> identify binding combat constraint
-> evaluate the whole shop phase
-> issue action batch
```

If any decision-material object is not identified with sufficient confidence, the engine must not name it or reason from an invented identity.

Unknown decision-material mechanics follow the runtime knowledge policy: local cache -> bounded external validation -> minimum Player tooltip/screenshot request if unresolved.

## 2. Identity confidence rule

Item identity is evidence, not a visual guess.

Use three states:

- **CONFIRMED:** tooltip, persistent cache match, or visually distinctive item with high confidence.
- **PROBABLE:** likely identification but not safe enough to drive a material purchase/sell decision without corroboration.
- **UNKNOWN:** insufficient confidence.

A PROBABLE or UNKNOWN item may be ignored if it is clearly irrelevant to the current decision. If it could change the action, resolve it before acting.

Never fabricate a name such as `bag`, `weapon`, or specific item merely to complete the action loop.

## 3. Shop-phase transaction planning

Do not evaluate the shop as independent item decisions.

For each screenshot, compare reachable shop-phase transactions across:

- current deployed board;
- storage;
- current gold;
- liquidation value;
- current shop;
- recipe graph;
- pending combinations;
- displacement requirements;
- reroll budget;
- minimum liquidity reserve;
- current survival pressure;
- current binding combat constraint.

The output should represent the best coherent transaction sequence available before the next information boundary.

## 4. Deployed-capital discipline

Track separately:

- displayed gold;
- stored liquidation value;
- stored strategic option value;
- **deployed combat capital**;
- stranded/idle purchased capital.

A purchase is not credited as combat strength merely because it was bought.

An item in storage must justify itself as one of:

- near-term combination component;
- high-value option with realistic deployment path;
- deliberate economic asset;
- temporary unavoidable overflow.

Otherwise it is idle capital and a warning signal.

When storage is materially congested, the default purchase threshold rises sharply.

## 5. Space and eviction gate

Before recommending any purchase on a constrained board, answer internally:

1. Does it fit without strategic damage?
2. If not, what exactly is displaced or stored?
3. Is the new deployed state better than the displaced state?
4. Does the transaction worsen idle-capital congestion?

If the engine cannot identify a viable eviction or rearrangement, it must not recommend the purchase merely because the item is individually good.

Routine geometry may still be delegated to Player as best-fit packing, but strategic displacement is engine-owned.

## 6. Combination-first audit

Before purchase, reroll, sell, or combat, audit recipes/combinations involving:

- board items;
- storage items;
- current shop items;
- staged components.

Missed combinatorial action space is a first-class regression category.

The mechanics cache should be expanded incrementally with verified recipes and combinations as encountered. Do not attempt a complete catalogue unless repeated evidence shows the cache itself is the bottleneck.

## 7. Action queue semantics

Commands are cumulative until explicitly cancelled or superseded.

`BUY A -> BUY B -> REROLL` means A and B remain intended.

The engine must not issue a batch containing mutually incompatible deployments unless it also specifies the displacement/priority logic.

`Buy` implies deploy when legal and strategically intended. Storage is not an automatic destination for purchases.

## 8. Autonomous placement authority

Player may perform best-guess packing for routine spatial execution.

Exact placement is required from the engine when any of the following materially affects value:

- adjacency;
- star coverage;
- item activation geometry;
- recipe staging;
- socket placement;
- eviction choice;
- mutually exclusive deployment.

Routine packing does not create an observation boundary.

## 9. Voluntary Player guidance

Autonomous Mode means **zero required Player strategy input**, not zero permitted input.

Player may flag:

- an apparent blunder;
- a missed recipe;
- an item identity;
- a mechanic;
- a strategic concern.

The engine must independently recompute. Player guidance is evidence, not automatic authority, unless explicitly stated as an override.

Repeated Player correction remains a model failure signal.

## 10. Battle-result severity and reset

Classify outcomes at minimum as:

- decisive win;
- close win;
- close loss;
- decisive loss.

A decisive loss triggers a **structural reset** before the next purchase:

```text
reassess board strength
-> identify likely binding failure
-> review idle capital
-> review weapon/stamina balance
-> review defense/sustain
-> review combinations and displacement
-> then evaluate shop
```

Do not respond to a decisive loss with another incremental purchase unless the full-state audit supports it.

## 11. Reroll discipline

Reroll is not the automatic fallback when the shop is unclear.

Before rerolling, compare:

- expected information/value of the next shop;
- current gold and liquidity;
- unresolved current-shop identities;
- whether current shop contains a consolidation/space upgrade;
- survival pressure;
- ability to purchase a meaningful hit after paying reroll cost.

If a current-shop item cannot be identified and could plausibly dominate reroll, resolve it rather than skipping it by assumption.

## 12. Observation economy

Retain DM-0.5 observation economy:

`SCREENSHOT -> ENGINE -> ACTION BATCH -> INFORMATION BOUNDARY -> SCREENSHOT`

Batch deterministic actions. Do not request screenshots after every purchase or placement.

An information boundary exists when new unseen information could materially change the next action, including rerolls, random outcomes, combat, unidentified-item resolution, or strategically material geometry uncertainty.

## 13. Bounded external validation

Use the persistent mechanics cache first.

For a decision-material cache miss:

- external lookup target: roughly 10 seconds;
- operating ceiling: roughly 15 seconds;
- stop rather than search indefinitely;
- if unresolved, request the minimum tooltip/screenshot;
- persist verified mechanics with provenance.

This is an operating budget, not a guaranteed wall-clock timeout.

## 14. Privacy-by-design firewall

Public BPB terminology uses **Player** for the human participant.

Do not publish unrelated project/workstream names, professional titles/callsigns, real-world identity, private operational terminology, customer/confidential data, or unrelated personal context.

Pre-publication scrub covers body text, filenames, screenshots, commit text, and metadata.

## 15. Architecture constraint

Do not overbuild the experiment.

Current minimum sufficient architecture:

- screenshot input;
- persistent patch-frozen mechanics/recipe cache;
- versioned decision model;
- concise run logs;
- explicit state/economy reconciliation;
- bounded external validation.

Reconsider heavier data ingestion or a purpose-built state engine only if repeated DM-0.6 runs show that state reconstruction or combinatorial bookkeeping remains the dominant performance bottleneck after this refactor.

## 16. Run viability and surrender policy

The experiment-level objective dominates the in-game objective. **Player time and patience are the ultimate scarce resources.** The engine must not assume that every damaged run deserves to be played until formal game-over.

At materially impaired states, evaluate three actions:

`CONTINUE / ATTEMPT RECOVERY / SURRENDER`

Use a qualitative value comparison:

`ContinueValue = recovery probability × recovered-run value + remaining information value - expected Player cost`

`RestartValue = expected fresh-run value - restart cost - expected Player cost`

Do not assign false numerical precision until empirical calibration exists.

Maintain a run-viability state:

`HEALTHY -> IMPAIRED -> CRITICAL -> TERMINAL`

- **HEALTHY:** normal play. No viability intervention.
- **IMPAIRED:** meaningful tempo/capital/board deficit exists, but ordinary recovery remains plausible.
- **CRITICAL:** accumulated disadvantage may be effectively irreversible. Before further routine spending, perform an explicit recovery-feasibility audit.
- **TERMINAL:** additional expected information and plausible recovery value no longer clear Player-time cost relative to restarting. Recommend `SURRENDER`.

Low win probability alone is not sufficient for surrender. A losing run may remain experimentally valuable.

### Recovery-feasibility audit

At CRITICAL, identify an actual reachable recovery route rather than relying on generic shop luck. Consider:

- deployed board strength relative to round;
- idle/stranded capital and whether it can realistically be converted;
- current gold and liquidation options;
- space constraints and consolidation opportunities;
- recipe/combinational rescue paths;
- remaining lives/tries and likely number of decision cycles available;
- magnitude and reversibility of prior tempo loss;
- recent combat severity;
- expected Player interactions required to test the recovery;
- whether an unresolved experimental hypothesis makes continued play informative.

If no credible recovery path can be articulated, prefer surrender over consuming Player time to confirm an already high-confidence failure.

### Failure attribution

A surrender caused materially by accumulated engine decisions is recorded as an **engine failure**, not hidden as an abandoned/incomplete run.

Track at minimum:

`run_surrendered_due_to_model_induced_unrecoverable_state = true/false`

The purpose is to preserve falsifiability and prevent surrender from artificially improving reported run outcomes.

Surrender is therefore a legitimate autonomous recommendation and should appear in the live interface as simply:

`SURRENDER -> restart run.`

## Live output contract

Normal response is concise and executable.

Examples:

`BUY X -> SELL Y -> deploy X in Y's space -> REROLL once.`

`NO BUY -> REROLL once.`

`BLOCKED: lower-left item could materially change the decision and identity is uncertain. Hover it.`

`SURRENDER -> restart run.`

The engine should never issue a confident item-specific action when the item identity itself is uncertain.

## Regression cases carried into DM-0.6

- missed broom/pan combination opportunity;
- repeated accumulation of substantial storage value while deployed board remained weak;
- recommending purchases without a viable deployment/eviction plan;
- treating shop items locally instead of as a transaction set;
- unsupported skill inference from names;
- hallucinated item identity (`bag`) from screenshot;
- excessive Player correction required to keep the run coherent;
- continuing a likely unrecoverable run when restart has higher information-adjusted value per unit of Player time.

These are mandatory checks for future BPB debriefs.
