# Font Hierarchy & Click-to-Inspect Editor — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add OffBit font hierarchy and a click-to-inspect CSS editor overlay to `31-the-index.html`.

**Architecture:** Single-file changes to `round-4/31-the-index.html`. Font hierarchy via `@font-face` + CSS variable updates. Editor is a self-contained JS/CSS block appended after existing code, scoped with `[data-editor]` to avoid style conflicts.

**Tech Stack:** Vanilla HTML/CSS/JS, no dependencies.

**Spec:** `docs/superpowers/specs/2026-04-18-font-hierarchy-and-editor-design.md`

---

## File Map

Only one file is modified throughout: **`round-4/31-the-index.html`**

| Region | What changes |
|--------|-------------|
| Lines 12–21 (after existing `@font-face`) | Add 6 new `@font-face` declarations for OffBit family |
| Lines 29–42 (`:root` block) | Add 3 new CSS variables |
| Lines ~100–575 (various selectors) | Update ~10 `font-family` declarations to use new variables |
| After line 583 (end of `</style>`) | Insert editor CSS block (~150 lines) |
| After line 986 (end of existing `</script>`) | Insert editor JS block (~250 lines) |

---

### Task 1: Add OffBit @font-face declarations

**Files:**
- Modify: `round-4/31-the-index.html:21` (after existing Astro Mono `@font-face` blocks)

- [ ] **Step 1: Add 6 @font-face rules after the existing Astro Mono declarations (line 21)**

Insert after the closing `}` of the Astro Mono Bold `@font-face` (line 21):

```css
@font-face {
  font-family: 'OffBit';
  src: url('../references/fonts/T17139-OT/OffBit-Regular.otf') format('opentype');
  font-weight: 400;
}
@font-face {
  font-family: 'OffBit';
  src: url('../references/fonts/T17139-OT/OffBit-Bold.otf') format('opentype');
  font-weight: 700;
}
@font-face {
  font-family: 'OffBit 101';
  src: url('../references/fonts/T17139-OT/OffBit-101.otf') format('opentype');
  font-weight: 400;
}
@font-face {
  font-family: 'OffBit 101';
  src: url('../references/fonts/T17139-OT/OffBit-101-Bold.otf') format('opentype');
  font-weight: 700;
}
@font-face {
  font-family: 'OffBit Dot';
  src: url('../references/fonts/T17139-OT/OffBit-Dot.otf') format('opentype');
  font-weight: 400;
}
@font-face {
  font-family: 'OffBit Dot';
  src: url('../references/fonts/T17139-OT/OffBit-Dot-Bold.otf') format('opentype');
  font-weight: 700;
}
```

- [ ] **Step 2: Add CSS variables to `:root`**

Add these three variables after `--font-secondary` (line 41):

```css
--font-display: 'OffBit', 'Astro Mono', monospace;
--font-label: 'OffBit 101', 'Astro Mono', monospace;
--font-meta: 'OffBit Dot', 'Astro Mono', monospace;
```

- [ ] **Step 3: Open in browser, verify fonts load**

Open `round-4/31-the-index.html` in a browser. Check the Network tab — all 6 `.otf` files should load with 200 status. No change in appearance yet (variables not applied).

- [ ] **Step 4: Commit**

```bash
git add round-4/31-the-index.html
git commit -m "feat: add OffBit font-face declarations and CSS variables"
```

---

### Task 2: Apply font hierarchy to selectors

**Files:**
- Modify: `round-4/31-the-index.html` (CSS selectors throughout `<style>` block)

- [ ] **Step 1: Apply `--font-display` to headline selectors**

Update these selectors to use `font-family: var(--font-display);`:

| Selector | Current line (approx) |
|----------|----------------------|
| `.card-name` | ~102 |
| `.identity-name` | ~211 |
| `.topic-title` | ~354 |

- [ ] **Step 2: Apply `--font-label` to label/UI selectors**

Update these selectors to use `font-family: var(--font-label);`:

| Selector | Current line (approx) |
|----------|----------------------|
| `.section-label` | ~171 |
| `.module-block` | ~405 |

- [ ] **Step 3: Apply `--font-meta` to number/metadata selectors**

