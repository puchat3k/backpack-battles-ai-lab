# Run #001 — Anonymized Checkpoint Log

**Status:** paused after Round 6 combat  
**Patch:** 1.1.8  
**Character:** Reaper  
**Selection:** Random / Random  
**Strategy prior:** H2  
**Played under:** DM-0.2  
**Resume under:** DM-0.3  
**Public operator label:** Player

This document intentionally contains no real-world identity information. It records only Backpack Battles experiment state and model-learning observations.

## Current checkpoint

- Record: **4 wins / 2 losses**
- Lives remaining: **3**
- Resume: next shop after Round 6 combat
- Gold entering checkpoint: **3**
- Current shell: Pan + food + Torch
- Stored strategic option: Thorn Whip
- Corrupted Crystal owned and used in backpack during the last combat
- Pan/Crystal and Cap/Crystal transformations were intentionally suppressed
- Current directionality: Bloodthorne **LEANING**, not committed

## Selected decision divergences

### Thorn Whip / Bloodthorne

Player preferred buying an earlier full-price Thorn Whip and waiting for Hungry Blade. The model passed because prerequisite and transition risk outweighed the 8g commitment. Later, a 4g discounted Thorn Whip appeared and the model bought it. The divergence remains useful because it exposed sensitivity to pathing, price, transition cost, and commitment timing.

### Bank 10g before Round 5 combat

Player was skeptical of entering combat with 10g unspent. The model rejected a discounted Dagger and the 9g Poison Dagger package, enabled a zero-additional-gold Torch transformation, and banked the rest.

Forecast: ~62% win.  
Outcome: win with 26/70 HP.

This is positive but weak evidence for disciplined resource allocation; it is not proof that banking was optimal.

## Important model failures observed

### Discount salience / Hammer

A discounted Hammer was initially recommended despite weak Reaper pathing, no supporting dagger package, no relevant skill, and space pressure. The issue was not simply "discounts are bad"; the engine evaluated economic attractiveness before strategic feasibility.

### Stale reroll threshold

The engine initially recommended a 2g reroll with only 8g liquid despite a discounted broadly useful defensive item being available. It failed to refresh the search threshold after the economic regime changed.

### Pending transformation omission

Sword + Coal could form Torch, but after an early defer decision the engine stopped re-evaluating the transformation across later screenshots. The option had not disappeared; it was merely suppressed by locks.

### Isolated-item evaluation

Dagger and Poison Potion were initially considered separately instead of generating the immediately reachable Poison Dagger state. The general correction is to evaluate reachable board states rather than only visible shop items.

### Question-as-criticism contamination

Player audit questions sometimes caused the model to assume an error existed and invent a correction. Under DM-0.3, questions trigger a neutral re-scan rather than directional updating.

## Combat forecast checkpoint

- Round 5: predicted win ~62% → **win**, 26/70 HP remaining
- Round 6: predicted win ~65% → **loss**, opponent 23/100 HP remaining

The Round 6 miss is a calibration datapoint. It does not by itself establish that the Corrupted Crystal purchase or placement was incorrect.

## DM-0.3 changes motivated by the run

DM-0.3 adds:

- reachable-state generation across owned/shop recipes, locks, deployment, sales, rerolls, and hold
- persistent re-check of deferred recipes and stored strategic items
- neutral handling of Player challenges and questions
- dynamic reroll thresholds based on current cost and liquidity
- directionality as a critical-mass process rather than a fixed round trigger
- full transition-cost accounting for pivots
- lookahead to known deterministic decision nodes
- explicit lock/unlock checks before combat
- pre-combat forecasting with exception-triggered result screenshots

These are generalized reasoning changes. They are not intended to reproduce Player strategy preferences.

## Privacy

Public experiment material uses **Player** only. No real name, personal account information, unrelated private projects, private correspondence, credentials, third-party confidential information, or other non-public context belongs in this repository.