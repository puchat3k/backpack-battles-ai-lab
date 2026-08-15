# Screenshot Analysis Concept

A potential player-facing use of the Backpack Battles analysis engine is a simple screenshot analysis flow.

A player uploads a screenshot of their current game state and optionally asks a specific question, such as:

> Should I pivot from Bloodthorne to Darksaber?

The engine interprets the visible board and shop using patch-aware game knowledge and the decision principles developed through gameplay analysis, then returns a structured advice package.

## Analysis package

A useful response can include:

1. **Recommendation** - the action the engine currently prefers.
2. **Confidence** - how strongly the available evidence supports the recommendation.
3. **Reasoning** - the most important factors driving the decision.
4. **Alternative line** - the strongest competing option.
5. **Decision conditions** - what additional state or shop developments would change the recommendation.
6. **Board observations** - visible weaknesses, synergies, replacement opportunities, or efficiency improvements.
7. **Uncertainty** - relevant information that cannot be inferred reliably from the screenshot.

The community already uses a similar human workflow: submit a screenshot, explain the problem, and ask other players what they would do. The screenshot analysis engine applies the same interaction pattern to an LLM-generated second opinion.

A worked example is captured in [`examples/reddit-pivot-bloodthorne-darksaber.md`](../examples/reddit-pivot-bloodthorne-darksaber.md).

## Potential capabilities

The same input/output pattern can support several related questions:

- **What should I buy?** Rank relevant shop options.
- **What should I sell or replace?** Identify low marginal-value pieces and replacement opportunities.
- **Should I pivot?** Compare the value of the current trajectory with the transition cost and expected value of an alternative build.
- **Improve my board.** Suggest high-value changes without requiring a complete rebuild.
- **What went wrong?** Analyze a late-game or failed board and identify likely structural weaknesses.
- **What am I building toward?** Infer plausible build trajectories from an improvised board.
- **Second opinion.** Let the player provide their intended move and have the engine critique it rather than choose independently.

## Infrastructure

A deliberately simple implementation could look like:

```text
Browser
   |
Screenshot upload + optional question/context
   |
Vision-capable LLM
   |
Patch-aware game knowledge + versioned decision logic
   |
Structured analysis
   |
Human-readable advice
   |
Optional player feedback/outcome
   |
Structured datastore
```

### Minimal stack

| Layer | Possible implementation |
| --- | --- |
| Frontend | Conventional web application |
| Hosting | Managed web hosting |
| Analysis | Multimodal LLM API |
| Model response | Structured JSON/schema |
| Database | Relational datastore |
| Screenshot storage | Object storage |
| Game knowledge and decision logic | Version-controlled files |
| Analytics | SQL initially |
| Source control/deployment | Standard source-control workflow |

The application layer is conventional enough to be vibe-coded if desired. The more important component to control explicitly is the reasoning layer: patch assumptions, game knowledge, decision criteria, heuristics and engine versions should remain inspectable and versioned.

**Vibe-code the commodity application layer; explicitly design and version the decision engine.**

## Data model

A minimal useful schema separates the submitted state, the engine's interpretation, its recommendation, and subsequent feedback.

```text
analysis_request
- id
- timestamp
- patch_version
- screenshot_reference
- user_question
- optional_context

analysis
- request_id
- engine_version
- detected_state
- recommendation
- confidence
- reasoning
- alternative_lines
- decision_conditions
- uncertainty

feedback
- request_id
- useful
- recommendation_followed
- user_correction
- eventual_result
```

This distinction matters because an incorrect recommendation can originate from several different failure modes: incorrect visual interpretation, stale game knowledge, weak decision logic, missing context, or a reasonable probabilistic decision producing a bad outcome.

## Feedback and data loop

Optional user feedback could turn isolated analyses into decision/outcome records:

```text
game state
   -> engine interpretation
   -> recommendation
   -> player action
   -> outcome
   -> feedback/correction
```

Over time this could provide evidence for evaluating recommendation quality and identifying recurring engine failure modes. Patch version and engine version should remain attached to each observation so results remain interpretable as both the game and the analysis logic change.

## Limitations

A screenshot is not a complete game history. The engine may not know what alternatives were previously available, why particular purchases were made, or what future shop states will occur. Vision can also misidentify items or board relationships, and game knowledge can become stale after patches.

The useful framing is therefore not an authoritative solver, but a **patch-aware second opinion based on the state and context supplied by the player**, with uncertainty and decision-changing conditions made explicit.