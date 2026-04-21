# Testing Patterns

**Analysis Date:** 2026-04-21

## Overview

This project has no automated testing framework. Validation is performed manually through a Claude AI command (`/validate`). The codebase is a static content site with no runtime code requiring unit tests.

## Test Framework

**Runner:** None

**Assertion Library:** None

**Run Commands:**
```bash
# No automated tests - use validation command instead
/validate        # Run all validation checks
/validate battles    # Validate battle files only
/validate factions   # Validate faction references only
/validate all        # Full validation suite
```

## Validation Approach

**Location:** `.claude/commands/validate.md`

**Purpose:** Integrity checking for campaign data files and cross-references.

### Validation Checks Performed

**1. Data File Integrity:**
- `data/factions.json` - Valid JSON, required fields present
- `data/alliances.json` - Valid JSON, faction references exist
- `data/planets.json` - Valid JSON, control totals valid (0-100)
- `data/battles.json` - Valid JSON, faction references exist

**2. Battle File Checks:**
For each battle in `data/battles.json`:
- HTML file exists at specified path
- Title in HTML matches JSON
- Combatants in HTML match JSON
- Status (pending/completed) is consistent

**3. Faction URL Checks:**
- `agendas_url` is valid URL format
- URL domain is wahapedia.ru

**4. Cross-Reference Checks:**
- All faction slugs in `alliances.json` exist in `factions.json`
- All faction slugs in `battles.json` exist in `factions.json`
- All planet slugs in alliances match `planets.json`
- Battle numbers are sequential with no gaps

**5. Alliance Consistency:**
- Each faction appears in exactly one alliance
- CVP/SAP values are non-negative integers

## Validation Output Format

```
=== Campaign Validation Report ===

Data Files:
  [check] factions.json - 7 factions loaded
  [check] alliances.json - 3 alliances, 7 members
  [check] planets.json - 6 planets loaded
  [check] battles.json - 15 battles indexed

Battle Files:
  [check] battle_001 - file exists, metadata valid
  [check] battle_002 - file exists, metadata valid
  ...

Faction URLs:
  [check] black-templars - URL valid
  [check] chaos-space-marines - URL valid
  ...

Cross-References:
  [check] All faction references valid
  [check] All planet references valid

Issues Found: 0
```

## Error Handling in Validation

**On Failure:**
- Report which check failed
- Show expected vs actual values
- Suggest fix approach
- Do NOT automatically fix issues

**Example Error:**
```
Battle Files:
  [x] battle_012 - file missing: expected 'battles/battle_012_echoes_of_the_silenced.html'

Fix: Create battle page using /prep-battle or update battles.json
```

## Manual Testing Checklist

**Before Battle Page Creation:**
- [ ] Battle number not already used
- [ ] Combatant factions exist in `data/factions.json`
- [ ] Mission name is valid Nachmund mission

**After Battle Finalization:**
- [ ] `data/battles.json` status updated to "completed"
- [ ] `battles/index.html` entry moved to Completed section
- [ ] VOX-INTERCEPT under 2000 characters
- [ ] M42 date incremented
- [ ] `.claude/memory.md` updated

**Cross-Reference Verification:**
- [ ] Faction URLs pulled from `data/factions.json`, not hardcoded
- [ ] Alliance membership consistent across files
- [ ] Planet control percentages sum correctly

## Test Coverage Gaps

**No Automated Checks For:**
- HTML validity (no linting)
- CSS consistency across pages
- Accessibility compliance
- Broken internal links
- Image file existence
- Character count limits (VOX-INTERCEPT)

**Recommended Manual Reviews:**
- Visual inspection of new battle pages
- Link testing in browser
- Mobile responsiveness check

## Model Selection for Validation

**From `.claude/commands/validate.md`:**
- Use **haiku** model for validation tasks
- It's a straightforward validation task, no narrative generation needed

## Data Integrity Patterns

**JSON Schema (Informal):**

Battle entry:
```json
{
  "NNN": {
    "title": "string (required)",
    "mission": "string (required)",
    "type": "string (hulk|raid|skirmish)",
    "file": "string (required, path)",
    "status": "string (pending|in_progress|completed)",
    "date_m42": "number|null",
    "date_real": "string|null (YYYY-MM-DD)",
    "combatants": {
      "attacker": "string|null (faction slug)",
      "defender": "string|null (faction slug)"
    },
    "agendas": "object|null",
    "result": "object|null"
  }
}
```

Faction entry:
```json
{
  "slug": {
    "name": "string (required)",
    "parent_faction": "string|null",
    "alliance": "string (required, alliance slug)",
    "agendas_url": "string (required, URL)",
    "crusade_rules_url": "string (required, URL)"
  }
}
```

## Recommended Future Testing

**If automated testing is added:**
1. JSON schema validation (ajv or similar)
2. Link checker for HTML files
3. File existence verification
4. Character count validation for VOX content
5. Slug consistency checker

**CI/CD Integration:**
- None currently
- Could add GitHub Actions for JSON validation
- Could add htmlhint for HTML linting

---

*Testing analysis: 2026-04-21*
