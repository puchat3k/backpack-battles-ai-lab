# Custom GPT setup

This repository can be used to reproduce the Backpack Battles AI Lab workflow in a separate ChatGPT custom GPT.

This is a reproducibility package, not a trained Backpack Battles model.

## Recommended configuration

### Name

`Backpack Battles AI Lab`

### Description

Patch-aware Backpack Battles decision assistant that reasons from screenshots using versioned strategy priors, adaptive convergence, economy logic, placement tolerances and experiment logging.

### Instructions

Use `custom-gpt/INSTRUCTIONS.md` as the behavioral instruction set.

### Knowledge files

Recommended starting knowledge package:

1. `patches/1.1.8.md`
2. `model/strategies-H2.md`
3. `model/decision-model-DM-0.2.md`
4. `docs/run-schema.md`

OpenAI's current GPT builder documentation supports uploaded knowledge files and recommends putting behavioral rules in Instructions while using Knowledge for source/reference material. The current builder documentation states a GPT can attach up to 20 knowledge files, each up to 512 MB, though product limits may change over time.

### Capabilities

For the screenshot workflow, image understanding is inherent to the ChatGPT interface. Web search can be enabled when current patch verification or external mechanic research is needed.

An external Action is not required for the basic community version. A future Action could connect the GPT to a public experiment API for current strategy priors and aggregate run results.

## Conversation starters

- `Start a new run. I am playing Random and here is my first shop screenshot.`
- `Analyze this board and make the next shop decision.`
- `I reached 10 wins. Bank or enter Survival?`
- `Review this finished run and identify the most important decision error.`

## Important behavior

The GPT should:

- treat the current patch and H/DM version as explicit experiment variables
- make decisions rather than merely list possibilities when enough state is visible
- keep operator-facing gameplay instructions concise
- distinguish strategy quality from stochastic combat results
- avoid forcing H2 build paths when the board supports a stronger emergent line
- request extra screenshots only when uncertainty has material expected-value impact
- clearly mark mechanic uncertainty instead of inventing item behavior

## Privacy and isolation

A community GPT should use **only** the public Backpack Battles files from this repository.

Do not upload or include unrelated private documents, personal memory exports, CRM data, email, private messages, other project material, database credentials or connector secrets.

Custom GPTs do not use the builder's saved ChatGPT memory, personal custom instructions, or previous conversations; each GPT conversation starts fresh. This helps reproducibility, but users should still review any knowledge files and external Actions they configure themselves.

## Sharing

OpenAI currently allows GPTs to be kept private, shared by link, shared in eligible workspaces, or published publicly when account/workspace eligibility permits. Public GPTs that use Actions have additional requirements, including a valid privacy-policy URL for public Actions.

For this project the simplest community distribution is:

1. canonical public repository = inspectable specification
2. official shared GPT = low-friction usage
3. community forks = users reproduce or modify the public files and instructions

## Version identity

At run start, identify the active configuration, for example:

`Patch 1.1.8 | H2 | DM-0.2 | reasoning model version`

The actual model available to a GPT can change over time as ChatGPT models are updated or retired, so the reasoning-model version should remain an experimental variable rather than being assumed constant.

## Sources

Current product behavior should be verified against OpenAI's official GPT documentation before major releases because GPT builder capabilities and limits can change.