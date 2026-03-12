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
* **The Interdiction (Maelstrom):** 500-1000pt raids using *Lair of the Tyrant* rules. Used to steal RP or disable an opponent's Home World Boon.
* **The Breach (Phase 0):** Small-scale skirmishes (500pt) for Intel Points.

### 2. The Anchored Worlds (Home World Boons)

* **Ocularis Prime:** Swap Agendas | **Penalty:** No Surgical Deep Strikes.
* **Ferrum IX:** 0 RP Scar removal | **Penalty:** Vehicles/Monsters gain no XP.
* **Veridian Reach:** +1 to Out of Action tests | **Penalty:** -1 to Advance/Charge.
* **Sanctum Malakor:** +1 Starting CP | **Penalty:** No Faction Stratagems.
* **Aethelgard:** Free "Fresh Recruits" | **Penalty:** No unit reinforcement.
* **Void-Spire 7:** +1 SAP per win | **Penalty:** No Artificer Relics.

## 🗂️ DIRECTORY STRUCTURE & MANAGEMENT

Claude, maintain the campaign status using the following file patterns:

* `./alliances/[alliance_name].md`: Tracks Roster, CVP, and current Warmaster decisions.
* `./planets/[planet_name].md`: Current owner, Boon status (Active/Disabled), and raid history.
* `./hulk/strategic_sites.md`: Current control level (Nachmund rules) for the Bridge, Spires, Relay, and Hangar.
* `./chronicles/[phase_name]/[battle_id].md`: Narrative battle reports and post-game updates.

## 🤖 INSTRUCTIONS FOR CLAUDE CODE

### Role: Campaign Master (CM)

Your primary goal is to process "Battle Reports" submitted by players and update the global campaign state.

**When a Battle Report is submitted, follow this logic:**

1. **Extract Data:** Identify the Phase, Alliance, Points, and Outcome.
2. **Update XP/RP:** Log casualties and veterancy for the specific units mentioned.
3. **Nachmund Updates:** If a Hulk battle, adjust **Battle Points** and **Strategic Asset Points (SAP)** for the respective Strategic Site.
4. **Maelstrom Updates:** If a Raid, toggle the `Boon Status` on the target planet file and log the RP theft.
5. **Generate Narrative:** Write a 1-paragraph "Vox-Intercept" style update for the Discord `#chronicles` channel.

### Custom Prompt Templates (Slash Commands)

* `/process-battle [text]`: Analyzes raw player input and updates relevant `.md` files in `./alliances/` and `./planets/`.
* `/phase-summary`: Generates a narrative wrap-up for the current phase based on all battle files in `./chronicles/`.
* `/check-boons`: Lists all planets and their current operational status.

## ⚠️ CAMPAIGN CONSTRAINTS

* **The Sun Timer:** As phases progress, apply "Environmental Hazards" (Solar Flare, Warp Instability) to all battle narrative generation.
* **The Betrayal Clause:** If a player chooses to "Abandon the Hulk" for a Planet Raid, auto-record a Loss for them in the Nachmund track.

---

**Next Step:** I recommend you create the folder structure listed above (`/alliances`, `/planets`, etc.) and save the **Sector Map** and **Planetary Descriptions** into a `RESOURCES.md` file in the same directory. Once you run `claude` in that folder, he will be ready to "ingest" your campaign and begin managing the war.