Update these selectors to use `font-family: var(--font-meta);`:

| Selector | Current line (approx) |
|----------|----------------------|
| `.index-num` | ~272 |
| `.topic-num` | ~349 |
| `.status-label` | ~312 |
| `.card-year` | ~130 |
| `.card-domain` | ~126 |
| `.footer-machine` | ~523 |
| `.identity-number` | ~203 |

For selectors that don't currently have a `font-family` property, add one. For those that do, replace it.

- [ ] **Step 4: Open in browser, verify font rendering**

Click through the arrival card. Verify:
- Name "OLIVIER GILLAIZEAU" renders in OffBit (blocky/pixel style)
- Section labels like "ESSAYS_" render in OffBit 101 (more pixelated)
- Numbers "01", "02" and status text render in OffBit Dot
- Body text, descriptions, and links remain in Astro Mono

- [ ] **Step 5: Commit**

```bash
git add round-4/31-the-index.html
git commit -m "feat: apply OffBit font hierarchy to display, label, and meta selectors"
```

---

### Task 3: Add editor CSS

**Files:**
- Modify: `round-4/31-the-index.html` (insert before closing `</style>` tag)

- [ ] **Step 1: Add editor CSS before `</style>`**

Insert this block before the closing `</style>` tag. All selectors are scoped with `[data-editor]` to avoid conflicts with existing page styles:

