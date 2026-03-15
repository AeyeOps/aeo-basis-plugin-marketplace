# aeo-basis-plugin-marketplace

Claude Code plugin marketplace for an accounting firm serving C-corporations.
Plugins span four service lines plus a foundation layer and platform integration.

## Repository Structure

- `.claude-plugin/marketplace.json` — Marketplace manifest (plugin registry)
- `plugins/` — Plugin directories, each self-contained with `.claude-plugin/plugin.json`
- `plugins/*/skills/*/references/` — Source knowledge base articles per skill
- `README.md` — Architecture documentation, skill map, constraints, and contributing guidelines

## Plugins

- `accounting-foundation` — cross-cutting foundation knowledge
- `qbo-integration` — QuickBooks Online platform integration (swappable)
- `bookkeeping` — BK service line
- `tax-prep` — TX service line
- `financial-planning` — FP&A service line
- `firm-operations` — OPS (internal)

## Build Conventions

- SKILL.md files are synthesized from KB sources — not copy-paste collages
- Each plugin must be self-contained (cross-plugin `@` imports fail silently)
- Cross-plugin knowledge sharing uses runtime skill invocation, not file duplication
- KB source files live in `plugins/*/skills/*/references/` alongside each SKILL.md
- Domain plugins are platform-neutral — delegate accounting system execution to `qbo-integration` skills
- Content must not contain company-specific bank names, broker names, provider names, or client placeholders — skills are for any accounting professional, not one firm

## SKILL.md Authoring

- SKILL.md body: majority synthesized operational content, remainder as annotated navigation pointers to references/
- Descriptions use the v3 domain-fused pattern: "Contains verified [2-3 artifacts]... [domain keywords]. Consult when [trigger scenarios]." — leading with specific artifacts signals knowledge the model can't reproduce from training alone
- Reference files may be duplicated across skills because each skill's references/ must be self-contained (cross-plugin imports fail silently)

## Key Constraints

- 30-skill display cap per Claude Code session — skills beyond this are hidden from the model
- ~16,000-character budget for skill descriptions (2% of context window) — every word in a description is a ranking signal, not narrative
- Total skill count exceeds the display cap — check `plugins/*/skills/` for current count
