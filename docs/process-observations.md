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