# Codebase Structure

**Analysis Date:** 2026-04-21

## Directory Layout

```
crusade/
├── .claude/                    # Claude AI configuration and commands
│   ├── commands/               # Skill definitions (markdown)
│   ├── memory.md               # Session context and state summary
│   └── settings.local.json     # Local Claude settings
├── .planning/                  # Planning and analysis documents
│   └── codebase/               # Codebase mapping documents
├── alliances/                  # Alliance detail pages (markdown)
├── battles/                    # Battle pages (HTML)
│   ├── _template.html          # Battle page template
│   ├── index.html              # Battle log index
│   └── battle_*.html           # Individual battle pages
├── chronicles/                 # Historical narrative summaries
│   └── phase_0/                # Phase 0 (pre-campaign) records
├── data/                       # Campaign state (JSON + rules)
│   ├── alliances.json          # Alliance rosters and standings
│   ├── battles.json            # Battle index and metadata
│   ├── factions.json           # Faction references and URLs
│   ├── planets.json            # Planetary control data
│   └── rules.md                # Campaign rules reference
├── hulk/                       # Space Hulk strategic content
│   └── strategic_sites.md      # Strategic site definitions
├── images/                     # Image assets
├── planets/                    # Planet detail pages (markdown)
├── players/                    # Player army rosters
│   ├── _template.html          # Roster page template
│   ├── index.html              # Player roster index
│   ├── *.html                  # Army roster pages (HTML)
│   └── *.md                    # Army roster data (markdown)
├── rules/                      # Rules reference pages
│   ├── index.html              # Rules landing page
│   └── errata.html             # Campaign errata
├── CLAUDE.md                   # Root project instructions
├── index.html                  # Campaign landing page (Quick Start)
├── QUICK_START_GUIDE.md        # Player quick start (source)
└── RESOURCES.md                # External links and resources
```

## Directory Purposes

**`.claude/`**
- Purpose: Claude AI configuration and skill definitions
- Contains: Markdown command files, memory, settings
- Key files:
  - `commands/prep-battle.md` — Battle creation command
  - `commands/finalize-battle.md` — Battle completion command
  - `commands/set-agendas.md` — Agenda setting command
  - `commands/validate.md` — Data validation command
  - `memory.md` — Session state and pending items

**`data/`**
- Purpose: Single source of truth for campaign state
- Contains: JSON data files and rules reference
- Key files:
  - `battles.json` — Master battle index with all metadata
  - `alliances.json` — Alliance membership and standings
  - `factions.json` — Faction slugs, names, Wahapedia URLs
  - `planets.json` — Planetary control percentages
  - `rules.md` — Campaign rules quick reference

**`battles/`**
- Purpose: Individual battle pages and index
- Contains: HTML pages for each battle, template, index
- Key patterns:
  - Files named `battle_NNN_[slug].html`
  - Three-digit zero-padded battle numbers
  - Slug derived from narrative title (lowercase, hyphens)

**`players/`**
- Purpose: Army rosters with unit details and narratives
- Contains: HTML roster pages, markdown source, template, index
- Key patterns:
  - HTML for presentation, MD for source data
  - Named by army name slug (e.g., `crusade_of_binding_chains.html`)

**`planets/`**
- Purpose: Planet lore and raid history
- Contains: Markdown files for each planet
- Key patterns:
  - Files named `[planet_slug].md`
  - Include boon description, current status, raid history

**`alliances/`**
- Purpose: Alliance overview and standings
- Contains: Markdown files for each alliance
- Key patterns:
  - Files named `[alliance].md` (guardians, despoilers, marauders)
  - Include member roster, CVP/SAP, planetary control summary

**`rules/`**
- Purpose: Campaign rules reference pages
- Contains: HTML rules pages
- Key files:
  - `index.html` — Main rules reference
  - `errata.html` — Campaign-specific errata

