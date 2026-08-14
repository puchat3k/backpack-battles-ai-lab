# Decision Model DM-0.2

**Status:** active  
**Patch context:** 1.1.8  
**Strategy prior context:** H2

DM-0.2 is the policy used by the AI during live play. It is not a fixed build script. The objective is to maximize practical ranked outcome through a screenshot-driven human interface.

## 1. Phase-dependent objective

### Early game
Priority order is approximately:

1. immediate tempo
2. board value per gold
3. gold preservation
4. optionality

Default rule: buy useful power before paying to search for ideal power. A reroll must beat the expected value of the best useful shop purchase plus the value of preserving gold.

### Mid game
Priority shifts toward:

1. strategy convergence
2. synergy formation
3. tempo preservation
4. commitment management

The phase transition is state-dependent, not strictly round-dependent.

### Late game
Priority shifts toward:

1. system strength
2. matchup robustness
3. scaling
4. intervention risk
5. replacement efficiency

A functioning board always has a status-quo candidate.

## 2. Strategic belief state

Maintain comparative belief weights over:

- known H2 strategy priors
- open play
- emergent/unmodeled lines

The weights are heuristic comparative beliefs, not calibrated probabilities.

Strategic commitment states:

`OPEN -> LEANING -> COMMITTED -> ECONOMICALLY LOCKED`

**OPEN:** option value dominates; no line sufficiently justifies commitment.  
**LEANING:** one or more destinations are favored, but preserving alternatives still has material value.  
**COMMITTED:** exploiting the leading line has greater expected value than preserving alternatives.  
**ECONOMICALLY LOCKED:** switching is negative expected value after stranded assets, rerolls, recipes, board disruption and temporary weakness, even if another line has a higher theoretical ceiling.

Belief confidence and economic commitment are separate variables.

## 3. Freestyling heuristic

When no strategy is sufficiently dominant, generate candidate actions under five modes:

- exploit current board
- preserve optionality
- pivot
- exploratory
- tempo

Compare candidate actions using:

- immediate power
- future option value
- synergy density
- assembly friction
- economy efficiency
- space efficiency
- timing coherence
- counter coverage
- transition cost
- ceiling
- human execution complexity

An emergent line is allowed to outperform H2 priors. H2 is a prior over plausible destinations, not a forced route.

## 4. Commitment and pivot rules

Commit when:

`EV(exploit leading line) > EV(preserve remaining options)`

Pivot when:

`EV(new line after switching costs) > EV(continue current line)`

There is no universal commitment percentage. Health, gold, phase, board strength, missing components, switching cost and alternative quality affect the threshold.

A committed state may reopen when missing components, repeated adverse shops, board weakness or new high-value evidence materially reduce continuation EV.

## 5. Random-value and agency tradeoff

For mechanics that exchange gold/control for random item value:

`EV(convert) = generated value x usability + option value + synergy upside - lost agency - lost liquidity - space pressure - stranded-item risk`

Random value is generally easier to exploit early because more outcomes are usable. As commitment rises, random nominal value is discounted more heavily for incompatibility.

## 6. Combining

Combining is a commitment decision, not an automatic upgrade.

Available actions:

`COMBINE NOW / HOLD COMPONENTS / ABANDON RECIPE`

Evaluate:

- immediate power gain
- synergy gain
- space effect
- future scaling
- destroyed option value
- component opportunity cost
- transition risk

Owning part of a recipe does not justify excessive rolling. Sunk cost must not override tempo/economy. Branching recipes retain option value while alternatives remain plausible.

Warn the operator when placement could cause an unwanted combination.

## 7. Late-game replacement

For a proven/full board compare:

`HOLD / LOCAL OPTIMIZE / RESTRUCTURE`

Do not replace a functioning component merely because another item is nominally stronger. The expected improvement must exceed sale/rebuy cost, lost synergy, recipe risk, positioning disruption, timing disruption and probability the replacement plan fails to complete.

The burden of proof for intervention rises with demonstrated board robustness.

## 8. Survival decision

At 10 wins compare banking against continuing using:

- demonstrated robustness
- remaining hearts
- current board coherence
- recent fight trajectory
- current scaling
- reachable upgrades
- probability and magnitude of those upgrades
- fragility to likely late-game opponents

A board can be classified approximately as dominant, strong + scalable, incomplete high-upside, capped/fragile or incoherent.

## 9. Human execution and positioning

The system assumes imperfect human execution. Strategic optimization is heavy; placement optimization is medium-weight by default.

Placement guidance should state:

- **Critical:** relationships that must be preserved
- **Target:** preferred coverage
- **Minimum acceptable:** sufficient outcome when further optimization has low marginal value

Example: `Target 4/4 stars; 3/4 is fine if the fourth breaks the stronger interaction.`

Request an intermediate screenshot only when uncertainty about placement has material expected-value impact.

Tag obvious execution errors separately from strategy quality.

## 10. Live output format

Default operator-facing response:

`Buy / Sell / Roll / Combine / Hold / Placement priority / Why`

Keep this concise during play. Detailed reasoning belongs in the experiment log.

## 11. Decision-quality measurement

Evaluate decisions ex ante separately from combat outcomes. A good decision can lose and a bad decision can win.

Major decisions may log:

- chosen action
- alternatives
- confidence
- commitment state
- strategy beliefs
- immediate power estimate
- option value
- pivot/transition cost
- exploration value
- later outcome

## 12. Policy revision rule

Do not update DM because of an isolated loss or win. A material policy change requires at least one of:

- repeated evidence
- a clear theoretical justification
- a strong externally verified mechanic correction

Every material change receives a new DM version. Historical DM versions remain immutable for performance comparison.