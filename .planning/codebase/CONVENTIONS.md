# Coding Conventions

**Analysis Date:** 2026-04-21

## Overview

This is a static HTML/JSON campaign management system for a Warhammer 40,000 Crusade campaign. It uses no build tools, frameworks, or transpilation. Content is managed through Claude AI commands defined in `.claude/commands/`.

## Naming Patterns

**Files:**
- Battle pages: `battle_NNN_[title_slug].html` (e.g., `battle_013_no_ground_given.html`)
- Player pages: `[army_name_slug].html` (e.g., `the_bottomless_bucket.html`)
- Player markdown: `[army_name_slug].md` for detailed lore (e.g., `the_bottomless_bucket.md`)
- Template files: `_template.html` in each content directory
- Data files: lowercase with underscores (e.g., `data/battles.json`)
- Markdown docs: lowercase with underscores (e.g., `planets/sanctum_malakor.md`)

**Slugs:**
- Lowercase, hyphens for spaces: `black-templars`, `chaos-space-marines`
- Used consistently across all JSON and HTML files for cross-referencing
- Faction slugs defined in `data/factions.json` are canonical

**CSS Classes:**
- BEM-like naming: `.battle-entry`, `.battle-header`, `.battle-title`
- State modifiers: `.upcoming`, `.winner`, `.loser`
- Alliance classes: `.guardians`, `.despoilers`, `.marauders`

**JSON Keys:**
- snake_case for all keys: `date_m42`, `date_real`, `agendas_url`
- Consistent structure across battle entries

## Code Style

**HTML:**
- HTML5 doctype with `lang="en"`
- Inline CSS in `<style>` tags (no external stylesheets)
- CSS custom properties (variables) in `:root` for theming
- Self-contained pages (all styles embedded)

**CSS Variables (Theme):**
```css
:root {
    --bg-dark: #0a0a0a;
    --bg-card: #1a1612;
    --bg-highlight: #241a14;
    --accent: #8b0000;
    --accent-bright: #c41e3a;
    --accent-gold: #c9a227;
    --accent-bronze: #8b6914;
    --text: #e8dcc4;
    --text-muted: #9a8b7a;
    --bone: #d4c4a8;
    --border: #3d3225;
    --shadow-red: rgba(139, 0, 0, 0.4);
    --shadow-gold: rgba(201, 162, 39, 0.3);
}
```

**Fonts:**
- Primary: `'Crimson Text', Georgia, serif` (body text)
- Headings: `'Cinzel', serif` (uppercase, letter-spacing)
- Fonts loaded from Google Fonts

**JSON:**
- 2-space indentation
- Keys in double quotes
- Trailing commas NOT used (standard JSON)

**Markdown:**
- ATX-style headers (`# H1`, `## H2`)
- Tables for structured data
- Frontmatter-style metadata at top (`**Field:** Value`)

## Import Organization

**HTML Resources (order):**
1. Google Fonts preconnect
2. Google Fonts stylesheet
3. Inline `<style>` block
4. Page content
5. Inline `<script>` at end of body (if needed)

**No external dependencies:**
- No npm, no package.json
- No CSS framework imports
- All JavaScript is vanilla (minimal, inline)

## Error Handling

**JSON Data:**
- Null values for missing optional data (e.g., `"result": null` for pending battles)
- Empty arrays `[]` for empty collections
- Explicit `null` preferred over omitting keys

**Validation:**
- Manual validation via `/validate` command
- Checks JSON structure, cross-references, file existence
- Reports issues to user for manual resolution

## Documentation Patterns

**Markdown Files:**
- Section headers with `##`
- Status/metadata block at top
- Tables for tabular data
- Horizontal rules (`---`) to separate major sections

**HTML Comments:**
- Section markers for template guidance:
```html
<!-- ============================================
     SECTION NAME - Instructions
     ============================================ -->
```

**Template Notes:**
- `.template-note` class for editable guidance
- Commented-out example blocks showing filled state

## Content Guidelines

**VOX-INTERCEPT Narratives:**
- Under 2000 characters
- Gothic 40K tone
- Monospace font family (`'Courier New', monospace`)
- Green terminal aesthetic (`#4a9956`, `#a0d4a8`)
- Format:
```html
<div class="vox-transmission">
    <div class="vox-header">...</div>
    <div class="vox-body">...</div>
    <div class="vox-footer">...</div>
</div>
```

**Battle Titles:**
- Thematic, evocative: "No Ground Given", "The Black Tide Rises"
- No generic mission names as titles

**Faction References:**
- Use `data/factions.json` for URLs, never hardcode
- Reference by slug, lookup name and URLs dynamically

## File Structure Patterns

**Battle Pages Structure:**
1. Header (battle number, title, subtitle)
2. Victory Banner (completed) or VOX briefing (pending)
3. Matchup (attacker vs defender)
4. Mission Overview (pending) or Casualties table (completed)
5. Scoring rules
6. Agendas
7. Source link to Wahapedia
8. Footer

**Data File Updates:**
- Always read before write (check current state)
- Update `data/battles.json` when creating/finalizing battles
- Update `.claude/memory.md` for session continuity
- Update `battles/index.html` for battle listings

## Commit Message Conventions

**Prefixes (defined in CLAUDE.md):**
- `[battle]` - Battle page changes
- `[data]` - Campaign data updates
- `[skill]` - Skill/command changes
- `[fix]` - Bug fixes
- `[docs]` - Documentation

**Format:**
```
[prefix] Brief description

Co-Authored-By: Claude <noreply@anthropic.com>
```

## Common Idioms

**Linking to Wahapedia:**
```html
<a href="https://wahapedia.ru/wh40k10ed/..." target="_blank">
    Rule Name on Wahapedia
</a>
```

**Alliance-colored Elements:**
```html
<span class="result-tag victory">Guardians Victory</span>
<!-- Alliance colors via CSS classes -->
```

**Collapsible Sections (JavaScript):**
```javascript
function toggleCollapse(header) {
    header.classList.toggle('collapsed');
    const content = header.nextElementSibling;
    // ... toggle max-height
}
```

**M42 Timestamps:**
- Format: `NNN.M42` (e.g., `827.M42`)
- Tracked globally in `data/battles.json` as `m42_date`
- Increment by 1 per battle

---

*Convention analysis: 2026-04-21*
