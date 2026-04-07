# Prepare Battle

Creates a new battle page with mission briefing and pre-battle information.

## Model
Use **sonnet** for this skill — requires narrative generation and HTML templating.

## Input Schema
```
/prep-battle <battle_number>, <mission_name>, <attacker> vs <defender>, <date>
```

| Field | Type | Required | Example |
|-------|------|----------|---------|
| battle_number | integer | yes | `7` |
| mission_name | string | yes | `Strategic Strike` |
| attacker | faction slug | yes | `black-templars` |
| defender | faction slug | yes | `aeldari` |
| date | YYYY-MM-DD | no | `2026-04-15` |

### Validation
- `battle_number` must not already exist in `data/battles.json`
- `attacker` and `defender` must exist in `data/factions.json`
- `mission_name` should be a valid Nachmund Gauntlet mission

## Data Files to Read
1. `data/factions.json` — Get faction names and agenda URLs
2. `data/alliances.json` — Get army names and alliance affiliations
3. `data/battles.json` — Verify battle number is new, get next M42 date

## What This Command Does

### 1. Creates Battle Page
Uses template structure to create HTML with:
- Toolbar navigation
- Header with battle number, narrative title, mission name
- Pre-battle VOX transmission narrative
- Mission overview, deployment map
- Scoring rules (from Wahapedia)
- Tactical Reserves setup
- Agenda selection (links only, no inline rules)
- Post-battle reporting checklist

### 2. Updates Data Files
Add entry to `data/battles.json`:
```json
{
  "007": {
    "title": "[Narrative Title]",
    "mission": "Strategic Strike",
    "type": "hulk",
    "file": "battle_007_[slug].html",
    "status": "pending",
    "combatants": {
      "attacker": "black-templars",
      "defender": "aeldari"
    }
  }
}
```

### 3. Updates Battles Index
Add to `battles/index.html` in Upcoming Battles section.

## Agenda Links
**IMPORTANT:** Look up URLs from `data/factions.json`. Never hardcode URLs.

```html
<a href="[faction.agendas_url]" target="_blank">[faction.name] Agendas</a>
```

## Narrative Guidelines
- Generate a thematic title (e.g., "Wrath Unchained", "The Black Tide Rises")
- Write VOX transmission that sets the scene
- Use gothic 40K tone
- Reference previous campaign events where relevant

## File Naming
`battle_[NNN]_[title_slug].html`
- Title slug: lowercase, hyphens for spaces
- Example: `battle_007_the_emperors_wrath.html`

## Checklist
- [ ] Validate input against schema
- [ ] Read faction data from `data/factions.json`
- [ ] Read alliance data from `data/alliances.json`
- [ ] Create HTML file with narrative title
- [ ] Add VOX transmission briefing
- [ ] Link agendas to Wahapedia (use data file URLs)
- [ ] Update `data/battles.json`
- [ ] Update `battles/index.html`
- [ ] Update `.claude/memory.md` with new pending battle
