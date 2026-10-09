# Domain docs

How the engineering skills read this repo's domain documentation.

## Layout: single-context

```
/
├── GLOSSARY.md          ← the shared vocabulary
└── docs/
    └── adr/             ← architecture decision records
        ├── 0001-<slug>.md
        └── …
```

Neither exists yet. The `domain-modeling` skill creates them only when there is something to write: `GLOSSARY.md` when the first term is resolved, `docs/adr/` when the first decision earns an ADR.

## Before exploring, read these

- `GLOSSARY.md` at the repo root.
- The ADRs in `docs/adr/` that touch the area you're about to work in.

If either is missing, carry on without comment. Don't flag the absence and don't offer to create them; that's `domain-modeling`'s job once there's something to write.

## Use the glossary's vocabulary

When your output names a domain concept (an issue title, a refactor proposal, a hypothesis, a test name), use the term exactly as `GLOSSARY.md` defines it. Don't drift to synonyms the glossary avoids.

If the concept you need isn't in the glossary, either you're inventing language the project doesn't use (reconsider), or you've found a real gap (note it for `domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, say so explicitly rather than silently overriding it:

> _Contradicts ADR-0003 (…), but worth reopening because…_
