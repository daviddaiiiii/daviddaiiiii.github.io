# CSS Architecture Refactor Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Consolidate `accent-enhancements.css` into a proper SCSS partial `_custom.scss`, eliminate duplicate rules, unify all colors under SCSS variables, and update the color scheme to deep ocean blue + forest green.

**Architecture:** All custom styles migrate from the standalone `accent-enhancements.css` into `_sass/_custom.scss`, which is imported by `main.scss` in the SCSS compilation chain. Color values are declared once in `_variables.scss` (SCSS vars) and `_base.scss` (CSS vars), then referenced everywhere else via variables.

**Tech Stack:** Jekyll, SCSS (sass gem), Minimal Mistakes theme

**Spec:** `docs/superpowers/specs/2026-03-10-css-architecture-refactor-design.md`

**Build command:** `bundle exec jekyll build` (or `bash run_server.sh` for live preview)

---

## Chunk 1: Color Variables

### Task 1: Update SCSS color variables in `_variables.scss`

**Files:**
- Modify: `_sass/_variables.scss`

- [ ] **Step 1: Replace primary color block**

Find the block starting with `// 主色调系统 - 深蓝色系` and replace:

```scss
// 主色调系统 — 深洋蓝
$primary-color              : #0f2744;
$primary-light              : #1a3a5c;
$primary-lighter            : #2a5278;
$primary-dark               : #091c30;
$primary-darker             : #050f1a;
```

- [ ] **Step 2: Replace accent color block**

Find the block starting with `// 强调色系统 - 珊瑚粉系` and replace:

```scss
// 强调色系统 — 森林绿
$accent-color               : #1e6b52;
$accent-light               : #27a077;
$accent-lighter             : #3dbf8f;
$accent-dark                : #155040;
$accent-darker              : #0d3529;
```

- [ ] **Step 3: Update gradient definitions**

Replace the gradient block:

```scss
$gradient-primary           : linear-gradient(135deg, $primary-color 0%, $primary-light 50%, $accent-color 100%);
$gradient-accent            : linear-gradient(135deg, $accent-color 0%, $accent-light 50%, $accent-lighter 100%);
$gradient-subtle            : linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
$gradient-hero              : linear-gradient(135deg, $primary-darker 0%, $primary-color 30%, $accent-dark 100%);
```

- [ ] **Step 4: Update box-shadow variables** (they reference old primary color tone)

```scss
$box-shadow                 : 0 4px 15px rgba(15, 39, 68, 0.08);
$box-shadow-hover           : 0 8px 25px rgba(15, 39, 68, 0.12);
$box-shadow-accent          : 0 4px 15px rgba(30, 107, 82, 0.15);
```

- [ ] **Step 5: Build to verify no SCSS compilation errors**

```bash
bundle exec jekyll build 2>&1 | grep -E "error|Error|warning" | head -20
```

Expected: no errors output (warnings about deprecation are OK)

- [ ] **Step 6: Commit**

```bash
git add _sass/_variables.scss
git commit -m "refactor(css): update color scheme to deep ocean blue + forest green"
```

---

### Task 2: Update CSS variables in `_base.scss`

**Files:**
- Modify: `_sass/_base.scss`

- [ ] **Step 1: Update `:root {}` CSS custom properties**

Find the `:root {` block and update these values:

```css
:root {
  --gradient-bg: linear-gradient(145deg, #ffffff, #f8f9fa);
  --gradient-accent: linear-gradient(135deg, #1e6b52 0%, #27a077 50%, #3dbf8f 100%);
  --gradient-primary: linear-gradient(135deg, #0f2744 0%, #1a3a5c 50%, #1e6b52 100%);
  --gradient-subtle: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
  --box-shadow: 0 4px 15px rgba(15, 39, 68, 0.08);
  --box-shadow-hover: 0 8px 25px rgba(15, 39, 68, 0.12);
  --box-shadow-accent: 0 4px 15px rgba(30, 107, 82, 0.15);
  --border-radius: 8px;
  --border-radius-large: 12px;
  --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  --spacing-unit: 1.4rem;

  --accent-color: #1e6b52;
  --accent-light: #27a077;
  --accent-dark: #155040;
  --primary-color: #0f2744;
  --primary-light: #1a3a5c;

  --bg-white: #ffffff;
  --bg-light: #f8f9fa;
  --bg-card: linear-gradient(145deg, #ffffff, #f8f9fa);
}
```