**`chronicles/`**
- Purpose: Historical narrative summaries of campaign phases
- Contains: Markdown narrative documents
- Organized by phase subdirectories

## Key File Locations

**Entry Points:**
- `index.html`: Campaign landing page (Quick Start Guide)
- `battles/index.html`: Battle log with completed/upcoming sections
- `players/index.html`: Player roster directory
- `rules/index.html`: Campaign rules reference

**Configuration:**
- `CLAUDE.md`: Root instructions for Claude AI
- `.claude/memory.md`: Session context and state
- `.claude/settings.local.json`: Local Claude settings

**Core Data:**
- `data/battles.json`: Battle index (source of truth for all battles)
- `data/alliances.json`: Alliance rosters and standings
- `data/factions.json`: Faction metadata and external URLs
- `data/planets.json`: Planetary control tracking

**Templates:**
- `battles/_template.html`: Battle page template
- `players/_template.html`: Player roster template

## Naming Conventions

**Files:**
- Battle pages: `battle_NNN_[narrative_slug].html` (e.g., `battle_013_no_ground_given.html`)
- Player rosters: `[army_name_slug].html/.md` (e.g., `crusade_of_binding_chains.html`)
- Planet pages: `[planet_slug].md` (e.g., `sanctum_malakor.md`)
- Alliance pages: `[alliance].md` (e.g., `guardians.md`)

**Slugs:**
- Lowercase with underscores for files: `the_black_tide_rises`
- Lowercase with hyphens for JSON keys: `black-templars`, `death-guard`
- Factions use hyphenated slugs matching Wahapedia URL patterns

**Battle Numbers:**
- Three-digit zero-padded: `001`, `002`, `015`
- Sequential with no gaps
- Used in both filenames and JSON keys

## Where to Add New Code

**New Battle:**
- Primary code: `battles/battle_NNN_[slug].html` (use `_template.html` as base)
- Data update: Add entry to `data/battles.json`
- Index update: Add to `battles/index.html` (Upcoming Battles section)
- Memory update: Add to `.claude/memory.md` pending list

**New Player Roster:**
- Primary code: `players/[army_slug].html` (use `_template.html` as base)
- Markdown source: `players/[army_slug].md`
- Index update: Add to `players/index.html`

**New Planet:**
- Primary code: `planets/[planet_slug].md`
- Data update: Add entry to `data/planets.json`

**New Faction:**
- Data update: Add entry to `data/factions.json`
- Alliance update: Add member to `data/alliances.json`

**New Claude Command:**
- Primary code: `.claude/commands/[command-name].md`
- Reference update: Add to `CLAUDE.md` skills list

## Special Directories

**`.git/`**
- Purpose: Git version control
- Generated: Yes
- Committed: No (is the repo itself)

**`.planning/`**
- Purpose: Planning and analysis documents
- Generated: By analysis tools
- Committed: Yes

**`images/`**
- Purpose: Image assets (banner, etc.)
- Generated: No (manually created)
- Committed: Yes

**`chronicles/`**
- Purpose: Historical phase summaries
- Generated: No (manually written narratives)
- Committed: Yes
- Note: Subdirectories per phase (phase_0, phase_1, etc.)

## File Format Patterns

**HTML Pages:**
- Self-contained with embedded CSS (no external stylesheets)
- Use CSS custom properties for theming
- Include toolbar navigation to main sections
- Gothic 40K visual styling (Cinzel + Crimson Text fonts)

**JSON Data Files:**
- Root object with entity collections
- Entities keyed by slug (e.g., `"black-templars": {...}`)
- Use null for unknown/TBD values
- Dates as ISO strings (YYYY-MM-DD) or M42 integers

**Markdown Files:**
- Frontmatter-style metadata at top (using bold labels)
- Tables for structured data
- Narrative sections with blockquotes
- Consistent header hierarchy (# for title, ## for sections)

---

*Structure analysis: 2026-04-21*
