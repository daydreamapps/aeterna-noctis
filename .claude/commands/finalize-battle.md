# Finalize Battle Report

Converts a battle page from pre-battle planning mode to a completed battle record.

## Input Required
Provide the battle number and the battle notes/outcome data. Example:
```
/finalize-battle 003
```

Then provide the battle notes including:
- Final score
- Victor
- Casualties and battle scars
- Kill tallies
- Marked for Greatness selections
- Agenda completions
- XP earned
- Notable moments for narrative

## What This Command Does

### 1. Adds Battle Report Section
Inserts after the Battle Briefing:
- Victory banner with final score
- VOX-INTERCEPT narrative summary (under 2000 chars)
- Casualties & Battle Scars table
- Marked for Greatness summary
- Any special outcomes (new champions, etc.)

### 2. Removes Pre-Planning Sections
Removes sections only useful before the battle:
- Experience Summary (detailed XP breakdown)
- Mission Overview
- Deployment Zones
- Army Setup: Tactical Reserves
- Post-Battle Reporting checklist

### 3. Keeps Reference Sections
Preserves sections useful for historical record:
- Battle Briefing (narrative context)
- Mission Rules
- Scoring rules
- Selected Agendas
- Source Link

## Narrative Guidelines

The VOX-INTERCEPT narrative should:
- Be under 2000 characters (per CLAUDE.md rules)
- Highlight heroic moments and dramatic kills
- Reference units by their crusade names
- Include any battle scars earned and their narrative impact
- Mention Marked for Greatness units
- End with campaign implications

## Example Output Structure

```html
<!-- Battle Report -->
<section class="battle-report">
    <h2>Battle Report</h2>

    <div class="card result-card [alliance]-victory">
        <div class="result-header">
            <span class="result-label">[ALLIANCE] VICTORY</span>
            <span class="result-score">[Score] — [Score]</span>
        </div>
    </div>

    <div class="card">
        <h3>++ VOX-INTERCEPT: AFTER ACTION REPORT ++</h3>
        <div class="vox-body">
            [Narrative paragraphs...]
        </div>
    </div>

    <div class="card">
        <h3>Casualties & Battle Scars</h3>
        [Table of destroyed units and any scars earned]
    </div>

    <div class="card">
        <h3>Marked for Greatness</h3>
        [Units selected for MFG by each player]
    </div>
</section>
```

## CSS Classes for Victory Banners
- `.guardians-victory` - Green border (#4a9956)
- `.despoilers-victory` - Red border (#c44a40)
- `.marauders-victory` - Gold border (#c9a227)
