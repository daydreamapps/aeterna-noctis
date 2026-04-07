# Finalize Battle Report

Converts a battle page from pre-battle planning to completed battle record.

## Model
Use **sonnet** for this skill — requires narrative generation and complex HTML updates.

## Input Schema
```
/finalize-battle <battle_number>
```

Then provide battle notes including:
- Final score (e.g., "10-5")
- Victor alliance
- Casualties and battle scars
- Kill tallies per unit
- Marked for Greatness selections
- Agenda completions
- Notable moments for narrative

| Field | Type | Required | Validation |
|-------|------|----------|------------|
| battle_number | integer | yes | Must exist in `data/battles.json` with status "pending" |

## Data Files to Update
1. `data/battles.json` — Set status to "completed", add result
2. `data/alliances.json` — Update CVP/SAP if applicable
3. `.claude/memory.md` — Update pending battles list

## What This Command Does

### 1. Adds Battle Report Section
Replace pre-battle content with:
- Victory banner with final score
- VOX-INTERCEPT after-action report (under 2000 chars)
- Casualties & Battle Scars table
- Marked for Greatness summary

### 2. Removes Pre-Planning Sections
Remove:
- Pre-battle VOX briefing
- Mission Overview
- Deployment Zones
- Army Setup: Tactical Reserves
- Post-Battle Reporting checklist

### 3. Keeps Reference Sections
Preserve:
- Mission Rules
- Scoring rules
- Selected Agendas
- Source Link

### 4. Updates Battles Index
In `battles/index.html`:
- Move from "Upcoming Battles" to "Completed Battles"
- Add M42 timestamp and real-world date
- Add final score and victor
- Apply winner/loser CSS classes

### 5. Updates Data Files
In `data/battles.json`:
```json
{
  "006": {
    "status": "completed",
    "date_m42": 820,
    "date_real": "2026-04-15",
    "result": {
      "victor": "guardians",
      "score": { "attacker": 10, "defender": 5 }
    }
  }
}
```

## M42 Timestamp Rules
1. Read `data/battles.json` to get current `m42_date`
2. Increment by 1 for this battle
3. Update the global `m42_date` in the JSON

## VOX-INTERCEPT Format
```html
<div class="vox-transmission">
    <div class="vox-header">
        <span class="vox-priority">++ Priority: Amber ++</span>
        <span>++ Source: Aeterna Noctis Sensorium ++</span>
        <span>++ Timestamp: [M42_DATE].M42 ++</span>
    </div>
    <div class="vox-body">
        <p>[Opening - battle outcome]</p>
        <p>[Key moments - heroic actions]</p>
        <p>[Closing - campaign implications]</p>
    </div>
    <div class="vox-footer">
        <em>"[Thematic quote]"</em><br>
        — Thought for the Day
    </div>
</div>
```

## Narrative Guidelines
- Under 2000 characters total
- Reference units by crusade names
- Highlight Marked for Greatness units
- Include battle scars narratively
- End with campaign implications

## CSS Classes
Victory banners:
- `.guardians-victory` — Green (#4a9956)
- `.despoilers-victory` — Purple (#9b4d96)
- `.marauders-victory` — Gold (#c9a227)

Combatants:
- Add `winner` class to victor
- Add `loser` class to defeated

## Checklist
- [ ] Validate battle exists and is pending
- [ ] Gather all battle notes from user
- [ ] Calculate final scores
- [ ] Generate VOX-INTERCEPT narrative (under 2000 chars)
- [ ] Update battle HTML file
- [ ] Update `battles/index.html`
- [ ] Update `data/battles.json`
- [ ] Update `.claude/memory.md`
- [ ] Increment M42 date
