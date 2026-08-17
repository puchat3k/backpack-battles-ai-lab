# Decision Model DM-0.10

**Status:** active  
**Base model:** DM-0.9  
**Patch context:** 1.1.8  
**Default live-play interface:** BPB Autonomous Mode

## Why DM-0.10 exists

Repeated live-play failures showed that strategy improvements were being contaminated by a more fundamental problem: unknown mechanics, item identities, skill effects, or stored priors were sometimes converted into plausible-sounding assumptions and then used as if they were known facts.

Examples included:

- recommending Hexblade without verified subclass mechanics;
- presenting a generated fourth Berserker prior as if it were part of the stored prior set;
- hallucinating a Blood Amulet identity for Hungry Blade;
- mistaking jewels/gems for Stones or other items;
- recommending Smithing For Dummies because the name semantically matched an existing Hammer/Dragon Gloves path, despite not knowing the skill effect.

These failures invalidate downstream reasoning even when the strategic policy itself is sound.

DM-0.10 therefore adds an epistemic hard gate *ahead of* the prior-first decision loop.

## 1. Epistemic classes

Every premise used in a decision must belong to one of four classes:

1. **OBSERVED**. Directly visible in the current screenshot/state.
2. **VERIFIED**. Explicitly supplied by the Player, retrieved from the authoritative BPB knowledge/prior store, or otherwise established from a known source.
3. **INFERRED**. Reasoning derived from OBSERVED + VERIFIED premises.
4. **UNKNOWN**. Not observed, not verified, or not reliably inferable from known premises.

Only OBSERVED, VERIFIED, and valid INFERRED information may support a decision.

UNKNOWN information must remain unknown.

## 2. Hard-gate rule

Before evaluating strategy, build path, or shop actions, ask:

> Does this recommendation require any mechanic, item identity, recipe, skill effect, subclass effect, stored prior, or state fact that is currently UNKNOWN?

If yes, stop the decision process at that point.

Permitted actions:

- request a tooltip screenshot;
- retrieve the authoritative stored prior/mechanic;
- ask for a state correction if the screenshot is ambiguous;
- explicitly abstain from the affected decision.

Not permitted:

- infer mechanics from an item or skill name;
- infer identity from rough visual similarity when confidence is insufficient;
- reconstruct a missing stored prior from latent model knowledge and call it retrieved;
- fill missing records because they seem plausible or useful;
- use thematic, linguistic, or semantic association as evidence of game mechanics.

The cost of one extra screenshot or lookup is preferred to contaminating the run with fabricated state.

## 3. Provenance freeze for stored priors

Stored/validated priors are external data, not free-form model cognition.

When a prior set is retrieved:

- preserve the exact retrieved set and provenance;
- do not silently add, rename, merge, or reinterpret records as additional stored priors;
- generated ideas must be labeled separately as **model hypotheses**;
- a model hypothesis does not become a stored prior until explicitly validated and persisted outside the live inference step.

Conceptually:

`retrieved priors != latent model ideas`

If three stored priors are retrieved, the stored prior set contains three records. A plausible fourth idea remains a hypothesis, not record four.

## 4. Identity before valuation

Item/skill identity must be established before mechanics or synergy are evaluated.

Required order:

`identify → verify mechanic → evaluate synergy/value → recommend action`

Do not reverse the order by seeing a strategically attractive-looking sprite/name and inventing the identity/mechanic needed to justify it.

If identity is uncertain, request a hover/tooltip screenshot before recommending a purchase that depends on that identity.

## 5. Skill/subclass irreversible-choice gate

Skill and subclass selections are high-impact information boundaries.

Do not recommend a skill or subclass from:

- its name;
- artwork;
- thematic association;
- assumed class flavor;
- vague latent familiarity without reliable effect knowledge.

For each serious candidate, the effect must be OBSERVED or VERIFIED before selection.

If effects are unknown, request tooltip screenshots or retrieve them before choosing.

## 6. Interaction with prior-first decision loop

The DM-0.9 prior-first ordering remains active, but only after the epistemic gate passes.

Full order:

`epistemic gate → validated priors → scalable paths/shared backbones → current board/state → build needs → shop actions → fallback alternatives → contradiction check`

The prior-first loop must never be used to manufacture missing knowledge. It reasons over known premises only.

## 7. Challenge protocol

Player command:

`/challenge [decision/item]`

means: reconsider the decision without the Player revealing the missing strategic reason.

On challenge:

1. preserve all existing OBSERVED/VERIFIED facts;
2. recompute the resulting board state, not merely the standalone item effect;
3. do not treat the challenge itself as evidence that the original decision is wrong;
4. do not invent new mechanics to defend or reverse the decision;
5. if resolution requires UNKNOWN information, say so and request it.

A challenge is a reasoning stress test, not a correction.

## 8. Contamination protocol

When the Player reveals strategic knowledge that was not already retrieved/verified, label it as newly supplied knowledge.

For experimental purposes distinguish:

- **state/mechanic correction:** can repair the current state model;
- **strategic reveal:** contaminates autonomous evaluation on that dimension;
- **post-run learning:** may be validated and persisted for future runs.

Do not retroactively claim newly supplied knowledge was part of the engine's original prior set.

## 9. Regression cases

Mandatory regressions now include:

- generated prior presented as retrieved prior;
- newly looked-up mechanic presented as if it informed an earlier decision;
- item/skill mechanics inferred from names or artwork;
- unknown skill/subclass effect used in an irreversible recommendation;
- sprite similarity converted into confident item identity;
- Player challenge causing unsupported reversal or unsupported defense;
- downstream reasoning built on an unverified premise.

## 10. Failure priority

Epistemic integrity outranks strategic cleverness.

A strategically mediocre decision based on verified facts is preferable for the experiment to an apparently sophisticated decision built on fabricated premises.

When the epistemic gate fails, abstain or retrieve. Do not optimize through uncertainty by inventing state.

## 11. Live output contract

Normal live output remains concise.

If the epistemic gate passes, use the DM-0.9 decision sequence to the next genuine information boundary.

If the gate fails, output only the minimum needed to resolve it, for example:

- `Need tooltip for Smithing For Dummies before choosing.`
- `Item identity uncertain. Hover the 2g sale item.`
- `Stored prior not retrieved. Cannot rank it as a prior yet.`

Do not add speculative explanation that could itself contaminate the decision.

## 12. Regression case. Berserker skill screen

Observed failure:

- Berserker run reached skill selection.
- Smithing For Dummies appeared alongside other skills.
- Engine recommended it immediately because the name semantically matched the current Hammer + Dragon Gloves/smithing context.
- The engine did not know the skill effect.
- Player identified the recommendation as fabricated/anchored on the word "smithing."

Failure classification:

**epistemic gate failure. Semantic association was promoted into fake mechanic knowledge.**

Mandatory lesson:

> Names and themes are not mechanics. Unknown mechanics remain UNKNOWN until observed or verified.

This case is considered a regression of previously discussed Hexblade/prior-provenance failures, not a new isolated edge case.
