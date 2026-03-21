This document is designed to be saved as **`CLAUDE.md`** in the root of your campaign folder. It provides **Claude Code** (and other AI agents) with the specific mechanical and narrative logic required to manage the *Aeterna Noctis* campaign autonomously.

---

# 🛸 PROJECT: AETERNA NOCTIS CRUSADE

**System:** Warhammer 40,000 (10th Edition)

**Campaign Engine:** Modified Nachmund Gauntlet

**Expansion Modules:** Administratum Conquest (Planetary Boons) & Maelstrom (Piracy/Scavenging)

## 📖 NARRATIVE SUMMARY

The Space Hulk *Aeterna Noctis* has entered the Malakor Sector, its massive warp-wake anchoring six worlds and dragging them toward the system's sun. Alliances (Guardians, Despoilers, Marauders) fight for control of the Hulk's internal systems while simultaneously raiding home worlds to cripple their rivals.

## 🛠️ MECHANICAL FRAMEWORK

### 1. Three-Tiered Battle Structure

* **The Hulk (Nachmund):** Main 1000-2000pt battles on the *Aeterna Noctis*. Tracks Campaign Victory Points (CVP) and Strategic Site control.
* **The Interdiction (Maelstrom):** 500-1000pt raids using *Lair of the Tyrant* rules. Used to reduce enemy planetary control and potentially steal RP.
* **The Breach (Phase 0):** Small-scale skirmishes (500pt) for Intel Points.

### 2. The Anchored Worlds (Planetary Control System)

Planetary control is tracked via Administratum's location system. Each alliance's control percentage determines how many boon uses they receive per phase.

#### Control Thresholds

| Control % | Uses Available |
|-----------|----------------|
| <30% | 0 uses |
| 30-59% | 1 use per phase |
| 60%+ | 2 uses per phase |

**How control works:**
- Tracked via Administratum's location system (battles assigned to planets affect control)
- Battle wins increase control for the victor's alliance
- Control only drops from losses (no passive decay)
- Campaign organizer can run rebalancing events when needed

#### Phase-Locked Benefits
- **Calculated at phase start:** Control % determines uses for the entire phase
- **No rollover:** Unused boons expire at phase end
- **Warmaster allocation:** Alliance leader decides which members get to use each boon
- **Alliance-wide pool:** Uses belong to the alliance, not individuals

#### Planet Boons (Per Use)

| Planet | Boon (per use) |
|--------|----------------|
| **Ocularis Prime** | Swap one Agenda after seeing opponent's choice |
| **Ferrum IX** | Remove one Battle Scar for 0 RP |
| **Veridian Reach** | +1 to Out of Action tests for one battle |
| **Sanctum Malakor** | +1 Starting CP for one battle |
| **Aethelgard** | Free "Fresh Recruits" Requisition |
| **Void-Spire 7** | +1 SAP from your next Hulk victory |

#### Raid Rewards
**Simple rule:** If a successful raid reduces the defender's control below a boon threshold (drops them from 60%+ to below 60%, or from 30%+ to below 30%), the raider gains **+1 RP**.

## 🗂️ DIRECTORY STRUCTURE & MANAGEMENT

Claude, maintain the campaign status using the following file patterns:

* `./alliances/[alliance_name].md`: Tracks Roster, CVP, and current Warmaster decisions.
* `./planets/[planet_name].md`: Planet information, boon description, and raid history.
* `./hulk/strategic_sites.md`: Current control level (Nachmund rules) for the Bridge, Spires, Relay, and Hangar.
* `./chronicles/[phase_name]/[battle_id].md`: Narrative battle reports and post-game updates.

## 🤖 INSTRUCTIONS FOR CLAUDE CODE

### Role: Campaign Master (CM)

Your primary goal is to process "Battle Reports" submitted by players and update the global campaign state.

**When a Battle Report is submitted, follow this logic:**

1. **Extract Data:** Identify the Phase, Alliance, Points, and Outcome.
2. **Update XP/RP:** Log casualties and veterancy for the specific units mentioned.
3. **Nachmund Updates:** If a Hulk battle, adjust **Battle Points** and **Strategic Asset Points (SAP)** for the respective Strategic Site.
4. **Maelstrom Updates:** If a Raid, adjust planetary control percentages in Administratum. If the raid drops the defender below a threshold (60% or 30%), award the raider +1 RP.
5. **Generate Narrative:** Write a "Vox-Intercept" style update for the Discord `#chronicles` channel. **The VOX-INTERCEPT section must be under 2000 characters.**

### Custom Prompt Templates (Slash Commands)

* `/process-battle [text]`: Analyzes raw player input and updates relevant `.md` files in `./alliances/` and `./planets/`.
* `/phase-summary`: Generates a narrative wrap-up for the current phase based on all battle files in `./chronicles/`.
* `/check-boons`: Lists all planets, current control percentages, and available boon uses per alliance.

## ⚠️ CAMPAIGN CONSTRAINTS

* **The Sun Timer:** As phases progress, apply "Environmental Hazards" (Solar Flare, Warp Instability) to all battle narrative generation.
* **The Betrayal Clause:** If a player chooses to "Abandon the Hulk" for a Planet Raid, auto-record a Loss for them in the Nachmund track.

---

**Next Step:** I recommend you create the folder structure listed above (`/alliances`, `/planets`, etc.) and save the **Sector Map** and **Planetary Descriptions** into a `RESOURCES.md` file in the same directory. Once you run `claude` in that folder, he will be ready to "ingest" your campaign and begin managing the war.