```css
/* ── EDITOR OVERLAY ──────────────────────── */
[data-editor] .editor-toggle {
  position: fixed;
  bottom: 1.5rem;
  right: 1.5rem;
  width: 44px;
  height: 44px;
  border: 1px solid var(--rule-strong);
  background: var(--bg);
  color: var(--text-dim);
  font-family: var(--font-label);
  font-size: 1rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9000;
  transition: all 0.15s;
}

[data-editor].editor-active .editor-toggle {
  border-color: var(--accent);
  color: var(--accent);
  box-shadow: 0 0 20px rgba(74, 158, 255, 0.15);
}

[data-editor] .editor-toggle:hover {
  border-color: var(--accent);
  color: var(--accent);
}

[data-editor] .editor-hint {
  position: fixed;
  bottom: 1.75rem;
  right: 5rem;
  font-family: var(--font-primary);
  font-size: 0.6rem;
  color: var(--text-dim);
  letter-spacing: 0.05em;
  z-index: 9000;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  opacity: 0;
  transition: opacity 0.15s;
}

[data-editor] .editor-toggle:hover ~ .editor-hint {
  opacity: 1;
}

[data-editor] .editor-hint kbd {
  background: var(--bg-card);
  border: 1px solid var(--rule-strong);
  padding: 0.15rem 0.4rem;
  font-family: var(--font-primary);
  font-size: 0.55rem;
  color: var(--text);
}

/* Hover outline in editor mode */
[data-editor].editor-active * {
  cursor: crosshair !important;
}

[data-editor] .editor-hover-outline {
  outline: 2px dashed var(--accent) !important;
  outline-offset: 2px;
}

[data-editor] .editor-selected-outline {
  outline: 2px solid var(--accent) !important;
  outline-offset: 2px;
}

/* Editor Panel */
[data-editor] .editor-panel {
  position: fixed;
  width: 320px;
  background: #151820;
  border: 1px solid var(--rule-strong);
  font-family: var(--font-primary);
  font-size: 0.7rem;
  z-index: 9500;
  box-shadow: 0 8px 32px rgba(0,0,0,0.4);
  display: none;
  max-height: 80vh;
  overflow-y: auto;
}

[data-editor] .editor-panel.visible {
  display: block;
}

[data-editor] .editor-panel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1rem;
  border-bottom: 1px solid var(--rule-strong);
  background: #1A1D24;
  position: sticky;
  top: 0;
  z-index: 1;
}

[data-editor] .editor-panel-tag {
  font-family: var(--font-primary);
  color: var(--accent);
  font-size: 0.65rem;
  letter-spacing: 0.05em;
}

[data-editor] .editor-panel-class {
  font-family: var(--font-primary);
  color: var(--text-dim);
  font-size: 0.6rem;
}

[data-editor] .editor-panel-close {
  color: var(--text-dim);
  cursor: pointer;
  font-size: 0.8rem;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid var(--rule);
  background: none;
  font-family: var(--font-primary);
  transition: border-color 0.15s, color 0.15s;
}

[data-editor] .editor-panel-close:hover {
  border-color: var(--text);
  color: var(--text);
}

[data-editor] .editor-section {
  padding: 0.75rem 1rem;
  border-bottom: 1px solid var(--rule);
}

[data-editor] .editor-section-title {
  font-family: var(--font-label);
  font-size: 0.6rem;
  font-weight: 700;
  color: var(--text-dim);
  letter-spacing: 0.15em;
  margin-bottom: 0.6rem;
  text-transform: uppercase;
}

[data-editor] .editor-prop-row {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.4rem;
}

[data-editor] .editor-prop-label {
  color: var(--text-dim);
  font-size: 0.6rem;
  min-width: 80px;
  letter-spacing: 0.03em;
}

[data-editor] .editor-prop-input {
  flex: 1;
  background: var(--bg);
  border: 1px solid var(--rule-strong);
  color: var(--text-bright);
  font-family: var(--font-primary);
  font-size: 0.6rem;
  padding: 0.3rem 0.5rem;
  outline: none;
  transition: border-color 0.15s;
}

[data-editor] .editor-prop-input:focus {
  border-color: var(--accent);
}

[data-editor] .editor-prop-input-short {
  width: 56px;
  flex: none;
}

[data-editor] .editor-prop-unit {
  color: var(--text-dim);
  font-size: 0.55rem;
}

[data-editor] .editor-color-swatch {
  width: 20px;
  height: 20px;
  border: 1px solid var(--rule-strong);
  cursor: pointer;
  flex-shrink: 0;
  position: relative;
  overflow: hidden;
}

[data-editor] .editor-color-swatch input[type="color"] {
  position: absolute;
  inset: -4px;
  width: calc(100% + 8px);
  height: calc(100% + 8px);
  border: none;
  padding: 0;
  cursor: pointer;
  opacity: 0;
}

/* Spacing box model */
[data-editor] .editor-spacing-box {
  position: relative;
  width: 100%;
  margin: 0.25rem 0;
}

[data-editor] .editor-spacing-outer {
  border: 1px dashed rgba(232, 168, 73, 0.4);
  padding: 1rem 1.5rem;
  position: relative;
}

[data-editor] .editor-spacing-inner {
  border: 1px dashed rgba(74, 158, 255, 0.4);
  padding: 0.75rem 1rem;
  text-align: center;
}

[data-editor] .editor-spacing-core {
  background: rgba(74, 158, 255, 0.1);
  padding: 0.3rem;
  color: var(--text-dim);
  font-size: 0.5rem;
  letter-spacing: 0.05em;
  font-family: var(--font-meta);
}

[data-editor] .editor-spacing-label {
  position: absolute;
  font-size: 0.45rem;
  font-family: var(--font-meta);
  letter-spacing: 0.05em;
}

[data-editor] .editor-spacing-label.margin-l {
  color: var(--amber);
  top: -0.1rem;
  left: 0.3rem;
}

[data-editor] .editor-spacing-label.padding-l {
  color: var(--accent);
  top: -0.1rem;
  left: 0.3rem;
}

[data-editor] .editor-spacing-val {
  position: absolute;
  font-size: 0.55rem;
  font-family: var(--font-meta);
  background: none;
  border: none;
  text-align: center;
  width: 2.5rem;
  padding: 0.1rem;
  outline: none;
  cursor: text;
}

[data-editor] .editor-spacing-val:focus {
  background: var(--bg);
  border: 1px solid var(--accent);
}

[data-editor] .editor-spacing-val.m {
  color: var(--amber);
}

[data-editor] .editor-spacing-val.p {
  color: var(--accent);
}

[data-editor] .editor-spacing-val.top { top: 0.1rem; left: 50%; transform: translateX(-50%); }
[data-editor] .editor-spacing-val.right { right: 0.1rem; top: 50%; transform: translateY(-50%); }
[data-editor] .editor-spacing-val.bottom { bottom: 0.1rem; left: 50%; transform: translateX(-50%); }
[data-editor] .editor-spacing-val.left { left: 0.1rem; top: 50%; transform: translateY(-50%); }

/* Panel actions */
[data-editor] .editor-actions {
  padding: 0.75rem 1rem;
  display: flex;
  gap: 0.5rem;
}

[data-editor] .editor-btn {
  flex: 1;
  padding: 0.45rem 0.75rem;
  font-family: var(--font-label);
  font-size: 0.6rem;
  letter-spacing: 0.08em;
  cursor: pointer;
  border: 1px solid var(--rule-strong);
  background: none;
  color: var(--text-dim);
  transition: all 0.15s;
}

[data-editor] .editor-btn:hover {
  border-color: var(--text);
  color: var(--text);
}

[data-editor] .editor-btn.primary {
  background: var(--accent);
  border-color: var(--accent);
  color: #fff;
}

[data-editor] .editor-btn.primary:hover {
  background: #5AABFF;
}
```

