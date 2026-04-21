# Architecture

**Analysis Date:** 2026-04-21

## Pattern Overview

**Overall:** Static Content Site with AI-Managed State

**Key Characteristics:**
- No runtime code or build process - pure static HTML, CSS, and JSON
- Claude AI acts as the "application layer" - reading JSON state, executing commands, generating HTML
- Campaign state stored in flat JSON files (source of truth)
- Content pages are static HTML with embedded CSS (no external stylesheets)

## Layers

**Data Layer:**
- Purpose: Single source of truth for campaign state
- Location: `data/`
- Contains: JSON files defining all campaign entities and relationships
- Key Files:
  - `data/battles.json` — Battle index with status, combatants, results, M42 dates
  - `data/alliances.json` — Alliance rosters, CVP/SAP tracking, member lists
  - `data/planets.json` — Planetary control percentages, boons, thresholds
  - `data/factions.json` — Faction metadata, Wahapedia URLs, alliance affiliations
  - `data/rules.md` — Campaign rules reference (markdown)
- Depends on: Nothing
- Used by: Commands, presentation pages (for cross-reference)

**Command Layer:**
- Purpose: Define Claude AI operations that modify campaign state
- Location: `.claude/commands/`
- Contains: Skill definition files (markdown) describing input schema, validation, and execution steps
- Key Commands:
  - `prep-battle.md` — Create new battle page, update battles.json
  - `finalize-battle.md` — Complete battle with results, update indexes
  - `set-agendas.md` — Set faction agendas for a battle
  - `validate.md` — Integrity checks across all data files
- Depends on: Data Layer (reads/writes JSON)
- Used by: Claude AI when user invokes `/command`

**Presentation Layer:**
- Purpose: Static HTML pages for viewing campaign content
- Location: Root directory, `battles/`, `players/`, `planets/`, `alliances/`, `rules/`
- Contains: HTML pages with embedded CSS styling (Warhammer 40K gothic theme)
- Depends on: Data Layer (content reflects JSON state)
- Used by: Human readers (via browser)

**Memory Layer:**
- Purpose: Session continuity and context for Claude AI
- Location: `.claude/memory.md`
- Contains: Current phase, pending battles, recent actions, player notes
- Depends on: Data Layer (summarizes state)
- Used by: Claude AI for context between sessions

## Data Flow

**Battle Creation Flow:**

```
User: /prep-battle 016, Strategic Strike, necrons vs death-guard

1. Claude reads data/factions.json (validate factions exist)
2. Claude reads data/alliances.json (get army names)
3. Claude reads data/battles.json (get next M42 date, verify battle number)
4. Claude generates narrative title
5. Claude creates battles/battle_016_[slug].html
6. Claude updates data/battles.json (add entry with status: pending)
7. Claude updates battles/index.html (add to Upcoming section)
8. Claude updates .claude/memory.md (add to pending battles)
```

**Battle Finalization Flow:**

```
User: /finalize-battle 016
User provides: score, victor, casualties, MfG, agenda completions

1. Claude reads data/battles.json (verify battle exists, is pending)
2. Claude reads existing battle HTML page
3. Claude generates VOX-INTERCEPT narrative (<2000 chars)
4. Claude updates battle HTML (add victory banner, after-action report)
5. Claude updates data/battles.json (status: completed, result, scores)
6. Claude updates battles/index.html (move to Completed section)
7. Claude increments m42_date in data/battles.json
8. Claude updates .claude/memory.md
```

**State Management:**
- All state lives in `data/*.json` files
- Claude reads state, performs operations, writes updated state
- No runtime state or database - JSON files are checked into git
- Human edits allowed but discouraged (use commands for consistency)

## Key Abstractions

**Battle:**
- Purpose: Single engagement between two factions
- Examples: `data/battles.json` entries, `battles/battle_*.html` pages
- Pattern: JSON metadata + HTML presentation page
- Lifecycle: pending → in_progress → completed

**Alliance:**
- Purpose: Team of factions competing for campaign victory
- Examples: `data/alliances.json` (guardians, despoilers, marauders)
- Pattern: Container for factions with shared CVP/SAP pools

**Faction:**
- Purpose: Playable army with rules links and alliance membership
- Examples: `data/factions.json` entries (black-templars, necrons, etc.)
- Pattern: Slug-keyed reference data with external URLs

**Planet:**
- Purpose: Strategic location with control percentages and boons
- Examples: `data/planets.json` entries, `planets/*.md` pages
- Pattern: Per-alliance control tracking with threshold-based rewards

## Entry Points

**Human Entry Points:**
- `index.html` — Campaign landing page (Quick Start Guide)
- `battles/index.html` — Battle log index
- `players/index.html` — Player roster index
- `rules/index.html` — Rules reference

**Claude Command Entry Points:**
- `CLAUDE.md` — Root project instructions, skill references
- `.claude/commands/*.md` — Individual command definitions
- `.claude/memory.md` — Session context

## Error Handling

**Strategy:** Validation-first with human review

**Patterns:**
- Commands validate inputs before making changes
- `/validate` command checks cross-references and data integrity
- Failed operations produce error messages, not partial updates
- No automatic fixes - report issues for human decision

## Cross-Cutting Concerns

**Styling:**
- Gothic Warhammer 40K theme with CSS custom properties
- Colors: --accent (dark red), --accent-gold, --bone, --bg-dark
- Fonts: Cinzel (headers), Crimson Text (body)
- All CSS embedded in each HTML file (no external stylesheets)

**URLs and Links:**
- External rules link to Wahapedia (URLs stored in `data/factions.json`)
- Internal navigation via relative paths
- External campaign tracker: Administratum (Goonhammer)

**Narrative Tone:**
- VOX-INTERCEPT format for battle reports
- Gothic 40K language ("The Emperor protects", "heretic", etc.)
- Under 2000 characters for VOX transmissions
- Reference unit crusade names, not generic descriptions

**M42 Timestamps:**
- Campaign uses in-universe dates (e.g., 823.M42)
- Tracked in `data/battles.json` as `m42_date` integer
- Incremented by 1 for each completed battle
- Format: `[number].M42`

---

*Architecture analysis: 2026-04-21*
