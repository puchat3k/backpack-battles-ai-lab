# Decision Model DM-0.6 — STAGING RECORD

**Status:** promoted to `model/decision-model-DM-0.6.md`  
**Base model:** DM-0.5  
**Patch context:** 1.1.8  
**Strategy prior:** unchanged

This staging record was promoted after repeated live-run failures under DM-0.5, including local item evaluation, idle-capital accumulation, missed combination logic, spatially invalid deployment assumptions, and unsupported screenshot item identification.

The active DM-0.6 implementation adds:

- full-state gate before action;
- item-identity confidence states;
- shop-phase transaction planning;
- deployed-vs-idle capital accounting;
- space/eviction gate;
- combination-first audit;
- explicit action-queue semantics;
- autonomous placement authority;
- voluntary Player-guidance handling;
- decisive-loss structural reset;
- reroll discipline;
- bounded external validation;
- privacy-by-design firewall;
- explicit non-overengineering architecture constraint.

See `model/decision-model-DM-0.6.md` for the active policy.
