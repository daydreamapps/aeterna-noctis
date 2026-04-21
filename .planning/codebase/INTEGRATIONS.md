# External Integrations

**Analysis Date:** 2026-04-21

## APIs & External Services

**No APIs Consumed:**
- This is a static site with no backend
- No fetch/XHR calls to external APIs
- All data is local JSON files

## Third-Party Services

**Wahapedia (Rules Reference):**
- Purpose: Warhammer 40K rules and faction agendas
- Integration: External links only (no API)
- URLs stored in: `data/factions.json` (`agendas_url`, `crusade_rules_url`)
- URL Pattern: `https://wahapedia.ru/wh40k10ed/...`
- Files referencing: 33 files across `battles/`, `players/`, `rules/`

**Administratum (Campaign Tracker):**
- Purpose: External campaign tracking tool by Goonhammer
- Integration: External links only (no API)
- URL: `https://administratum.goonhammer.com/campaigns/019ccdaf-2230-78be-ae32-5440e44659b7`
- Files referencing: 12 files (toolbars, index pages)

**Google Fonts (Typography):**
- Purpose: Web fonts for page styling
- Integration: CSS `@import` via `<link>` tags
- URL Pattern: `https://fonts.googleapis.com/css2?family=...`
- Fonts loaded:
  - Cinzel (weights: 500, 700)
  - Crimson Text (weights: 400, 600; styles: normal, italic)
- Files referencing: 30 HTML files

## Data Sources

**Local JSON Files:**
| File | Purpose | Updated By |
|------|---------|------------|
| `data/battles.json` | Battle index, metadata, results | `/finalize-battle` command |
| `data/alliances.json` | Alliance standings, CVP/SAP, rosters | Manual / battle completion |
| `data/planets.json` | Planetary control percentages, boons | Manual / raid results |
| `data/factions.json` | Faction definitions, Wahapedia URLs | Rarely updated |

**Local Markdown Files:**
- `data/rules.md` - Campaign mechanics reference
- `planets/*.md` - Planet descriptions and lore
- `alliances/*.md` - Alliance background
- `chronicles/**/*.md` - Historical battle summaries

**Image Assets:**
- `images/aeterna_noctis_banner.jpg` - Campaign banner
- Deployment maps linked from Wahapedia (external)

## Deployment Maps (External Images)

**Source:** Wahapedia mission diagrams
**Pattern:** `https://wahapedia.ru/wh40k10ed/img/maps/ng/*.png`
**Example:** `FrontLineWarfare.png`, `Stranglehold.png`

## External URLs Referenced

**Rules & Game Resources:**
- Nachmund Gauntlet rules: `https://wahapedia.ru/wh40k10ed/the-rules/nachmund-gauntlet/`
- Faction-specific Crusade rules: See `data/factions.json` for full list

**Campaign Tracker:**
- Administratum campaign page: `https://administratum.goonhammer.com/campaigns/019ccdaf-2230-78be-ae32-5440e44659b7`

## Webhooks & Callbacks

**Incoming:**
- None

**Outgoing:**
- None

## Authentication & Identity

**Auth Provider:**
- None - Static site with no authentication
- No user accounts or login functionality

## Monitoring & Observability

**Error Tracking:**
- None

**Logs:**
- None - No server-side logging
- Git commit history serves as change log

## CI/CD & Deployment

**Hosting:**
- Not yet configured (local development only)
- Suitable for: GitHub Pages, Netlify, Vercel, any static host

**CI Pipeline:**
- None configured
- Manual git commits

## Data Flow Summary

```
┌─────────────────────────────────────────────────────────┐
│                    Static HTML Site                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────────┐     ┌──────────────┐                  │
│  │ battles/*.html│◄────│ data/battles │                  │
│  └──────────────┘     │     .json    │                  │
│                       └──────────────┘                  │
│  ┌──────────────┐     ┌──────────────┐                  │
│  │ players/*.html│◄────│data/alliances│                  │
│  └──────────────┘     │    .json     │                  │
│                       └──────────────┘                  │
│  ┌──────────────┐     ┌──────────────┐                  │
│  │  External    │     │data/factions │                  │
│  │  Links       │◄────│    .json     │                  │
│  │ (Wahapedia)  │     │  (URLs)      │                  │
│  └──────────────┘     └──────────────┘                  │
│                                                          │
└───────────────────────┬─────────────────────────────────┘
                        │
            ┌───────────▼───────────┐
            │   External Services   │
            ├───────────────────────┤
            │ • Wahapedia (rules)   │
            │ • Administratum       │
            │ • Google Fonts        │
            └───────────────────────┘
```

## Environment Configuration

**Required env vars:**
- None

**Secrets location:**
- No secrets required
- No API keys needed
- All external services are public links

---

*Integration audit: 2026-04-21*
