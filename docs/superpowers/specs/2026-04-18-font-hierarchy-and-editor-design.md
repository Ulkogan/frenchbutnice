# Font Hierarchy & Click-to-Inspect Editor

**Date:** 2026-04-18
**Target file:** `round-4/31-the-index.html`

## Overview

Two changes to the personal site (version 4 / "The Index"):

1. **New font hierarchy** using the OffBit family for display, labels, and metadata — keeping Astro Mono for body copy.
2. **Click-to-inspect editor overlay** for visually tweaking CSS properties on any element, with live preview and CSS export.

## Font Hierarchy

Four tiers, each with a distinct role and typeface:

| Tier | Font | CSS Variable | Applied To |
|------|------|-------------|------------|
| Display | OffBit Regular/Bold | `--font-display` | `.identity-name`, `.card-name`, `.topic-title` |
| Labels / UI | OffBit 101 Regular/Bold | `--font-label` | `.section-label`, `.module-block` |
| Numbers / Meta | OffBit Dot Regular/Bold | `--font-meta` | `.index-num`, `.topic-num`, `.status-label`, `.card-year`, `.card-domain`, `.footer-machine`, `.identity-number` |
| Body | Astro Mono Regular/Bold | `--font-primary` (existing) | `.identity-sub`, `.bio-text`, `.topic-desc`, `.index-title`, footer links, `.card-tagline` |

### Font-face declarations

Eight new `@font-face` rules (OffBit × 3 sub-families × 2 weights). Paths relative to the HTML file:

```
../references/fonts/T17139-OT/OffBit-Regular.otf
../references/fonts/T17139-OT/OffBit-Bold.otf
../references/fonts/T17139-OT/OffBit-101.otf
../references/fonts/T17139-OT/OffBit-101-Bold.otf
../references/fonts/T17139-OT/OffBit-Dot.otf
../references/fonts/T17139-OT/OffBit-Dot-Bold.otf
```

### CSS variables (added to `:root`)

```css
--font-display: 'OffBit', 'Astro Mono', monospace;
--font-label: 'OffBit 101', 'Astro Mono', monospace;
--font-meta: 'OffBit Dot', 'Astro Mono', monospace;
```

`--font-primary` and `--font-secondary` remain unchanged.

## Click-to-Inspect Editor

A dev-only overlay for tweaking CSS properties visually. Not part of the public site experience — a design tool for the author.

### Activation

- **Keyboard:** Press `E` to toggle editor mode on/off.
- **Button:** Floating toggle button in the bottom-right corner. Shows `E` glyph. Blue accent border, dark background. Visible at all times (subtle when editor is off, highlighted when on).

### Editor Mode Behavior

When editor mode is **on**:

1. **Hover:** Any element under the cursor gets a blue outline (2px dashed, accent color). Cursor changes to crosshair.
2. **Click:** The clicked element becomes "selected." A floating editor panel appears anchored near the element (positioned to avoid going off-screen).
3. **Click elsewhere:** Selects new element, panel moves to it.
4. **Press `Escape` or `E`:** Deselects and exits editor mode.

When editor mode is **off**:

- All editor UI is hidden.
- Page behaves normally (all original click/hover interactions work).
- Any live CSS changes made during the session persist until page reload.

### Editor Panel

A floating panel (~320px wide) with a dark background (`#151820`), border, and box shadow. Sections:

#### Header
- Element tag name (e.g., `H1`) in accent color
- Class name (e.g., `.identity-name`) in dim text
- Close button (`×`)

#### Typography Section
- `font-size` — number input + unit dropdown (px/rem/em)
- `font-weight` — number input (100–900)
- `letter-spacing` — number input + unit
- `line-height` — number input

#### Color Section
- `color` — color swatch (click to open native color picker) + hex input
- `background` — color swatch + hex input

#### Spacing Section
- Visual box model diagram (nested boxes for margin and padding)
- Click any value in the diagram to edit it inline
- Margin values in amber, padding values in accent blue

#### Border Section
- `border-width` — number input + px
- `border-color` — color swatch + hex input

#### Actions
- **Reset** — reverts all changes on the selected element (removes inline styles added by the editor)
- **Copy CSS** — copies all modifications across all edited elements as a CSS block to clipboard. Format: selector + changed properties only.

### Implementation Details

- All changes are applied as inline styles on the element.
- The editor tracks which elements have been modified and what their original computed values were.
- "Copy CSS" iterates over all modified elements, diffs current inline styles against originals, and outputs class-based CSS rules.
- The editor script is a self-contained block at the end of `<body>`, after the existing page script. It injects its own styles scoped with a `[data-editor]` attribute prefix to avoid conflicts.
- No external dependencies.
- Editor state does not persist across page reloads.

### Edge Cases

- **Panel positioning:** Panel anchors to the right of the selected element by default. If it would overflow the viewport, flip to the left. If vertical overflow, shift up.
- **Arrival overlay:** Editor should not activate while the arrival overlay is visible. Wait until workspace is shown.
- **SVG elements (graph):** Click-to-inspect works on SVG group elements (`<g>`) but not individual SVG primitives. Panel shows limited properties (fill, stroke, opacity).
- **Nested elements:** Clicking selects the deepest element under the cursor (same as browser default). No special parent-climbing logic needed.

## Files Changed

Only one file is modified: `round-4/31-the-index.html`

Changes within that file:
1. Add 8 `@font-face` declarations for OffBit family (after existing Astro Mono declarations)
2. Add 3 CSS variables to `:root`
3. Update `font-family` on ~10 selectors to use new variables
4. Add editor CSS (scoped with `[data-editor]` prefix, appended after existing styles)
5. Add editor JavaScript (new `<script>` block after existing script)

## Out of Scope

- No changes to other round files or the root `index.html`
- No build tools, bundlers, or external dependencies
- No persistence / local storage for editor state
- No undo history beyond per-element reset
- No drag-to-resize or visual margin/padding handles on the page itself
