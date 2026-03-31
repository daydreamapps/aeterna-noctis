# Prepare Battle

Creates a new battle page with mission briefing and pre-battle information.

## Input Required
Provide the battle details. Example:
```
/prep-battle Battle 4, Front-line Warfare, Black Templars vs Aeldari, March 31 2026
```

Include:
- Battle number
- Mission name (from Nachmund Gauntlet)
- Combatants (Army Name, Faction, Alliance)
- Attacker and Defender assignments
- Real-world date

## What This Command Does

### 1. Creates Battle Page
Uses the template structure from existing battles to create a new HTML page with:
- Toolbar navigation
- Header with battle number, narrative title, and mission name
- Battle briefing (Vox Transmission narrative)
- Mission overview, deployment map, objective placement
- Scoring rules
- Army Setup (Tactical Reserves)
- Agenda Selection
- Post-Battle Reporting

### 2. Sets Combatants and Roles
Updates the vox-combatants section with Attacker/Defender roles:
```html
<div class="vox-combatant guardians">
    <div class="vox-combatant-name">Army Name</div>
    <div class="vox-combatant-alliance">Faction — Alliance</div>
    <div class="vox-combatant-role">Attacker</div>
</div>
```

### 3. Agenda Selection Structure
**IMPORTANT:** Do NOT include detailed agenda rules inline. Always link to Wahapedia instead to avoid inaccuracies.

Structure for each agenda section:
```html
<div class="collapsible-header" onclick="toggleCollapse(this)">
    <span>Attacker Agendas — [Army Name]</span>
</div>
<div class="collapsible-content">
    <p class="scoring-timing">Select from Nachmund Gauntlet Attacker agendas:</p>
    <div class="note" style="margin-top: 15px;">
        <a href="https://wahapedia.ru/wh40k10ed/the-rules/nachmund-gauntlet/#Agendas" target="_blank">View Nachmund Gauntlet Agendas on Wahapedia</a>
    </div>
</div>
```

### 4. Agenda Sources (Link Only)
Always link to the source - never write out agenda rules:

- **Nachmund Gauntlet Agendas:** `https://wahapedia.ru/wh40k10ed/the-rules/nachmund-gauntlet/#Agendas`
- **Faction Agendas:** `https://wahapedia.ru/wh40k10ed/factions/[faction-slug]/#Crusade-Rules`

Faction slugs:
- `black-templars`, `dark-angels`, `space-marines`
- `death-guard`, `chaos-space-marines`
- `necrons`, `aeldari`, `leagues-of-votann`

### 5. Updates Battles Index
Add the new battle to `battles/index.html` in the Upcoming Battles section.

### 6. File Naming Convention
Use narrative titles for battle files:
- `battle_001_breach_of_sector_theta_9.html`
- `battle_002_heralds_of_vengeance.html`
- `battle_004_the_black_tide_rises.html`

## Battle Briefing Narrative
Write a Vox Transmission style briefing that:
- Sets the scene for the conflict
- References previous campaign events where relevant
- Establishes stakes and motivation for both sides
- Uses 40K gothic tone and terminology
- Includes a thematic quote in the footer

## Checklist
- [ ] Create battle HTML file with narrative title
- [ ] Add vox transmission briefing
- [ ] Set correct mission rules and scoring from Wahapedia
- [ ] Link agendas to Wahapedia (do NOT write agenda rules inline)
- [ ] Update battles/index.html
- [ ] Commit and push changes
