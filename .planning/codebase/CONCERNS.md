# Codebase Concerns

**Analysis Date:** 2026-04-21

## Summary

The Aeterna Noctis campaign codebase is a static HTML/JSON content site with Claude Code commands for automation. The primary concerns center on data consistency, CSS duplication, and stale documentation rather than traditional code issues.

---

## Critical

*No critical issues identified.*

---

## High Priority

### Data Consistency: Memory File Out of Sync

- **Issue:** `.claude/memory.md` contains stale state information
- **Files:** `.claude/memory.md`
- **Details:**
  - Memory shows `M42 Date: 823` but `data/battles.json` shows `m42_date: 823` (and battles up to 829.M42 exist)
  - "Last Updated: 2026-04-07" is nearly two weeks old
  - "Pending Battles" list references battles 009-011 as scheduled, but these are now completed
  - Outstanding items reference Battle 007 and 008 results that are outdated
- **Impact:** Claude Code sessions may start with incorrect context, leading to confusion or duplicate work
- **Fix approach:** Update `/finalize-battle` command to reliably update memory.md, or add memory sync to `/validate` command

### Data Consistency: Battle JSON Incomplete Metadata

- **Issue:** Early battles (001-002) missing structured data
- **Files:** `data/battles.json`
- **Details:**
  - Battles 001-002 have `date_m42: null`, `date_real: null`, `combatants.attacker: null`, `combatants.defender: null`, and `result: null`
  - Battle 003 has `result.score` with `null` values
  - This creates inconsistency when parsing battle history programmatically
- **Impact:** Any tool iterating battles must handle null checks; reports may be incomplete
- **Fix approach:** Backfill historical battle data from `battles/index.html` which has this information

### CSS Duplication Across HTML Files

- **Issue:** Full CSS styling is duplicated in every HTML file
- **Files:** All files in `battles/*.html`, `players/*.html`, `index.html`, `rules/index.html`
- **Details:**
  - Each HTML file contains 400-600 lines of identical CSS in `<style>` blocks
  - CSS variables defined identically (`:root` block) in every file
  - Same responsive media queries duplicated everywhere
  - Total CSS duplication: ~15,000+ lines across all files
- **Impact:**
  - Any style change requires editing 20+ files
  - Risk of style drift between files
  - Larger file sizes affect page load
- **Fix approach:** Extract to shared `styles.css` file or use a CSS preprocessor. Would require updating all HTML files to reference external stylesheet.

---

## Medium Priority

### Army Name Inconsistency

- **Issue:** Army names inconsistently referenced
- **Files:** `data/alliances.json`, `data/battles.json`, `battles/index.html`
- **Details:**
  - "Rattle Me Bones" in `alliances.json` vs "Tekhsarun Awakening" in `battles/index.html`
  - "The Crusade of Binding Chains" vs "Crusade of Binding Chains" (with/without "The")
  - Battle 003 in index shows "Chaos Space Marines" as army name but should be "The Killing Stone"
- **Impact:** Narrative inconsistency; confusing for players and readers
- **Fix approach:** Canonicalize army names in `alliances.json` and use those as source of truth

### Player Roster Duplication

- **Issue:** Player rosters exist in both HTML and Markdown formats
- **Files:** `players/crusade_of_binding_chains.html`, `players/crusade_of_binding_chains.md`, `players/the_bottomless_bucket.html`, `players/the_bottomless_bucket.md`
- **Details:**
  - Two files maintaining same roster information
  - MD files have rich narrative content; HTML files have structured display
  - Easy for them to drift out of sync
- **Impact:** Double maintenance burden; potential for conflicting information
- **Fix approach:** Either generate HTML from MD (using a build step) or pick one format as canonical

### Missing Toolbar Navigation on Battle Pages

- **Issue:** Individual battle pages lack the toolbar navigation present on index pages
- **Files:** `battles/battle_*.html` (all individual battle pages)
- **Details:**
  - `index.html`, `battles/index.html`, `players/index.html` have toolbar with Battles/Rules/Rosters links
  - Individual battle pages only have "Back to Campaign" or "Back to Battle Logs" link
  - Users cannot easily navigate between sections from battle detail pages
- **Impact:** Poor navigation UX; users must go back to index to change sections
- **Fix approach:** Add toolbar to battle page template and existing pages

### Alliance Data Not Updated

- **Issue:** Alliance CVP/SAP values remain at 0
- **Files:** `data/alliances.json`
- **Details:**
  - All alliances show `cvp: 0`, `sap: 0`
  - 14 battles completed but no cumulative tracking
  - `warmaster: null` for all alliances
  - `requisition_pool: 0` for all alliances
- **Impact:** Campaign progress not reflected in structured data; leaderboard/tracking features cannot work
- **Fix approach:** Add CVP/SAP update logic to `/finalize-battle` command

---

## Low Priority

### Date Format Inconsistency

