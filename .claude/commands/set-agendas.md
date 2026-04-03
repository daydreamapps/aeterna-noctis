# Set Agendas

Sets the selected agendas for a faction in a battle page.

## Input Required
Provide the battle number, faction, and selected agendas. Example:
```
/set-agendas Battle 6, Black Templars: First Hand Experience, Fulfil Your Vows (Accept Any Challenge), Oaths of Crusade
```

Include:
- Battle number
- Faction name
- List of selected agendas (with any vow/oath selections noted in parentheses)

## What This Command Does

### 1. Looks Up Faction Data
Reads `data/factions.json` to get the correct Wahapedia URL for the faction's agendas page. This ensures links are always accurate.

### 2. Updates Battle Page
Finds the faction's agenda section in the battle HTML file and updates it with:
- The selected agendas as a bulleted list (bolded agenda names)
- Any vow/oath selections shown after an em-dash
- A note box with the correct Wahapedia link

### 3. Output Format
```html
<h4>[Faction Name] (Selected)</h4>
<ul>
    <li><strong>[Agenda 1]</strong></li>
    <li><strong>[Agenda 2]</strong> — [Vow/Oath Selection]</li>
    <li><strong>[Agenda 3]</strong></li>
</ul>
<div class="note" style="margin-top: 10px;">
    <a href="[faction_agendas_url]" target="_blank">[Faction Name] Agendas on Wahapedia</a>
</div>
```

## Faction URL Reference
URLs are stored in `data/factions.json`. Current factions:

| Faction | Slug | Notes |
|---------|------|-------|
| Black Templars | `black-templars` | Under Space Marines |
| Dark Angels | `dark-angels` | Under Space Marines |
| Death Guard | `death-guard` | Standalone |
| Chaos Space Marines | `chaos-space-marines` | Standalone |
| Necrons | `necrons` | Standalone |
| Aeldari | `aeldari` | Standalone |
| Leagues of Votann | `leagues-of-votann` | Standalone |

## Adding New Factions
If a faction is missing from `data/factions.json`:
1. Find the correct Wahapedia URL for their Agendas section
2. Add an entry to the factions object with:
   - `name`: Display name
   - `parent_faction`: Parent faction if applicable (e.g., "Space Marines" for chapters)
   - `alliance`: Campaign alliance (guardians/despoilers/marauders)
   - `agendas_url`: Direct link to the Agendas section
   - `crusade_rules_url`: Link to full Crusade Rules

## Example Usage

Input:
```
/set-agendas Battle 6, Chaos Space Marines: Honour the Dark Gods, Scour the Enemy Lines
```

Output updates `battles/battle_006_wrath_unchained.html`:
```html
<h4>Chaos Space Marines (Selected)</h4>
<ul>
    <li><strong>Honour the Dark Gods</strong></li>
    <li><strong>Scour the Enemy Lines</strong></li>
</ul>
<div class="note" style="margin-top: 10px;">
    <a href="https://wahapedia.ru/wh40k10ed/factions/chaos-space-marines/#Agendas" target="_blank">Chaos Space Marines Agendas on Wahapedia</a>
</div>
```

## Checklist
- [ ] Read faction data from `data/factions.json`
- [ ] Find the battle file
- [ ] Update the faction's agenda section
- [ ] Preserve any existing agendas for other factions
- [ ] Commit changes (do not push unless asked)
