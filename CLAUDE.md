# CLAUDE.md — Project Notes for BLDGTYP Homepage

## Project Structure

```
homepage/
├── CLAUDE.md
└── site/
    ├── index.html          ← main homepage (single-file, inline CSS + JS)
    └── assets/
        ├── hero-drafting.png
        ├── detail-foundation.png
        ├── detail-wall-section.png
        ├── energy-model.png
        ├── thermal-bridge.png
        └── certification.png
```

## Brand Design System

The homepage consumes shared CSS from the BLDGTYP branding repo:

- **Repo:** https://github.com/bldgtyp/branding
- **Live reference:** https://bldgtyp.github.io/branding/
- **Tokens CSS:** `https://bldgtyp.github.io/branding/tokens/tokens.css` — design tokens (colors, fonts, radii, transitions) for light/dark themes
- **Components CSS:** `https://bldgtyp.github.io/branding/tokens/components.css` — reusable component classes

### Key component classes from the design system

- `.btn-primary`, `.btn-ghost` — buttons
- `.icon-btn`, `.nav-icons` — icon buttons and container
- `.service-card`, `.service-card__title`, `.service-card__subtitle`, `.service-card__body` — card components
- `.section-label` — section heading labels
- `.theme-toggle` — dark/light mode toggle button
- `.type-hero`, `.type-heading`, `.type-body`, `.type-body-lg`, `.type-mono-nav`, `.type-mono-label` — typography helpers
- `.graph-paper`, `.graph-paper--standard`, `.graph-paper--medium`, `.graph-paper--fine` — background patterns (note: `.graph-paper` base class required for positioning context)
- `.stats-bar`, `.stats-bar__number`, `.stats-bar__label` — stats display

### Important conventions

- Page-specific CSS stays inline in index.html `<style>` block — only layout, positioning, and page-unique styles
- All typography tokens, colors, and component styles should come from the design system
- `data-theme="light"` / `data-theme="dark"` on `<html>` controls theming
- Font import (Google Fonts for Outfit + JetBrains Mono) is kept in each consuming page's `<head>`

## Dev Server & Browser Preview

**IMPORTANT:** The Cowork sandbox VM cannot serve pages to the user's Chrome browser. `localhost` in Chrome refers to the user's Mac, not the VM.

- Running `python3 -m http.server` inside the VM works for `curl` verification but Chrome on the host machine **cannot reach it**
- Do NOT waste time trying to preview via localhost — it will always show "This site can't be reached"
- `mcp__Control_Chrome__open_url` can open `file:///Users/em/homepage/site/index.html` in Chrome, but the tab opens outside the MCP tab group so Claude in Chrome **cannot screenshot it**
- The Claude in Chrome screenshot/interaction tools only work on tabs inside the MCP tab group, and `file://` tabs opened via Control Chrome are not added to that group
- **Best workflow for visual feedback:** ask the user to open/refresh the page in Chrome and share a screenshot
- Use `curl` from within the VM to verify HTML structure, server responses, or CSS correctness if needed for testing
