# Decision Model DM-0.11

**Status:** active  
**Base model:** DM-0.10  
**Patch context:** 1.1.8  
**Default live-play interface:** BPB Autonomous Mode

## Why DM-0.11 exists

DM-0.10 fixed epistemic fabrication but the next live Berserker session exposed two execution failures that can still invalidate otherwise sound strategy:

1. **state drift:** recommending an action already completed, or reasoning from an earlier screenshot after the Player has bought, sold, repositioned, combined, or corrected an item;
2. **outcome misattribution:** treating an ugly combat result as evidence that the build is globally weak when the loss may primarily reflect a specific counter-matchup or combat dynamic not recoverable from the terminal screenshot.

DM-0.11 adds a transactional state ledger and matchup-attribution gate ahead of strategic updating.

## 1. Governing order

`epistemic gate → reconcile latest state ledger → validated priors → build/path evaluation → shop action → contradiction check`

After combat:

`observed result → Player-observed combat dynamics if supplied → matchup attribution → build-level update`

## 2. Transactional state ledger

Every explicit Player action updates current state immediately. Later recommendations must use the post-action state, not the last screenshot in isolation.

State-changing events include:

- purchase;
- sale;
- reroll;
- item moved between board and storage;
- recipe/combine started or completed;
- bag/space change;
- skill selection;
- Player correction of item identity, gold, recipe state, or board state.

Before issuing the next action, reconcile:

- current gold;
- board items;
- storage items;
- active/pending combines;
- free/committed bag space;
- purchased items no longer present in shop;
- current round/wins/lives when visible;
- any Player-reported state changes since the screenshot.

Do not recommend buying, selling, combining, or moving an item unless the ledger says that action is currently possible and not already completed.

## 3. Event precedence

When screenshots and Player action reports differ because the screenshot predates the action, the later event wins.

Conceptually:

`latest verified event > earlier screenshot state`

A Player statement such as "I bought the Whetstone after selling Spear, 4g left" updates the ledger. The engine must not subsequently instruct "buy Whetstone" merely because it is reasoning from the preceding shop image.

If a later screenshot conflicts with the ledger and chronology is unclear, stop and reconcile rather than silently choosing one state.

## 4. Recipe and combine representation

Track recipe components as separate physical items until the game actually consumes/transforms them.

Distinguish:

- owned and free;
- owned and currently committed to a pending combine;
- generated/stored;
- transformed result after combat.

Do not treat a pending recipe as already completed. Do not treat an item committed to one recipe as simultaneously available to another.

When a recipe mechanic is Player-supplied, store the exact supplied transformation and consumption semantics. Example from this session: Hammer + Gloves upgrades the Gloves to Dragon Gloves while Hammer persists. Do not infer consumption rules from generic crafting assumptions.

## 5. Recommendation idempotence

Before each recommendation ask:

> Has this exact action already happened according to the latest verified state?

If yes, do not recommend it again. Continue from its consequence.

This is a mandatory regression check for live screenshot play.

## 6. Player challenge remains non-directive

DM-0.10 `/challenge [decision/item]` remains active.

A Player challenge, suggestion, or alternative is evidence that a decision deserves recomputation, not evidence that the Player's proposed answer is correct.

When the Player says, for example, "we can sell it later," compare hold-versus-sell independently. Do not convert the observation directly into a recommendation.

## 7. Combat outcome attribution

A terminal combat screenshot is incomplete evidence about the path taken through combat.

Do not infer continuous stamina availability, proc frequency, shield interactions, or timing solely from final bars.

Separate:

- **result:** final health, win/loss, visible terminal buffs/debuffs;
- **combat dynamics:** events observed during the fight, including Player-reported repeated stamina starvation;
- **matchup effect:** interaction between architectures, counters, or opponent composition;
- **build weakness:** evidence that should generalize beyond the encountered opponent.

A severe loss is not automatically evidence of a severe general build deficit.

## 8. Matchup weakness versus build weakness

Before materially changing strategy after a fight, ask:

> Is this failure expected to generalize across opponents, or is there a credible specific counter interaction?

Session regression case:

- our board used a dual-weapon architecture;
- opponent presented two shields;
- Player observed repeated stamina depletion during combat;
- terminal screenshot alone misleadingly showed remaining stamina at death;
- correct abstraction: dual-weapon builds can suffer disproportionately into shield/double-shield opponents because attack attempts consume stamina while shields reduce conversion of those attacks into damage.

Therefore the 0-40 loss is primarily matchup evidence unless repeated results against broader opponent types establish a general tempo/stamina problem.

Do not pivot the build solely because a natural counter produced an ugly scoreline.

## 9. Human-observed hidden dynamics

The Player may observe temporal information that a static screenshot cannot encode.

Treat explicit Player reports of combat events as VERIFIED session evidence unless contradicted by stronger evidence. Examples:

- repeated stamina bottoming;
- an item failing to trigger;
- a shield repeatedly blocking a weapon line;
- timing/order effects invisible in the final frame.

Use these reports to repair attribution, but distinguish them from general strategic claims that still require validation before becoming reusable priors.

## 10. Regression cases

Mandatory regressions now include DM-0.10 cases plus:

- recommending purchase of an item already purchased;
- reasoning from pre-action gold after a sale/purchase/reroll;
- double-allocating a component committed to a pending recipe;
- treating a pending combine as completed;
- agreeing with a Player alternative without independently comparing it;
- inferring continuous combat behavior from terminal bars;
- converting a counter-matchup loss directly into global build weakness;
- ignoring Player-observed temporal combat dynamics unavailable in the screenshot.

## 11. Live output contract

Normal output remains concise.

Before output, internally reconcile the latest ledger. If state is contradictory, ask only for the minimum correction needed.

When a fight ends, do not prescribe a strategic pivot until matchup attribution is sufficiently resolved.
