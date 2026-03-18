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

### Jurisdiction Overlay Convention

Skills covering state-specific content follow a four-rule pattern:

1. **Scope callout** — blockquote at the top of SKILL.md naming reference jurisdictions and which sections are state-specific
2. **State-specific headers** — section headers include jurisdiction name in parentheses: `### CPE Requirements (Florida DBPR)` not `### CPE Requirements`
3. **National/federal content** — stands alone without jurisdiction qualification
4. **Reference files** — follow the same header pattern for state-specific subsections

The reference implementation is `firm-operations:quality-compliance`. Skills with only incidental state mentions (e.g., a Florida form number in an example list) do not need the overlay — it applies when a skill has dedicated state-specific sections.

### Reference Provenance Frontmatter

Every reference file carries YAML frontmatter with four fields:

- `authority_level` — `primary` (IRC, FASB ASC, IRS forms/pubs, state statutes, PCAOB/AICPA standards), `secondary` (IRS ATGs, AICPA practice aids, MTC model statutes, vendor official docs), or `tertiary` (practitioner blogs, community forums)
- `effective_from` — `evergreen` for procedural content; specific date (YYYY-MM-DD) for content tied to legislation (e.g., `2017-12-22` for TCJA)
- `last_verified` — date the content was last reviewed (YYYY-MM-DD)
- `jurisdiction` — `federal` for purely federal content, `FL` for Florida-specific files, `multistate` for files covering multiple states or mixed federal+state content, `N/A` for platform/operational content with no jurisdictional scope

## Key Constraints

- 30-skill display cap per Claude Code session — skills beyond this are hidden from the model
- ~16,000-character budget for skill descriptions (2% of context window) — every word in a description is a ranking signal, not narrative
- Total skill count exceeds the display cap — check `plugins/*/skills/` for current count
