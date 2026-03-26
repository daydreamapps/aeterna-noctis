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
Replaces the pre-battle briefing with:
- Victory banner with final score
- VOX-INTERCEPT after action report (full vox-transmission format with header)
- Casualties & Battle Scars table
- Marked for Greatness summary
- Any special outcomes (new champions, etc.)

### 2. Removes Pre-Planning Sections
Removes sections only useful before the battle:
- **Battle Briefing** (pre-battle narrative - replaced by after action report)
- Experience Summary (detailed XP breakdown)
- Mission Overview
- Deployment Zones
- Army Setup: Tactical Reserves
- Post-Battle Reporting checklist

### 3. Keeps Reference Sections
Preserves sections useful for historical record:
- Mission Rules
- Scoring rules
- Selected Agendas
- Source Link

## VOX-INTERCEPT Format

The after action report uses the full `vox-transmission` format with header:

```html
<div class="vox-transmission">
    <div class="vox-header">
        <span class="vox-priority">++ Priority: [Amber/Crimson] ++</span>
        <span>++ Source: [Location] Sensorium ++</span>
        <span>++ Timestamp: [Date].M42 ++</span>
    </div>
    <div class="vox-body">
        [Narrative paragraphs...]
    </div>
    <div class="vox-footer">
        <em>"[Thought for the day quote]"</em><br>
        — Thought for the Day
    </div>
</div>
```

### Narrative Guidelines
- Under 2000 characters (per CLAUDE.md rules)
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

    <div class="vox-transmission">
        <div class="vox-header">
            <span class="vox-priority">++ Priority: Amber ++</span>
            <span>++ Source: Sector [X] Sensorium ++</span>
            <span>++ Timestamp: 814.M42 ++</span>
        </div>
        <div class="vox-body">
            <p>[Opening - battle outcome and key casualties]</p>
            <p>[Heroic moments - units that distinguished themselves]</p>
            <p>[Closing - campaign implications]</p>
        </div>
        <div class="vox-footer">
            <em>"[Thematic quote]"</em><br>
            — Thought for the Day
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