- **Issue:** Multiple date formats used
- **Files:** `battles/index.html`, `data/battles.json`
- **Details:**
  - `data/battles.json`: "2026-03-25" (ISO format)
  - `battles/index.html`: "March 25, 2026", "19 Apr 2026", "April 7, 2026"
  - Some entries use "Apr" abbreviation, others spell out "April"
- **Impact:** Visual inconsistency; harder to parse for any automated tools
- **Fix approach:** Standardize on one format (ISO in data, localized in HTML)

### Missing Home World Data

- **Issue:** Most factions have `home_world: null` in alliance data
- **Files:** `data/alliances.json`
- **Details:**
  - Only Black Templars has `home_world: "aethelgard"` defined
  - Other 7 factions have `home_world: null`
- **Impact:** Cannot implement home world bonuses or narrative features
- **Fix approach:** Assign home worlds based on campaign narrative

### Player Names Missing

- **Issue:** Most alliance members have `player: null`
- **Files:** `data/alliances.json`
- **Details:**
  - Only 2 of 8 factions have player names assigned (Samuel, Kris)
  - Others show `player: null`
- **Impact:** Cannot attribute results to players or generate player statistics
- **Fix approach:** Fill in player names for active factions

### Battle Index Order

- **Issue:** Completed battles not sorted chronologically
- **Files:** `battles/index.html`
- **Details:**
  - Battle 009 (M42 825) appears before Battle 010 (M42 824) in the completed list
  - Battles should be sorted by M42 date, descending
- **Impact:** Confusing timeline presentation
- **Fix approach:** Re-order completed battles by M42 date

### Unused Chronicles Directory

- **Issue:** Chronicles directory appears abandoned
- **Files:** `chronicles/phase_0/`
- **Details:**
  - Contains `battle_001_aethelgard.md`, `battle_002_sanctum_malakor.md`, `week_001_summary.md`
  - References "Phase 0" battles not in main tracking system
  - No chronicles for Phase 1 battles
- **Impact:** Orphaned content; unclear relationship to main battle tracking
- **Fix approach:** Either resume chronicles feature or archive/remove directory

---

## Tech Debt Indicators

### Template Outdated

- **Issue:** Battle template may be out of sync with recent battle pages
- **Files:** `battles/_template.html`
- **Details:**
  - Template is 840 lines; recent battle pages vary from 490-1091 lines
  - Some battle pages have features not in template (e.g., victory banners, agenda results)
- **Impact:** New battles created from template require significant manual updates
- **Fix approach:** Update template with all sections from finalized battles

### No Build System

- **Issue:** Pure static HTML with no build/preprocessing
- **Files:** All HTML files
- **Details:**
  - No Tailwind, SCSS, or CSS-in-JS
  - No HTML templating (Handlebars, Nunjucks, etc.)
  - No static site generator
- **Impact:** CSS duplication problem; no way to share components
- **Fix approach:** Consider 11ty or Hugo for template inheritance while keeping static output

### Manual File Updates

- **Issue:** Many updates require editing multiple files manually
- **Files:** Multiple
- **Details:**
  - Adding a battle: `battles/[name].html`, `battles/index.html`, `data/battles.json`, `.claude/memory.md`
  - Completing a battle: Same files plus `data/alliances.json`, player roster files
  - No automated validation that all files are updated
- **Impact:** High risk of files drifting out of sync
- **Fix approach:** Enhance `/validate` command to check cross-file consistency

---

## Test Coverage Gaps

### No Automated Validation

- **Issue:** `/validate` command exists but is not automated
- **Files:** `.claude/commands/validate.md`
- **Details:**
  - Validation is manual (user must run `/validate`)
  - No CI/CD pipeline to catch data consistency issues
  - No JSON schema validation for data files
- **Impact:** Data corruption can accumulate undetected
- **Fix approach:** Add pre-commit hook or GitHub Action to run validation

### No HTML Validation

- **Issue:** HTML files are not validated
- **Files:** All `.html` files
- **Details:**
  - No W3C validation in place
  - Some files may have unclosed tags or invalid attributes
- **Impact:** Rendering issues in certain browsers
- **Fix approach:** Add HTML linting to validation workflow

---

## Security Considerations

### External Links to Wahapedia

- **Issue:** Heavy reliance on external wahapedia.ru domain
- **Files:** All battle HTML files, `data/factions.json`
- **Details:**
  - Deployment maps loaded from `wahapedia.ru/wh40k10ed/img/maps/`
  - Crusade rules linked to wahapedia.ru
  - If wahapedia.ru goes offline, images break and rules links dead
- **Impact:** Campaign pages degrade if external service unavailable
- **Fix approach:** Consider hosting critical images locally; document dependency

### No Input Sanitization in Commands

- **Issue:** Claude commands accept user input without validation
- **Files:** `.claude/commands/*.md`
- **Details:**
  - Battle titles, army names, etc. accepted as-is
  - Potential for injection if rendered without escaping (though static HTML mitigates this)
- **Impact:** Low risk for static site, but could introduce issues if build system added
- **Fix approach:** Add input validation patterns to command documentation

---

*Concerns audit: 2026-04-21*