- [ ] **Step 2: Verify no style conflicts**

Open the page in browser. The page should look identical — editor CSS is all scoped to `[data-editor]` which hasn't been added to the DOM yet.

- [ ] **Step 3: Commit**

```bash
git add round-4/31-the-index.html
git commit -m "feat: add scoped editor CSS for click-to-inspect panel"
```

---

### Task 4: Add editor toggle button and keyboard shortcut

**Files:**
- Modify: `round-4/31-the-index.html` (new `<script>` block after existing script, plus toggle button HTML before `</body>`)

- [ ] **Step 1: Add editor HTML before closing `</body>`**

Insert before `</body>` (after the existing `</script>`):

```html
<!-- ════════════════════════════════════════════
     EDITOR OVERLAY (dev tool)
     ════════════════════════════════════════════ -->
<div data-editor>
  <button class="editor-toggle" aria-label="Toggle editor">E</button>
  <span class="editor-hint"><kbd>E</kbd> toggle editor</span>
  <div class="editor-panel" id="editor-panel"></div>
</div>
```

- [ ] **Step 2: Add editor JS — toggle logic**

Add a new `<script>` block after the editor HTML:

```javascript
(function() {
  'use strict';

  var editorRoot = document.querySelector('[data-editor]');
  var toggleBtn = editorRoot.querySelector('.editor-toggle');
  var panel = document.getElementById('editor-panel');
  var active = false;
  var selectedEl = null;
  var hoveredEl = null;
  var modifications = new Map(); // element -> { original: {}, current: {} }

  // Don't activate during arrival overlay
  function isArrivalVisible() {
    var overlay = document.getElementById('arrival-overlay');
    return overlay && overlay.style.display !== 'none' && !overlay.classList.contains('dissolving');
  }

  function toggleEditor() {
    if (isArrivalVisible()) return;
    active = !active;
    editorRoot.classList.toggle('editor-active', active);
    if (!active) {
      deselectElement();
      if (hoveredEl) {
        hoveredEl.classList.remove('editor-hover-outline');
        hoveredEl = null;
      }
    }
  }

  toggleBtn.addEventListener('click', function(e) {
    e.stopPropagation();
    toggleEditor();
  });

  document.addEventListener('keydown', function(e) {
    if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;
    if (e.key === 'e' || e.key === 'E') {
      // Don't toggle if typing in editor inputs
      if (e.target.closest('[data-editor]') && e.target.tagName === 'INPUT') return;
      toggleEditor();
    }
    if (e.key === 'Escape' && active) {
      deselectElement();
      active = false;
      editorRoot.classList.remove('editor-active');
    }
  });

  // ── HOVER ──────────────────────────────
  document.addEventListener('mouseover', function(e) {
    if (!active) return;
    var target = e.target;
    if (target.closest('[data-editor]')) return;
    if (hoveredEl && hoveredEl !== target) {
      hoveredEl.classList.remove('editor-hover-outline');
    }
    if (target !== selectedEl) {
      target.classList.add('editor-hover-outline');
      hoveredEl = target;
    }
  });

  document.addEventListener('mouseout', function(e) {
    if (!active || !hoveredEl) return;
    hoveredEl.classList.remove('editor-hover-outline');
    hoveredEl = null;
  });

  // ── SELECT ─────────────────────────────
  document.addEventListener('click', function(e) {
    if (!active) return;
    var target = e.target;
    if (target.closest('[data-editor]')) return;
    e.preventDefault();
    e.stopPropagation();
    selectElement(target);
  }, true);

  function selectElement(el) {
    deselectElement();
    selectedEl = el;
    el.classList.remove('editor-hover-outline');
    el.classList.add('editor-selected-outline');

    // Track original styles if not already tracked
    if (!modifications.has(el)) {
      var cs = window.getComputedStyle(el);
      modifications.set(el, {
        original: {
          fontSize: cs.fontSize,
          fontWeight: cs.fontWeight,
          letterSpacing: cs.letterSpacing,
          lineHeight: cs.lineHeight,
          color: cs.color,
          backgroundColor: cs.backgroundColor,
          paddingTop: cs.paddingTop,
          paddingRight: cs.paddingRight,
          paddingBottom: cs.paddingBottom,
          paddingLeft: cs.paddingLeft,
          marginTop: cs.marginTop,
          marginRight: cs.marginRight,
          marginBottom: cs.marginBottom,
          marginLeft: cs.marginLeft,
          borderWidth: cs.borderTopWidth,
          borderColor: cs.borderTopColor
        },
        changed: {}
      });
    }

    showPanel(el);
  }

  function deselectElement() {
    if (selectedEl) {
      selectedEl.classList.remove('editor-selected-outline');
      selectedEl = null;
    }
    panel.classList.remove('visible');
  }

  // ── PANEL ──────────────────────────────
  function showPanel(el) {
    var rect = el.getBoundingClientRect();
    var cs = window.getComputedStyle(el);
    var tag = el.tagName;
    var cls = el.className
      .split(' ')
      .filter(function(c) { return c && !c.startsWith('editor-'); })
      .map(function(c) { return '.' + c; })
      .join('') || '(no class)';

    panel.innerHTML = buildPanelHTML(tag, cls, cs);
    panel.classList.add('visible');

    // Position panel
    var pw = 320;
    var ph = panel.offsetHeight;
    var left = rect.right + 12;
    var top = rect.top;

    if (left + pw > window.innerWidth - 16) {
      left = rect.left - pw - 12;
    }
    if (left < 16) left = 16;
    if (top + ph > window.innerHeight - 16) {
      top = window.innerHeight - ph - 16;
    }
    if (top < 16) top = 16;

    panel.style.left = left + 'px';
    panel.style.top = top + 'px';

    bindPanelEvents(el, cs);
  }

  function buildPanelHTML(tag, cls, cs) {
    var pxToNum = function(v) { return parseFloat(v) || 0; };
    var rgbToHex = function(rgb) {
      if (!rgb || rgb === 'transparent' || rgb === 'rgba(0, 0, 0, 0)') return 'transparent';
      var m = rgb.match(/(\d+),\s*(\d+),\s*(\d+)/);
      if (!m) return rgb;
      return '#' + [m[1],m[2],m[3]].map(function(x) {
        return parseInt(x).toString(16).padStart(2,'0');
      }).join('');
    };

    return ''
      + '<div class="editor-panel-header">'
      +   '<div>'
      +     '<div class="editor-panel-tag">' + tag + '</div>'
      +     '<div class="editor-panel-class">' + cls + '</div>'
      +   '</div>'
      +   '<button class="editor-panel-close" id="editor-close">&times;</button>'
      + '</div>'

      + '<div class="editor-section">'
      +   '<div class="editor-section-title">TYPOGRAPHY</div>'
      +   propRow('font-size', pxToNum(cs.fontSize), 'px')
      +   propRow('font-weight', cs.fontWeight, '')
      +   propRow('letter-spacing', pxToNum(cs.letterSpacing), 'px')
      +   propRow('line-height', pxToNum(cs.lineHeight), 'px')
      + '</div>'

      + '<div class="editor-section">'
      +   '<div class="editor-section-title">COLOR</div>'
      +   colorRow('color', rgbToHex(cs.color))
      +   colorRow('background-color', rgbToHex(cs.backgroundColor))
      + '</div>'

      + '<div class="editor-section">'
      +   '<div class="editor-section-title">SPACING</div>'
      +   spacingBox(cs)
      + '</div>'

      + '<div class="editor-section">'
      +   '<div class="editor-section-title">BORDER</div>'
      +   propRow('border-width', pxToNum(cs.borderTopWidth), 'px')
      +   colorRow('border-color', rgbToHex(cs.borderTopColor))
      + '</div>'

      + '<div class="editor-actions">'
      +   '<button class="editor-btn" id="editor-reset">RESET</button>'
      +   '<button class="editor-btn primary" id="editor-copy">COPY CSS</button>'
      + '</div>';
  }

  function propRow(prop, val, unit) {
    return '<div class="editor-prop-row">'
      + '<span class="editor-prop-label">' + prop + '</span>'
      + '<input class="editor-prop-input editor-prop-input-short" data-prop="' + prop + '" value="' + val + '" />'
      + (unit ? '<span class="editor-prop-unit">' + unit + '</span>' : '')
      + '</div>';
  }

  function colorRow(prop, hex) {
    return '<div class="editor-prop-row">'
      + '<span class="editor-prop-label">' + prop.replace('background-color','background') + '</span>'
      + '<div class="editor-color-swatch" style="background:' + hex + ';">'
      +   '<input type="color" data-color-prop="' + prop + '" value="' + (hex.startsWith('#') ? hex : '#000000') + '" />'
      + '</div>'
      + '<input class="editor-prop-input" data-prop="' + prop + '" value="' + hex + '" />'
      + '</div>';
  }

  function spacingBox(cs) {
    var pxN = function(v) { return parseFloat(v) || 0; };
    return '<div class="editor-spacing-box">'
      + '<div class="editor-spacing-outer">'
      +   '<span class="editor-spacing-label margin-l">margin</span>'
      +   spacingInput('margin-top', pxN(cs.marginTop), 'm top')
      +   spacingInput('margin-right', pxN(cs.marginRight), 'm right')
      +   spacingInput('margin-bottom', pxN(cs.marginBottom), 'm bottom')
      +   spacingInput('margin-left', pxN(cs.marginLeft), 'm left')
      +   '<div class="editor-spacing-inner">'
      +     '<span class="editor-spacing-label padding-l">padding</span>'
      +     spacingInput('padding-top', pxN(cs.paddingTop), 'p top')
      +     spacingInput('padding-right', pxN(cs.paddingRight), 'p right')
      +     spacingInput('padding-bottom', pxN(cs.paddingBottom), 'p bottom')
      +     spacingInput('padding-left', pxN(cs.paddingLeft), 'p left')
      +     '<div class="editor-spacing-core">ELEMENT</div>'
      +   '</div>'
      + '</div>'
      + '</div>';
  }

  function spacingInput(prop, val, classes) {
    var type = classes.startsWith('m') ? 'm' : 'p';
    var dir = classes.split(' ')[1];
    return '<input class="editor-spacing-val ' + type + ' ' + dir + '" data-prop="' + prop + '" value="' + val + '" />';
  }

  // ── PANEL EVENTS ───────────────────────
  function bindPanelEvents(el, cs) {
    // Close button
    document.getElementById('editor-close').addEventListener('click', function() {
      deselectElement();
    });

    // Property inputs
    panel.querySelectorAll('input[data-prop]').forEach(function(input) {
      input.addEventListener('input', function() {
        var prop = input.dataset.prop;
        var val = input.value;
        var unit = '';

        // Determine if value needs a unit
        var numProps = ['font-size','font-weight','letter-spacing','line-height',
          'border-width','padding-top','padding-right','padding-bottom','padding-left',
          'margin-top','margin-right','margin-bottom','margin-left'];

        if (numProps.indexOf(prop) !== -1 && prop !== 'font-weight') {
          unit = 'px';
        }

        var cssVal = val + unit;
        if (prop === 'font-weight') cssVal = val;
        if (prop === 'color' || prop === 'background-color' || prop === 'border-color') cssVal = val;

        el.style.setProperty(prop, cssVal);

        // Track change
        var mod = modifications.get(el);
        if (mod) mod.changed[prop] = cssVal;
      });
    });

    // Color pickers
    panel.querySelectorAll('input[data-color-prop]').forEach(function(picker) {
      picker.addEventListener('input', function() {
        var prop = picker.dataset.colorProp;
        el.style.setProperty(prop, picker.value);
        // Update hex input and swatch
        var hexInput = panel.querySelector('input[data-prop="' + prop + '"]');
        if (hexInput) hexInput.value = picker.value;
        picker.parentElement.style.background = picker.value;
        var mod = modifications.get(el);
        if (mod) mod.changed[prop] = picker.value;
      });
    });

    // Reset button
    document.getElementById('editor-reset').addEventListener('click', function() {
      var mod = modifications.get(el);
      if (mod) {
        Object.keys(mod.changed).forEach(function(prop) {
          el.style.removeProperty(prop);
        });
        mod.changed = {};
        // Refresh panel
        showPanel(el);
      }
    });

    // Copy CSS button
    document.getElementById('editor-copy').addEventListener('click', function() {
      var css = generateCSS();
      navigator.clipboard.writeText(css).then(function() {
        var btn = document.getElementById('editor-copy');
        btn.textContent = 'COPIED';
        setTimeout(function() { btn.textContent = 'COPY CSS'; }, 1500);
      });
    });
  }

  // ── CSS EXPORT ─────────────────────────
  function generateCSS() {
    var lines = [];
    modifications.forEach(function(mod, el) {
      if (Object.keys(mod.changed).length === 0) return;
      var selector = getSelector(el);
      lines.push(selector + ' {');
      Object.keys(mod.changed).forEach(function(prop) {
        lines.push('  ' + prop + ': ' + mod.changed[prop] + ';');
      });
      lines.push('}');
      lines.push('');
    });
    return lines.join('\n');
  }

  function getSelector(el) {
    var classes = Array.from(el.classList).filter(function(c) {
      return !c.startsWith('editor-');
    });
    if (classes.length > 0) return '.' + classes[0];
    return el.tagName.toLowerCase();
  }

})();
```

