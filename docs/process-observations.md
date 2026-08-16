# Process Observations

## Post-purchase state recomputation

**Observed during Run #001, Round 7.**

A discounted 4-slot bag was purchased. The initial decision process treated the purchase as an isolated action and moved directly to the next shop decision without explicitly recomputing the strategic state created by the purchase.

This was a structural failure. A purchase can change more than gold and immediate board power; it can alter the value of other current-shop items and future build paths by changing:

- available board space;
- space scarcity/shadow price;
- affordable component density;
- recipe feasibility;
- transition cost;
- liquidity;
- stamina burden;
- strategic commitment and option value.

### Required process correction

After every material action, especially **Buy / Sell / Combine / Add Bag / Deploy / Store**, recompute the reachable-state evaluation before issuing the next action when the action changes any important constraint.

Use the loop:

```text
observe state
-> choose action
-> apply action
-> update constraints
-> recompute candidate values
-> choose next action
```

Do not assume a multi-action recommendation remains optimal after the first action executes.

A bag purchase is a particularly clear example: additional capacity reduces the marginal cost of bulky components and can immediately increase the expected value of build-investment lines that were previously rejected because of space pressure.

This observation is logged for incorporation into the next formal decision-model revision rather than silently rewriting prior model versions.

---

## Prior-first decision loop and human calibration

**Introduced after Run #004 Reaper Stone Badge / Stoned debrief.**

### Observation

The live engine often demonstrated reasonable core-mechanics and strategic knowledge, but shop-state salience was weighted too heavily. The practical reasoning order frequently became:

```text
visible shop item
-> local item valuation
-> consult strategy
```

This allowed locally attractive sale, cheap, or novel items to override strategic conclusions established only moments earlier.

The revised hypothesis is that the ordering itself should be inverted:

```text
validated priors
-> candidate scalable build paths
-> current board feasibility
-> explicit build needs/dependencies
-> interrogate shop for those needs
-> fallback alternatives if the path cannot be advanced
```

The shop becomes an action/evidence source rather than the origin of strategic intent.

### Original-thesis connection

This is closer to the project's original experimental thesis: persistent accumulated strategic priors should enable repeatably better decisions than isolated local optimization.

The relevant question is not merely whether the model can identify a strong item in a screenshot. It is whether persistent validated knowledge can generate a coherent scaling plan and then correctly interpret successive shops through that plan.

This creates a stronger test of persistent AI decision support than shop-by-shop heuristic play.

### New limitation exposed

Prior-first reasoning increases dependence on accurate knowledge of:

- build paths;
- item combinations and recipes;
- enabling infrastructure;
- subclass dependencies;
- stamina/space requirements;
- bridge items and transitions.

A model may have strong high-level strategy while having incomplete or falsely confident dependency knowledge. Forcing a prior under those conditions could make performance worse rather than better.

### Human-in-the-loop calibration experiment

Rather than prebuilding a complete BPB dependency database, the engine will expose the candidate prior when a run starts or the preferred path materially changes.

Example interface:

```text
Build prior: Bloodthorne Ranger
Why: high repeatable scaling potential; compatible with current opening.
Key items/enablers I currently believe matter:
- Hungry Blade
- Thorn Whip
- relevant recipe/support infrastructure
Uncertain/missing: [material gaps]
```

The Player then corrects missing, wrongly weighted, or misunderstood dependencies.

The engine remains responsible for selecting the candidate strategy. The Player functions as a low-cost knowledge-quality check rather than the strategy generator.

Once calibrated, the dependency assumptions become the working build map and should be enforced until new evidence materially changes the path.

### Why this may matter beyond BPB

This experiment creates a potentially generalizable pattern for AI decision systems operating with incomplete domain knowledge:

1. use persistent validated priors to generate intent before reacting to local observations;
2. expose the model's dependency assumptions rather than hiding them behind a recommendation;
3. use targeted human correction at high-leverage uncertainty points;
4. preserve the corrected map across subsequent decisions;
5. allow new evidence to override the prior, but require an explicit evidentiary reason.

The interesting content/research question is whether this ordering produces more coherent and repeatable performance than reactive local optimization, while requiring much less human effort than manually specifying every action.

This observation is intentionally retained as process history because it may be useful for future GitHub documentation, experiment writeups, or longer-form analysis.
