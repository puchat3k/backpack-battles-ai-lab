# Public experiment schema

This document describes the logical data captured by the Backpack Battles AI Lab. It intentionally omits database credentials, infrastructure secrets and unrelated private data.

## Run

A run records the experimental unit.

Core fields:

- `started_at`
- `ended_at`
- `character`
- `subclass`
- `patch_id`
- `starting_rank`
- `ending_rank`
- `wins`
- `losses`
- `survival_round`
- `result`
- `final_build_label`
- `run_notes`
- `decision_model_version`
- `metadata`

The metadata object is used for experiment provenance such as:

- strategy prior version, e.g. `H2`
- reasoning model version
- character selection mode: `random` or `targeted`
- screenshot/capture references where appropriate

## Decision

A decision belongs to a round and can record:

- sequence number
- decision type
- chosen action
- alternatives
- decision criteria
- estimated probabilities
- concise reasoning summary
- confidence
- outcome notes
- strategy mode
- commitment state
- strategy belief distribution
- option-value score
- pivot-cost score
- immediate-power score
- transition-cost score
- exploration-value score

Not every trivial drag or item movement needs to become a major decision. The dataset should preferentially capture consequential decisions so analysis is not swamped by execution noise.

## Strategy hypothesis

A strategy prior records:

- hypothesis code
- patch version
- character
- strategy name
- subclass
- core items
- support items
- bridge items
- commit signals
- pivot signals
- assembly estimate
- combat-strength score
- tempo score
- flexibility score
- economy score
- space score
- composite climb score
- confidence
- evidence notes
- source references
- sample size
- observed run success rate
- status

H-series versions are immutable snapshots. A material strategic re-evaluation should create H3, H4, etc., rather than silently rewriting H2.

## Decision-model version

Each DM version stores:

- version
- status
- effective date
- summary
- rationale
- source types
- policy definition
- expected effects
- evidence references

DM versions are preserved so run performance can be compared against changes in reasoning policy.

## Primary analysis dimensions

The intended minimum segmentation is:

`patch x H-version x DM-version x reasoning-model version x character x selection mode`

Additional analysis can examine commitment state, strategy mode, decision confidence, matchup context, intervention type and operator execution errors.

## Privacy boundary

The public repository is scoped only to the Backpack Battles AI Lab experiment. No data from unrelated projects, work activity, personal CRM systems, private messages, contacts, or other connected sources belongs in this schema or repository.