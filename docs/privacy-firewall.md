# BPB Privacy Firewall

## Scope

This public repository is strictly scoped to the Backpack Battles experiment.

The human participant is referred to publicly as **Player**.

## Prohibited cross-context leakage

Do not publish or persist into BPB artifacts:

- real-world identity details;
- professional titles, callsigns, or unrelated role labels;
- unrelated project/workstream names or internal operating terminology;
- private correspondence, CRM/customer data, contacts, credentials, or account metadata;
- unrelated personal context;
- filenames, screenshots, comments, commit text, or metadata that unnecessarily identify the participant outside the BPB experiment.

## Publication rule

Before any BPB artifact is published, edited, or committed:

1. verify that the content is necessary for the game experiment;
2. normalize participant references to **Player**;
3. remove unrelated-project terminology and identity-bearing context;
4. check filenames and metadata as well as body text;
5. preserve only game-state, mechanics, decision logic, experimental outcomes, and relevant public-source references.

Cross-context terminology appearing in private conversation is never authorization to publish it.

## Screenshots

Publish screenshots only when they materially support a run log, decision case, or experimental finding. Scrub or avoid captures containing unrelated desktop/account information or identifying metadata.

## Remediation

If cross-context leakage is detected:

- remove or replace it immediately in the current repository state;
- do not reproduce the leaked term in subsequent documentation merely to explain the incident;
- record only the generic process failure when useful for regression testing.

Historical Git commit retention is a separate consideration from the current working tree. Avoid introducing sensitive material in the first place rather than relying on later deletion.
