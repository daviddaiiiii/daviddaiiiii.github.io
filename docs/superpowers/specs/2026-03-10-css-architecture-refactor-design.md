# CSS Architecture Refactor — Design Spec

**Date:** 2026-03-10
**Status:** Approved

## Goal

Consolidate the dual-CSS-file architecture into a single SCSS compilation chain, eliminate duplicate/conflicting rule definitions, unify all colors under SCSS variables, and update the color scheme.

## Color Scheme

| Role | Variable | Value |
|---|---|---|
| Primary (deep ocean blue) | `$primary-color` | `#0f2744` |
| Primary light | `$primary-light` | `#1a3a5c` |
| Primary lighter | `$primary-lighter` | `#2a5278` |
| Primary dark | `$primary-dark` | `#091c30` |
| Accent (forest green) | `$accent-color` | `#1e6b52` |
| Accent light | `$accent-light` | `#27a077` |
| Accent lighter | `$accent-lighter` | `#3dbf8f` |
| Accent dark | `$accent-dark` | `#155040` |

## File Changes

| Action | File |
|---|---|
| Delete | `assets/css/accent-enhancements.css` |
| Create | `_sass/_custom.scss` |
| Modify | `assets/css/main.scss` — add `@import "custom"`, remove inline custom style blocks |
| Modify | `_sass/_variables.scss` — replace primary/accent color values |
| Modify | `_sass/_base.scss` — update `:root {}` CSS variables to match |
| Check | `_includes/head.html` — remove any direct reference to `accent-enhancements.css` |

## Duplicate Rule Resolution

Keep the `accent-enhancements.css` version in all conflicts (more refined), delete `main.scss` version:

- `.filter-btn` — keep square-rounded + deep-blue active state; remove pill + purple gradient
- `.filter-btn.active` — keep `$primary-color`; remove `#a18cd1 → #fbc2eb`
- `.inner-tag-badge` — keep tag-author / tag-metric classified version
- `.inner-tag-badge.active` — keep deep-blue; remove purple gradient
- `.paper-box` — keep base flex layout in `main.scss`; move visual styles (border, shadow, hover) to `_custom.scss`

## `_custom.scss` Structure

```
1. CSS Variables (:root overrides)
2. Typography & Text Utilities (.accent-text, .primary-gradient-text, .link-accent)
3. Buttons (.btn-accent)
4. Cards & Floating Panels (.floating-card, .highlight-block, .quote-accent)
5. Publication Cards (.paper-box visual styles)
6. Publication Filter (#filter-container, .filter-btn)
7. Tag Badges (.inner-tag-badge, .tag-author, .tag-metric)
8. Sidebar & Icons
9. Animations & Utilities (.fade-in-up, .pulse-accent, shimmer, etc.)
10. Responsive Overrides
```

All hardcoded hex values replaced with SCSS variables. Colors managed exclusively in `_variables.scss`.
