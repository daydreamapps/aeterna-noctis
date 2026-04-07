# Set Agendas

Sets the selected agendas for a faction in a battle page.

## Model
Use **haiku** for this skill — straightforward data lookup and HTML update.

## Input Schema
```
/set-agendas <battle_number>, <faction>: <agenda1>, <agenda2>, [agenda3]
```

| Field | Type | Required | Validation |
|-------|------|----------|------------|
| battle_number | integer | yes | Must exist in `data/battles.json` |
| faction | faction slug | yes | Must exist in `data/factions.json` |
| agendas | string array | yes | 1-3 agenda names |

### Examples
```
/set-agendas 6, black-templars: First Hand Experience, Fulfil Your Vows (Accept Any Challenge)
/set-agendas 6, chaos-space-marines: Honour the Dark Gods, Scour the Enemy Lines
```

## Data Files to Read
1. `data/factions.json` — Get faction name and agendas_url
2. `data/battles.json` — Verify battle exists and faction is a combatant

## Validation Steps
1. Parse battle number and faction from input
2. Check battle exists in `data/battles.json`
3. Check faction is a combatant in that battle
4. Check faction exists in `data/factions.json`
5. Get the correct `agendas_url` from faction data

## What This Command Does

### 1. Updates Battle HTML
Find the faction's agenda section and replace with:

```html
<h4>[Faction Name] (Selected)</h4>
<ul>
    <li><strong>[Agenda 1]</strong></li>
    <li><strong>[Agenda 2]</strong> — [Vow/Oath if specified]</li>
</ul>
<div class="note" style="margin-top: 10px;">
    <a href="[agendas_url]" target="_blank">[Faction Name] Agendas on Wahapedia</a>
</div>
```

### 2. Updates Data Files
In `data/battles.json`, add agendas to battle entry:
```json
{
  "006": {
    "agendas": {
      "black-templars": [
        "First Hand Experience",
        "Fulfil Your Vows (Accept Any Challenge)"
      ]
    }
  }
}
```

### 3. Updates Memory
Add note to `.claude/memory.md` about agenda selection.

## Agenda Formatting
- Agenda names in **bold**
- Vow/oath selections after em-dash: `— Accept Any Challenge`
- Parenthetical notes preserved: `(No Matter the Odds)`

## Error Handling
| Error | Message |
|-------|---------|
| Battle not found | "Battle [N] not found in data/battles.json" |
| Faction not combatant | "[Faction] is not a combatant in Battle [N]" |
| Faction not found | "[Faction] not found in data/factions.json" |

## Checklist
- [ ] Validate battle number exists
- [ ] Validate faction is a combatant
- [ ] Look up faction URL from `data/factions.json`
- [ ] Update battle HTML with formatted agendas
- [ ] Update `data/battles.json` with agenda list
- [ ] Update `.claude/memory.md`
