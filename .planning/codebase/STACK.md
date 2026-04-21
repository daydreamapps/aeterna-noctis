# Technology Stack

**Analysis Date:** 2026-04-21

## Languages

**Primary:**
- HTML5 (100%) - All content pages are static HTML files

**Secondary:**
- JavaScript (minimal) - Inline scripts for collapsible UI components
- JSON - Campaign data storage (`data/*.json`)
- Markdown - Documentation, rules reference, and chronicle logs

## Runtime

**Environment:**
- Static site - No server-side runtime required
- Browser-based - Modern web browsers (Chrome, Firefox, Safari, Edge)

**Package Manager:**
- None - No build tooling or package dependencies
- Files served directly from filesystem or static hosting

## Frameworks

**Core:**
- None - Vanilla HTML/CSS/JavaScript
- No frontend framework (React, Vue, etc.)

**Testing:**
- None - Manual validation via Claude commands

**Build/Dev:**
- None - No transpilation, bundling, or build process
- Files are authored and served as-is

## Key Dependencies

**External CDN Resources:**
- Google Fonts - Typography (`fonts.googleapis.com`)
  - Cinzel (500, 700 weights) - Headings, titles
  - Crimson Text (400, 600, italic) - Body text

**No npm/yarn dependencies** - Zero JavaScript libraries

## Configuration

**Environment:**
- No environment variables required
- No `.env` files present
- All configuration is inline or in JSON data files

**Build:**
- No build configuration files
- No `package.json`, `tsconfig.json`, etc.

## Data Storage

**Campaign Data (JSON files in `data/`):**
- `data/battles.json` - Battle index and metadata
- `data/alliances.json` - Alliance standings and rosters
- `data/planets.json` - Planetary control and boons
- `data/factions.json` - Faction URLs and references

**Documentation (Markdown files):**
- `data/rules.md` - Campaign mechanics reference
- `*.md` files in `planets/`, `alliances/`, `chronicles/`

## CSS Approach

**Methodology:**
- Inline `<style>` blocks in each HTML file
- CSS custom properties (CSS variables) for theming
- No external CSS files or preprocessors (SASS, etc.)

**Design System (CSS Variables):**
```css
--bg-dark: #0a0a0a
--bg-card: #1a1612
--accent: #8b0000
--accent-gold: #c9a227
--text: #e8dcc4
--border: #3d3225
```

**Features Used:**
- CSS Grid and Flexbox for layouts
- CSS gradients for backgrounds
- SVG noise texture overlay (inline data URI)
- Media queries for responsive design

## Platform Requirements

**Development:**
- Any text editor
- Web browser for preview
- Git for version control

**Production:**
- Static file hosting (GitHub Pages, Netlify, etc.)
- No server requirements
- No database requirements

## Claude Integration

**Custom Commands (`.claude/commands/`):**
- `prep-battle.md` - Create new battle pages
- `finalize-battle.md` - Complete battles with results
- `set-agendas.md` - Set faction agendas
- `validate.md` - Data integrity checks

**Memory File:**
- `.claude/memory.md` - Session continuity notes

## File Statistics

**Total Content:**
- ~26,500 lines across all content files
- ~30 HTML files (battles, players, rules, index)
- ~25 Markdown files (planets, alliances, chronicles)
- 4 JSON data files

---

*Stack analysis: 2026-04-21*
