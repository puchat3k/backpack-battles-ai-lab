# Decision Model DM-0.8

**Status:** active  
**Base model:** DM-0.7  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged unless explicitly stated  
**Default live-play interface:** BPB Autonomous Mode

## Why DM-0.8 exists

DM-0.7's live output contract allowed concise executable responses but did not make explicit a previously established interaction requirement: live shop recommendations should normally provide a short decision sequence, not a single atomic action. During a Round 6 Reaper shop, the model repeatedly issued one-step commands such as buying one item and then waiting for another screenshot, despite enough visible state to plan several actions ahead. This unnecessarily increased Player interaction cost and correction burden.

DM-0.8 restores multi-action planning as the default live-play contract.

## 1. Decision-list default

When the visible state supports more than one reliable action, return a concise ordered list of decisions rather than one atomic command.

Preferred form:

1. BUY / SELL / MOVE / RESERVE / REROLL action.
2. Next action.
3. Next action or explicit stop condition.

The list should cover the current decision horizon, normally the remainder of the shop state that can be planned without seeing new information.

## 2. Stop only at genuine information boundaries

Do not request another screenshot merely because one action has been executed.

Stop the sequence when execution creates materially new hidden information or invalidates downstream planning, for example:

- a reroll changes the shop;
- buying an unidentified/random skill reveals a new option;
- a combination changes available space or resources in a way that cannot be reliably inferred;
- a tooltip or item identity is uncertain enough that proceeding would require guessing;
- Player intervention changes the intended line.

If the next action is already determined from current information, include it now.

## 3. Conditional branches are allowed

When an action will reveal new information, give the action plus the immediate decision rule when useful.

Example:

1. SELL low-value storage item.
2. BUY bag.
3. REROLL.
4. STOP and show shop.

Do not fabricate post-reroll purchases before the reroll is visible.

## 4. Transaction planning remains global

Before emitting the list, reconcile the full transaction sequence against:

- current gold;
- recoverable gold in storage;
- board and storage capacity;
- reserved/locked shop items;
- combinations that free space;
- immediate combat value;
- economy and future option value;
- phase-specific tempo priors.

A decision list is not permission to chain locally attractive actions. The sequence must be jointly feasible and strategically coherent.

## 5. Player interaction cost

Player time and patience are scarce resources. Avoid forcing the Player to execute obvious one-step instructions and return screenshots when several safe decisions can be batched.

Atomic commands are appropriate only when the decision horizon is genuinely one action long.

## 6. Regression case. Round 6 atomic-command drift

Observed failure:

- Reaper Round 6.
- Model recommended Leather Armor as a single command.
- On the unchanged shop state it then separately recommended the reserved bag.
- After another screenshot it separately recommended a shield.
- Player explicitly reminded the model that recommendations should be lists of decisions rather than atomic actions.

Failure classification:

**live-interface regression. Planning horizon was artificially truncated despite sufficient visible information.**

Mandatory lesson:

> Plan to the next genuine information boundary, not merely to the next executable click.

## Regression cases carried into DM-0.8

All DM-0.7 regression cases remain active, plus:

- issuing atomic commands when multiple reliable actions can be planned;
- requesting unnecessary screenshots between deterministic actions;
- failing to include storage liquidation, reservation, combination, purchase, and reroll actions in one coherent transaction sequence when visible state permits;
- planning beyond a genuine information boundary by hallucinating unseen reroll or random outcomes.

## Live output contract

Normal live-play responses are concise, ordered, and executable.

Default:

1. action
2. action
3. action
4. STOP / REROLL AND SHOW SHOP when new information is required

Explanations are omitted unless requested, uncertainty materially affects execution, or a failure is being diagnosed.
