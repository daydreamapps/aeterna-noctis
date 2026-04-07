# Validate Campaign Data

Performs integrity checks on campaign data files and battle pages.

## Usage
```
/validate
/validate battles
/validate factions
/validate all
```

## Model
Use **haiku** for this skill — it's a straightforward validation task.

## Checks Performed

### 1. Data File Integrity
- [ ] `data/factions.json` — Valid JSON, all required fields present
- [ ] `data/alliances.json` — Valid JSON, faction references exist
- [ ] `data/planets.json` — Valid JSON, control totals valid (0-100)
- [ ] `data/battles.json` — Valid JSON, faction references exist

### 2. Battle File Checks
For each battle in `data/battles.json`:
- [ ] HTML file exists at specified path
- [ ] Title in HTML matches JSON
- [ ] Combatants in HTML match JSON
- [ ] Status (pending/completed) is consistent

### 3. Faction URL Checks
For each faction in `data/factions.json`:
- [ ] `agendas_url` is a valid URL format
- [ ] URL domain is wahapedia.ru

### 4. Cross-Reference Checks
- [ ] All faction slugs in alliances.json exist in factions.json
- [ ] All faction slugs in battles.json exist in factions.json
- [ ] All planet slugs in alliances match planets.json
- [ ] Battle numbers are sequential with no gaps

### 5. Alliance Consistency
- [ ] Each faction appears in exactly one alliance
- [ ] CVP/SAP values are non-negative integers

## Output Format

```
=== Campaign Validation Report ===

Data Files:
  ✓ factions.json - 7 factions loaded
  ✓ alliances.json - 3 alliances, 7 members
  ✓ planets.json - 6 planets loaded
  ✓ battles.json - 6 battles indexed

Battle Files:
  ✓ battle_001 - file exists, metadata valid
  ✓ battle_002 - file exists, metadata valid
  ...

Faction URLs:
  ✓ black-templars - URL valid
  ✓ chaos-space-marines - URL valid
  ...

Cross-References:
  ✓ All faction references valid
  ✓ All planet references valid

Issues Found: 0
```

## On Failure
If validation fails, output:
- Which check failed
- Expected vs actual values
- Suggested fix

Do NOT automatically fix issues — report them for user review.