- [ ] **Step 2: Build to verify no errors**

```bash
bundle exec jekyll build 2>&1 | grep -E "error|Error" | head -20
```

Expected: no errors

- [ ] **Step 3: Commit**

```bash
git add _sass/_base.scss
git commit -m "refactor(css): sync CSS custom properties with new color scheme"
```

---

## Chunk 2: Create `_custom.scss`

### Task 3: Create `_sass/_custom.scss` from `accent-enhancements.css`

This is the core migration. Create the file with all content from `accent-enhancements.css`, organized into sections, with all hardcoded hex values replaced by SCSS variables.

**Files:**
- Create: `_sass/_custom.scss`

- [ ] **Step 1: Create `_sass/_custom.scss` with the full content below**

```scss
// =============================================================
// _custom.scss — Academic Homepage Custom Styles
// All colors use SCSS variables from _variables.scss
// =============================================================


// -------------------------------------------------------------
// 1. Page Structure & Decorative Elements
// -------------------------------------------------------------

.main-content {
  position: relative;
}

.accent-border-top {
  position: relative;
}

.accent-border-top::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, $primary-color 0%, $accent-color 50%, $primary-light 100%);
  z-index: 10;
}


// -------------------------------------------------------------
// 2. Typography & Text Utilities
// -------------------------------------------------------------

.accent-text {
  color: $accent-color;
  font-weight: 600;
}

.primary-gradient-text {
  color: $primary-color;
  font-weight: 600;
}

.link-accent {
  color: $primary-color;
  text-decoration: none;
  position: relative;
  font-weight: 500;
  transition: all 0.3s ease;

  &::after {
    content: '';
    position: absolute;
    bottom: -2px;
    left: 0;
    width: 0;
    height: 2px;
    background: linear-gradient(90deg, $accent-color 0%, $accent-light 100%);
    transition: width 0.3s ease;
  }

  &:hover {
    color: $accent-color;

    &::after {
      width: 100%;
    }
  }
}

.divider-accent {
  height: 2px;
  background: linear-gradient(90deg, transparent 0%, $accent-color 50%, transparent 100%);
  margin: 2rem 0;
  border: none;
}


// -------------------------------------------------------------
// 3. Buttons
// -------------------------------------------------------------

.btn-accent {
  background: $accent-color;
  color: white !important;
  border: none;
  padding: 12px 24px;
  border-radius: 25px;
  font-weight: 600;
  text-decoration: none;
  display: inline-block;
  transition: $global-transition;
  box-shadow: 0 4px 15px rgba($accent-color, 0.3);
  position: relative;
  overflow: hidden;

  &::after {
    display: none;
  }

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
    transition: left 0.5s;
  }

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 25px rgba($accent-color, 0.4);
    color: white !important;

    &::before {
      left: 100%;
    }
  }
}


// -------------------------------------------------------------
// 4. Cards & Floating Panels
// -------------------------------------------------------------

.floating-card {
  background: #ffffff !important;
  border-radius: $border-radius-large;
  box-shadow: $box-shadow;
  transition: $global-transition;
  border: 1px solid rgba($primary-color, 0.06);
  position: relative;
  overflow: hidden;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: linear-gradient(90deg, $primary-color 0%, $accent-color 100%);
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  &:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 24px rgba($primary-color, 0.12);
    border-color: rgba($accent-color, 0.15);

    &::before {
      opacity: 1;
    }
  }
}

.quote-accent {
  background: #f8f9fa !important;
  border-left: 4px solid $accent-color;
  padding: 1.5rem 2rem;
  border-radius: $border-radius;
  box-shadow: $box-shadow;
  position: relative;
  margin: 2rem 0;
  color: $primary-color !important;
}

.highlight-blocks {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1.2rem;
  margin: 1.5rem 0;
}

.highlight-block {
  padding: 1.2rem 1.4rem;
}


// -------------------------------------------------------------
// 5. Publication Cards (.paper-box visual styles)
// Note: base flex layout remains in main.scss
// -------------------------------------------------------------

.paper-box {
  background: #ffffff !important;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba($primary-color, 0.06);
  border: 1px solid rgba($primary-color, 0.07);
  border-left: 3px solid $accent-color;
  transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease;
  position: relative;
  margin-bottom: 1.4rem;

  &:hover {
    transform: translateX(4px);
    box-shadow: 0 4px 20px rgba($primary-color, 0.1);
    border-color: rgba($primary-color, 0.1);
    border-left-color: $primary-color;
  }

  &.hidden {
    display: none !important;
  }
}

.paper-box-text {
  padding: 1.5rem 1.8rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  min-width: 0;

  h3 {
    margin: 0 0 0.55rem 0;
    font-size: 1.05rem;
    font-weight: 700;
    color: $primary-color;
    line-height: 1.4;
    letter-spacing: -0.01em;

    a {
      color: inherit;
      text-decoration: none;
      background-image: linear-gradient(currentColor, currentColor);
      background-position: 0% 100%;
      background-repeat: no-repeat;
      background-size: 0% 1.5px;
      transition: background-size 0.3s ease, color 0.2s ease;

      &:hover {
        color: $accent-color;
        background-size: 100% 1.5px;
        text-decoration: none;
      }
    }
  }

  p {
    margin: 0.4rem 0;
    color: #495057;
    line-height: 1.6;
    font-size: 0.9rem;
  }

  .authors {
    font-weight: 500;
    color: $primary-light;
    margin-bottom: 0.3rem;
    font-size: 0.875rem;
    line-height: 1.5;

    .primary-gradient-text {
      color: $accent-color;
      font-weight: 700;
    }
  }

  .venue {
    color: #7a8a9a;
    font-style: italic;
    margin-bottom: 0;
    font-size: 0.82rem;
    line-height: 1.4;
  }

  .links {
    display: flex;
    gap: 0.8rem;
    flex-wrap: wrap;
    margin-top: 0.75rem;
  }
}

@media (max-width: 768px) {
  .paper-box {
    margin-bottom: 0.85rem;
    border-radius: 8px;
  }

  .paper-box-text {
    padding: 1rem 1.1rem;

    h3 {
      font-size: 0.97rem;
    }
  }
}


// -------------------------------------------------------------
// 6. Publication Filter
// -------------------------------------------------------------

#filter-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-bottom: 1.5rem;
  padding: 0.75rem 1rem;
  background: #f8f9fa;
  border-radius: $border-radius;
  border: 1px solid rgba($primary-color, 0.08);
}

.filter-btn {
  display: inline-flex;
  align-items: center;
  padding: 0.3rem 0.8rem;
  background: #ffffff;
  color: #495057;
  border: 1.5px solid #dee2e6;
  border-radius: 5px;
  font-size: 0.8rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  font-family: inherit;
  letter-spacing: 0.01em;

  &:hover {
    border-color: $primary-color;
    color: $primary-color;
    background: rgba($primary-color, 0.04);
  }

  &.active {
    background: $primary-color;
    color: #ffffff;
    border-color: $primary-color;
    box-shadow: 0 2px 6px rgba($primary-color, 0.2);
  }
}


// -------------------------------------------------------------
// 7. Tag Badges
// -------------------------------------------------------------

.badge-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.3rem;
  margin-top: 0.65rem;
}

.inner-tag-badge {
  display: inline-block;
  padding: 0.1rem 0.5rem;
  background: rgba($primary-color, 0.06);
  color: $primary-light;
  border: 1px solid rgba($primary-color, 0.12);
  border-radius: 4px;
  font-size: 0.73rem;
  font-weight: 500;
  letter-spacing: 0.01em;
  transition: all 0.2s ease;

  &.active {
    background: $primary-color;
    color: #ffffff;
    border-color: $primary-color;
  }

  // Author role tags — forest green
  &.tag-author {
    background: rgba($accent-color, 0.1);
    color: darken($accent-color, 5%);
    border-color: rgba($accent-color, 0.25);
    font-weight: 600;

    &.active {
      background: $accent-color;
      color: #ffffff;
      border-color: $accent-color;
    }
  }

  // Journal metric tags — teal (intentional hardcoded values; no SCSS variable defined for teal)
  &.tag-metric {
    background: rgba(16, 130, 120, 0.08);
    color: #0d6e68;
    border-color: rgba(16, 130, 120, 0.2);

    &.active {
      background: #108278;
      color: #ffffff;
      border-color: #108278;
    }
  }
}

.tag-accent {
  background: linear-gradient(135deg, $accent-color 0%, $accent-light 100%);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  display: inline-block;
  margin: 0.25rem;
  box-shadow: 0 2px 8px rgba($accent-color, 0.3);
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba($accent-color, 0.4);
  }
}

.badge {
  padding-left: 0.8rem;
  padding-right: 0.8rem;
  position: absolute;
  margin-top: .5em;
  margin-left: -.5em;
  color: white;
  background: linear-gradient(135deg, $accent-color 0%, $accent-light 100%);
  font-size: .8em;
  border-radius: 15px;
  font-weight: 600;
  box-shadow: 0 2px 8px rgba($accent-color, 0.2);
}


// -------------------------------------------------------------
// 8. Sidebar & Icons
// -------------------------------------------------------------

.fas, .fab, .far {
  color: $primary-color;
  transition: all 0.3s ease;

  &:hover {
    color: $primary-light;
    transform: scale(1.05);
  }
}

.sidebar .fas, .sidebar .fab, .sidebar .far,
.author__urls .fas, .author__urls .fab, .author__urls .far {
  color: $primary-color !important;

  &:hover {
    color: $accent-color !important;
  }
}


// -------------------------------------------------------------
// 9. Animations & Utilities
// -------------------------------------------------------------

.hover-float {
  transition: $global-transition;

  &:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba($primary-color, 0.12);
  }
}

.pulse-accent {
  animation: pulse-accent 2s infinite;
}

@keyframes pulse-accent {
  0%   { box-shadow: 0 4px 12px rgba($accent-color, 0.25); }
  50%  { box-shadow: 0 4px 20px rgba($accent-color, 0.4); }
  100% { box-shadow: 0 4px 12px rgba($accent-color, 0.25); }
}

.bg-gradient-animate {
  background: linear-gradient(-45deg, $primary-color, $primary-light, $accent-color, $accent-light);
  background-size: 400% 400%;
  animation: gradient-shift 15s ease infinite;
}

@keyframes gradient-shift {
  0%   { background-position: 0% 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.fade-in-up {
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.6s ease;

  &.animate {
    opacity: 1;
    transform: translateY(0);
  }
}

.loading-accent {
  width: 40px;
  height: 40px;
  border: 3px solid #e9ecef;
  border-top: 3px solid $accent-color;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0%   { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.progress-accent {
  height: 8px;
  background: #e9ecef;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
}

.progress-accent-bar {
  height: 100%;
  background: linear-gradient(90deg, $accent-color 0%, $accent-light 100%);
  border-radius: 10px;
  transition: width 1s ease;
  position: relative;

  &::after {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0; bottom: 0;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
    animation: shimmer 2s infinite;
  }
}

@keyframes shimmer {
  0%   { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}


// -------------------------------------------------------------
// 10. Responsive Overrides
// -------------------------------------------------------------

@media (max-width: 768px) {
  .highlight-blocks {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .floating-card {
    margin: 1rem 0;
  }

  .btn-accent {
    padding: 10px 20px;
    font-size: 0.9rem;
  }
}

// Force light theme — prevent dark mode interference
@media (prefers-color-scheme: dark) {
  .floating-card, .highlight-block, .paper-box, .quote-accent {
    background: #ffffff !important;
    color: $primary-color !important;
    border-color: rgba($primary-color, 0.08) !important;
  }

  .floating-card *, .highlight-block *, .paper-box *, .quote-accent * {
    color: $primary-color !important;
  }

  .floating-card a, .highlight-block a, .paper-box a, .quote-accent a {
    color: $primary-color !important;

    &:hover {
      color: $accent-color !important;
    }
  }
}

// Ensure base body color
body {
  background-color: #ffffff !important;
  color: $primary-color !important;
}

.highlight-block, .paper-box, .floating-card, blockquote, .quote-accent {
  background: #ffffff !important;
  color: $primary-color !important;
}

.highlight-block *, .paper-box *, .floating-card *, blockquote *, .quote-accent * {
  color: $primary-color !important;
}

// Keep button and badge text white
.btn-accent, .tag-accent, .badge {
  color: white !important;
}


// -------------------------------------------------------------
// Blog Grid (for future blog section)
// -------------------------------------------------------------

.blog-grid {
  display: grid !important;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)) !important;
  gap: 1.5rem !important;
  margin: 2rem 0 !important;
}

.blog-card {
  background: #ffffff;
  border-radius: $border-radius-large;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba($primary-color, 0.08);
  transition: $global-transition;

  &:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba($primary-color, 0.12);
  }
}

.blog-card-image {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
  }
}

.blog-badge {
  position: absolute;
  top: 12px;
  left: 12px;
  background: $accent-color;
  color: white;
  padding: 0.4em 0.8em;
  border-radius: 20px;
  font-size: 0.75em;
  font-weight: 600;
  box-shadow: 0 2px 8px rgba($accent-color, 0.3);
  z-index: 10;
}

.blog-card-content {
  padding: 1.2rem;
}

.blog-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: $primary-color;
  margin-bottom: 0.8rem;
  line-height: 1.3;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.blog-description {
  font-size: 0.9rem;
  color: #666;
  line-height: 1.5;
  margin-bottom: 1rem;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.blog-links {
  display: flex;
  gap: 0.6rem;
  flex-wrap: wrap;
}

.blog-link {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.5rem 1rem;
  background: $accent-color;
  color: white !important;
  text-decoration: none !important;
  border-radius: 25px;
  font-size: 0.85rem;
  font-weight: 600;
  box-shadow: 0 3px 8px rgba($accent-color, 0.2);
  transition: $global-transition;
  position: relative;
  overflow: hidden;
  border: none;

  &::after {
    display: none !important;
  }

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
    transition: left 0.5s;
  }

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 15px rgba($accent-color, 0.3);
    color: white !important;
    text-decoration: none !important;

    &::before {
      left: 100%;
    }
  }

  i {
    font-size: 0.9em;
    transition: transform 0.3s ease;
  }

  &:hover i {
    transform: scale(1.1);
  }
}

@media (max-width: 768px) {
  .blog-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
    margin: 1rem 0;
  }

  .blog-card-image { height: 180px; }
  .blog-card-content { padding: 1rem; }
  .blog-title { font-size: 1rem; }
  .blog-description { font-size: 0.85rem; }

  .blog-links {
    gap: 0.5rem;
    flex-direction: column;
  }

  .blog-link {
    font-size: 0.8rem;
    padding: 0.45rem 0.8rem;
    border-radius: 20px;
    justify-content: center;
    width: 100%;
    box-sizing: border-box;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .blog-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1025px) {
  .blog-grid { grid-template-columns: repeat(3, 1fr); }
}
```

