# Visual Hierarchy Design — Item 3

**Date:** 2026-03-11
**Scope:** Hero area, section header design, education timeline
**Files affected:** `_pages/about.md`, `_sass/_custom.scss`

---

## 1. Hero Area (About Me top)

Insert a hero block **before** the existing intro paragraph. Structure:

```
PhD Candidate · SYSU · Pharmacology   ← eyebrow: small, uppercase, accent green (#1e6b52)
Yuxing Dai (Frank)                    ← DM Serif Display, ~2.2rem, primary color
────────────────                      ← 48px gradient divider (primary→accent)
[existing intro paragraph]
[Cardiovascular] [Neuroprotection] [scRNA-seq] [Drug Discovery] [Deep Learning]
                                      ← research pill tags
```

Implementation:
- Add `.hero-intro` wrapper div in `about.md`
- `.hero-eyebrow`: `font-size: 0.75rem`, `text-transform: uppercase`, `letter-spacing: 0.1em`, `color: $accent-color`, `font-weight: 700`
- `.hero-name`: `font-family: $header-font-family`, `font-size: 2.2rem`, `color: $primary-color`
- `.hero-divider`: `height: 3px`, `width: 48px`, `background: linear-gradient(90deg, $primary-color, $accent-color)`, `border-radius: 2px`, margin `0.8rem 0`
- `.hero-pills`: flex wrap, gap `0.4rem`
- `.hero-pill`: `background: #f0f4fa`, `border: 1px solid #d0dcea`, `border-radius: 99px`, `font-size: 0.75rem`, `padding: 3px 12px`, `color: $primary-color`

## 2. Section Header Design

All `h2` elements on the page (generated from `# 🏫 Educations` etc.) get a trailing gradient line via CSS `::after`:

```css
.page__content h2 {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding-bottom: 0.4rem;
}
.page__content h2::after {
  content: '';
  flex: 1;
  height: 2px;
  background: linear-gradient(90deg, $primary-color 0%, transparent 100%);
  border-radius: 1px;
}
```

This is pure CSS, no HTML changes to `about.md`.

## 3. Education Timeline

Replace the markdown list in the `#-educations` section with a custom HTML timeline div.

Structure per entry:
```html
<div class="edu-timeline">
  <div class="edu-item">
    <div class="edu-dot"></div>
    <div class="edu-body">
      <div class="edu-date">2025 – Present</div>
      <div class="edu-school">Sun Yat-sen University (SYSU)</div>
      <div class="edu-degree">PhD Candidate in Pharmacology</div>
    </div>
  </div>
  ...
</div>
```

CSS:
- `.edu-timeline`: `position: relative`, `padding-left: 1.5rem`, `border-left: 2px solid $primary-color`
- `.edu-item`: `position: relative`, `margin-bottom: 1.5rem`
- `.edu-dot`: `position: absolute`, `left: -1.9rem`, `top: 0.35rem`, `width: 12px`, `height: 12px`, `border-radius: 50%`, `background: $accent-color`, `border: 2px solid #fff`, `box-shadow: 0 0 0 2px $accent-color`
- `.edu-date`: `font-size: 0.75rem`, `color: $accent-color`, `font-weight: 700`, `text-transform: uppercase`, `letter-spacing: 0.05em`
- `.edu-school`: `font-size: 0.95rem`, `font-weight: 600`, `color: $primary-color`
- `.edu-degree`: `font-size: 0.85rem`, `color: #6b7280`

---

## Implementation Order

1. Add CSS to `_sass/_custom.scss` (hero styles, section h2 style, timeline styles)
2. Update `_pages/about.md`: add hero block, replace education list with timeline HTML