- [ ] **Step 3: Test toggle behavior**

Open in browser. Verify:
1. `E` button visible in bottom-right corner
2. Click it or press `E` key → editor mode activates (button glows blue)
3. Hover over elements → dashed blue outline appears
4. Click an element → panel appears with correct values
5. Press `Escape` → deselects and exits editor mode
6. Page works normally when editor is off

- [ ] **Step 4: Test property editing**

In editor mode:
1. Click the name heading → panel shows font-size ~32px
2. Change font-size to 48 → text grows live
3. Change color to a red hex → text turns red
4. Click Reset → reverts to original
5. Make a change, click Copy CSS → clipboard contains valid CSS

- [ ] **Step 5: Test edge cases**

1. Arrival overlay: press `E` before clicking through → should not activate
2. Panel positioning: click an element near the right edge → panel should flip to the left
3. Click the editor panel itself → should not select panel elements
4. Multiple elements: edit 2 different elements, click Copy CSS → both appear in output

- [ ] **Step 6: Commit**

```bash
git add round-4/31-the-index.html
git commit -m "feat: add click-to-inspect editor with live CSS editing and export"
```

---

### Task 5: Final verification and push

- [ ] **Step 1: Full page walkthrough**

1. Open `round-4/31-the-index.html`
2. Arrival card: name should be in OffBit, domain in OffBit Dot
3. Click through → workspace loads
4. Verify all four font tiers render correctly throughout the page
5. Toggle editor, make some tweaks, copy CSS
6. Toggle editor off, verify page behaves normally
7. Check mobile responsive view (600px, 400px breakpoints)

- [ ] **Step 2: Push to GitHub**

```bash
git push origin main
```

GitHub Pages will auto-deploy. Verify at `https://ulkogan.github.io/frenchbutnice/round-4/31-the-index.html`.