- [ ] **Step 2: Build to verify file compiles without errors**

```bash
bundle exec jekyll build 2>&1 | grep -E "error|Error" | head -20
```

Expected: no errors

- [ ] **Step 3: Commit**

```bash
git add _sass/_custom.scss
git commit -m "feat(css): add _custom.scss partial with migrated and organized styles"
```

---

## Chunk 3: Wire up `main.scss` and remove old files

### Task 4: Update `main.scss` — add import, remove old blocks

**Files:**
- Modify: `assets/css/main.scss`

- [ ] **Step 1: Add `@import "custom"` at the end of the imports section**

After the last `@import "print";` line, add:

```scss
@import "custom";
```

- [ ] **Step 2: Delete the inline custom style blocks from `main.scss`**

After the `@import "print";` and `@import "custom";` lines, the file should contain ONLY the scroll-offset anchor block below. Delete everything else that follows the imports:

- Delete the `/* Paper Filter Styles (Added by User) */` block (`.filter-btn`, `.paper-box` overrides, `.inner-tag-badge`, `.badge`)
- Delete the `/* Inner Card Tags Highlight Logic */` block

The only non-import block that must remain after the imports is:
```scss
$scroll_offset : 2em;
h1:before, .anchor:before {
  content: '';
  display: block;
  position: relative;
  width: 0;
  height: $scroll_offset;
  margin-top: -$scroll_offset;
}
```

