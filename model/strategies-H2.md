# Strategy priors H2

**Patch:** 1.1.8  
**Status:** active pre-live priors  
**Scope:** 7 characters x 3 strategies = 21 hypotheses

These are heuristic strategy priors, not empirical win rates and not outputs of a full deterministic game simulation. H2 carries forward viable H1 archetypes and applies patch-sensitive adjustments where 1.1.8 materially affects the line.

## Adventurer

| Code | Strategy | Subclass | Assembly | Climb | Confidence | Patch interpretation |
|---|---|---|---:|---:|---:|---|
| H2.17 | Dual-wield Twine weapon tempo | Weaponmaster | 0.80 | 83.75 | 69 | Mercury Elemental, Very Long Spear and Scissorswords buffs improve weapon-tempo lines; Boomerang stamina nerf offsets slightly. |
| H2.16 | Adaptive trade/value shell | Merchant | 0.86 | 83.65 | 61 | Level Up and Fedora changes slightly reduce generic value acceleration, but the line retains very high flexibility. |
| H2.18 | Turtle shield sustain | Shieldmaster | 0.84 | 82.75 | 68 | Heart Shield buff gives the defensive shell a small direct improvement. |

## Berserker

| Code | Strategy | Subclass | Assembly | Climb | Confidence | Patch interpretation |
|---|---|---|---:|---:|---:|---|
| H2.10 | Anvil Double Axe | Blacksmith | 0.84 | 84.20 | 75 | Remains the highest reliability prior; do not force if Double Axe timing is poor. |
| H2.12 | Resistance sustain fatigue | Chieftain | 0.83 | 83.60 | 76 | Deerwood Guardian and Dragonscale Armor buffs materially strengthen this shell. |
| H2.11 | Double Axe + Iron Goobert | Fighter | 0.82 | 82.40 | 69 | No major direct patch delta to core line. |

## Engineer

| Code | Strategy | Subclass | Assembly | Climb | Confidence | Patch interpretation |
|---|---|---|---:|---:|---:|---|
| H2.19 | Tesla Rapier charge scaling | Electrician | 0.84 | 84.80 | 78 | Tesla Coil and Electric Torch buffs materially improve charge throughput and tempo. |
| H2.20 | Mecha Armor projectile/throwing shell | Power Pilot | 0.80 | 80.70 | 62 | Strong ceiling, but still penalized for board and component burden. |
| H2.21 | Laboratory Holy Spear sustain/control | Chemist | 0.79 | 80.30 | 67 | Poison Grenade and Spring Loader buffs add modest control/tempo value. |

## Mage

| Code | Strategy | Subclass | Assembly | Climb | Confidence | Patch interpretation |
|---|---|---|---:|---:|---:|---|
| H2.13 | Freeze Water Elemental | Waterbender | 0.80 | 81.55 | 70 | Scholar Bag mana duplication improves mana throughput. |
| H2.14 | Mana sword + secondary weapon | Shiny Chariot | 0.78 | 80.10 | 69 | Scholar Bag buff improves consistency for the mana weapon shell. |
| H2.15 | Cupcake Staff spirit scaling | Spectromancer | 0.74 | 76.90 | 65 | Cat Spirit Companion cooldown buff improves spirit throughput; remains a higher-ceiling, lower-reliability line. |

## Pyromancer

| Code | Strategy | Subclass | Assembly | Climb | Confidence | Patch interpretation |
|---|---|---|---:|---:|---:|---|
| H2.7 | Flame Whip heat engine | Firebender | 0.82 | 82.80 | 72 | No material direct change to selected core. |
| H2.8 | Burning Banner lightsaber | Crusader | 0.79 | 80.70 | 67 | No material direct change to selected core. |
| H2.9 | Dragon Nest / Blazing-Obsidian dragon shell | Scalewarden | 0.77 | 78.50 | 66 | High ceiling remains offset by space and component burden. |

## Ranger

| Code | Strategy | Subclass | Assembly | Climb | Confidence | Patch interpretation |
|---|---|---|---:|---:|---:|---|
| H2.2 | Poison Ivy pressure | Pathfinder | 0.81 | 81.40 | 70 | No material direct patch change to selected core. |
| H2.1 | Piercing Arrow / Red Orchid crit | Hunter | 0.78 | 81.30 | 68 | No material direct patch change to selected core. |
| H2.3 | Feedbox Rose Whip / pet-food tempo | Beastmaster | 0.75 | 79.05 | 64 | Boiling Pot and Stankus' Toothpick improve food-heavy tempo; Spicy Banana adds situational upside. |

## Reaper

| Code | Strategy | Subclass | Assembly | Climb | Confidence | Patch interpretation |
|---|---|---|---:|---:|---:|---|
| H2.5 | Debuff stun-dagger control | Hexblade | 0.80 | 81.40 | 70 | Moves slightly ahead of Snake after the Snake HP nerf. |
| H2.4 | Snake Rapier poison scaling | Venomancer | 0.82 | 81.20 | 72 | Snake maximum health 40 -> 38 is a small direct nerf. |
| H2.6 | Haircomb Bloodthorne sustain | Vampiress | 0.78 | 78.50 | 66 | No major direct patch delta. |

## Score interpretation

- **Assembly** is a heuristic estimate of practical assembly reliability, not an observed probability.
- **Climb** is a composite hypothesis score intended to balance combat strength, tempo, flexibility, economy, space burden and assembly reliability.
- **Confidence** measures confidence in the prior, not predicted win probability.

The live decision model is allowed to reject all three priors for a character when shop and board evidence support a stronger emergent line.