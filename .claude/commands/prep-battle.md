# Prepare Battle

Sets up a battle page with pre-battle information once players have made their selections.

## Input Required
Provide the battle number and pre-battle selections. Example:
```
/prep-battle 002
```

Then provide:
- Attacker and Defender assignments
- Selected agendas for each player (2 per player, plus any bonus agendas)
- Any special setup notes

## What This Command Does

### 1. Sets Attacker/Defender Roles
Updates the combatants section to show which player is Attacker vs Defender:
```html
<div class="vox-combatant-role">Attacker</div>
<div class="vox-combatant-role">Defender</div>
```

### 2. Updates Agendas Section
- Removes "Option 1/Option 2" structure
- Changes header from "Agenda Selection" to "Selected Agendas"
- Removes unselected agendas, keeping only chosen ones
- Labels agendas as "Agenda 1", "Agenda 2", "Bonus Agenda" etc.
- Adds role indicator (Attacker/Defender) to each faction header
- Looks up and includes rules text for each selected agenda

### 3. Agenda Sources
When looking up agenda rules, check:
1. **Faction-specific agendas** - From faction Crusade rules on Wahapedia
2. **Nachmund Gauntlet agendas** - Generic mission agendas (Cut Off the Head, Drive Deep, Search Drop Site, Strike and Purge, Strategic Dominance)
3. **Defender agendas** - Activate Defence Perimeter, Repel the Foe, Defiant to the End
4. **Attacker agendas** - Symbolic Objectives, Prime Macro-Ordnance, Raze and Ruin

### 4. Keeps Pre-Battle Sections
Preserves sections needed before battle:
- Battle Briefing (pre-battle narrative)
- Mission Overview
- Deployment Zones
- Mission Rules
- Scoring
- Army Setup: Tactical Reserves
- Post-Battle Reporting

## Example Output Structure

```html
<!-- Agendas -->
<section>
    <h2>Selected Agendas</h2>
    <div class="card">
        <div class="collapsible-header" onclick="toggleCollapse(this)">
            <span>[Faction] — [Army Name] (Attacker)</span>
        </div>
        <div class="collapsible-content">
            <div class="agenda-grid">
                <div class="agenda-card">
                    <span class="role-tag [faction]">Agenda 1</span>
                    <h4>[Agenda Name]</h4>
                    <p>[Agenda rules text]</p>
                    <p class="xp-reward">[XP rewards]</p>
                </div>
                <div class="agenda-card">
                    <span class="role-tag nachmund">Agenda 2</span>
                    <h4>[Agenda Name]</h4>
                    <p>[Agenda rules text]</p>
                    <p class="xp-reward">[XP rewards]</p>
                </div>
            </div>
        </div>

        <div class="collapsible-header" onclick="toggleCollapse(this)">
            <span>[Faction] — [Army Name] (Defender)</span>
        </div>
        <div class="collapsible-content">
            [Similar structure for defender's agendas]
        </div>
    </div>
</section>
```

## Role Tag Classes
- `.role-tag.[faction]` - Faction-specific agenda (e.g., `.role-tag.death-guard`, `.role-tag.necrons`)
- `.role-tag.nachmund` - Generic Nachmund Gauntlet agenda
- `.role-tag.attacker` - Attacker-specific role agenda
- `.role-tag.defender` - Defender-specific role agenda

## Common Faction Agenda Sources

### Death Guard
- Spread the Sickness, Despoil, Grandfather's Blessing, Plague Harvest

### Necrons
- Awaken the Sleepers, The Reanimation Protocols, Ancient Vengeance, Code of Combat

### Dark Angels
- Dark Rumour, Encircle the Foe, The Deathwing Cometh, Mental Interrogation

### Leagues of Votann
- Prospecting, No Effort Wasted, Ancestral Revelation, Priority Acquisition

### Space Marines (Generic)
- Angels of Death, Know No Fear, Armoured Assault, Quest of Atonement

### Black Templars
- Fulfil Your Vows, Reconsecration, First-Hand Experience, Oaths of Crusade (bonus)

### Chaos Space Marines
- Champions of Chaos, The Long War, Blood for the Blood God, Dark Pacts
- Claim and Despoil, Blasphemous Ritual, Path to Glory