- [ ] **Step 3: Build to verify**

```bash
bundle exec jekyll build 2>&1 | grep -E "error|Error" | head -20
```

Expected: no errors

- [ ] **Step 4: Commit**

```bash
git add assets/css/main.scss
git commit -m "refactor(css): wire _custom.scss import, remove duplicate inline style blocks"
```

---

### Task 5: Remove `accent-enhancements.css` and clean up `head.html`

**Files:**
- Delete: `assets/css/accent-enhancements.css`
- Modify: `_includes/head.html` (if it references `accent-enhancements.css`)

- [ ] **Step 1: Check if `head.html` references `accent-enhancements.css`**

```bash
grep -n "accent-enhancements" _includes/head.html _layouts/*.html _pages/*.md 2>/dev/null
```

If any matches found, remove those `<link>` tags.

- [ ] **Step 2: Delete the old CSS file**

```bash
rm assets/css/accent-enhancements.css
```

- [ ] **Step 3: Build to verify nothing broke**

```bash
bundle exec jekyll build 2>&1 | grep -E "error|Error" | head -20
```

Expected: no errors. The `_site/` directory should no longer contain `accent-enhancements.css`.

```bash
ls _site/assets/css/
```

Expected: `main.css` only (no `accent-enhancements.css`)

- [ ] **Step 4: Commit**

```bash
git rm assets/css/accent-enhancements.css
git add _includes/head.html
git commit -m "refactor(css): delete accent-enhancements.css, complete CSS architecture migration"
```

---

## Chunk 4: Visual Verification

### Task 6: Verify visual output

- [ ] **Step 1: Start the dev server**

```bash
bash run_server.sh
```

Or: `bundle exec jekyll serve --livereload`

- [ ] **Step 2: Check each visual element**

Open `http://localhost:4000` and verify:

| Element | What to check |
|---|---|
| Sidebar | Author name, links in deep ocean blue |
| Sidebar icons | Deep blue, turn forest green on hover |
| Publication cards | Forest green left border, deep blue title |
| Publication filter buttons | White default → deep blue active state |
| Tag badges | `tag-author` in green, `tag-metric` in teal |
| Highlight blocks (Researcher/Developer/Collaboration) | Cards with top gradient on hover |
| Quote accent block (research focus) | Forest green left border |
| Links | Deep blue → forest green on hover |
| Page top bar (if visible) | Deep blue to forest green gradient |

- [ ] **Step 3: Check for any remaining old coral/pink color (#FE667B)**

```bash
grep -r "FE667B\|fe667b\|012F63\|012f63" _site/assets/css/ 2>/dev/null | head -20
```

Expected: no matches (all replaced by new color values)

- [ ] **Step 4: If any visual discrepancies found in Step 2, fix the relevant file and commit**

Each fix should be a targeted edit to `_sass/_custom.scss` or `_sass/_variables.scss`.
Use a descriptive commit message, e.g.:

```bash
git add _sass/_custom.scss
git commit -m "fix(css): correct hover color on sidebar icons"
```

If Step 2 passes with no issues, this step is complete — no commit needed.
