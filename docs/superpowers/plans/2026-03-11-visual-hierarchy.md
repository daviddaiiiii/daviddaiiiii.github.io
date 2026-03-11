# Visual Hierarchy Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add hero area, styled section headers, and vertical timeline to the academic homepage.

**Architecture:** Pure CSS + HTML changes — no JS needed. CSS goes into `_sass/_custom.scss` (existing custom styles file). HTML structure is added/replaced in `_pages/about.md`. Jekyll SCSS compilation picks up changes automatically.

**Tech Stack:** Jekyll, SCSS, Minimal Mistakes theme, DM Serif Display + DM Sans fonts (already loaded).

---

## Chunk 1: CSS — Section Headers

### Task 1: Section header trailing-line style

**Files:**
- Modify: `_sass/_custom.scss` (append after `.accent-border-top::before` block, around line 67)

- [ ] **Step 1: Locate the insertion point**

  Open `_sass/_custom.scss`. Find the comment `// 1. Page Structure & Decorative Elements` (around line 50). The `.accent-border-top::before` block ends around line 67. Insert immediately after it.

- [ ] **Step 2: Add the CSS**

  ```scss
  // Section heading with trailing gradient line
  .page__content h2 {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding-bottom: 0.4rem;
    margin-top: 2rem;

    &::after {
      content: '';
      flex: 1;
      height: 2px;
      background: linear-gradient(90deg, $primary-color 0%, transparent 100%);
      border-radius: 1px;
    }
  }
  ```

- [ ] **Step 3: Build and verify**

  ```bash
  bundle exec jekyll build 2>&1 | tail -5
  ```
  Expected: `done in X seconds` with no SCSS errors.

- [ ] **Step 4: Commit**

  ```bash
  git add _sass/_custom.scss
  git commit -m "feat(style): add trailing gradient line to section h2 headers"
  ```

---

## Chunk 2: CSS — Hero Area

### Task 2: Hero area styles

**Files:**
- Modify: `_sass/_custom.scss` (new section appended at end of file)

- [ ] **Step 1: Add hero CSS block at end of `_sass/_custom.scss`**

  ```scss
  // -------------------------------------------------------------
  // Hero Intro Block
  // -------------------------------------------------------------

  .hero-intro {
    margin-bottom: 2rem;
  }

  .hero-eyebrow {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: $accent-color;
    font-weight: 700;
    margin-bottom: 0.4rem;
  }

  .hero-name {
    font-family: $header-font-family;
    font-size: 2.2rem;
    color: $primary-color;
    font-weight: 400;
    line-height: 1.15;
    margin-bottom: 0.6rem;
  }

  .hero-divider {
    height: 3px;
    width: 48px;
    background: linear-gradient(90deg, $primary-color, $accent-color);
    border-radius: 2px;
    margin: 0.8rem 0;
  }

  .hero-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 0.4rem;
    margin-top: 1rem;
  }

  .hero-pill {
    background: #f0f4fa;
    border: 1px solid #d0dcea;
    border-radius: 99px;
    font-size: 0.75rem;
    padding: 3px 12px;
    color: $primary-color;
    font-weight: 500;
  }
  ```

- [ ] **Step 2: Build and verify no SCSS errors**

  ```bash
  bundle exec jekyll build 2>&1 | tail -5
  ```
  Expected: clean build, no errors.

- [ ] **Step 3: Commit**

  ```bash
  git add _sass/_custom.scss
  git commit -m "feat(style): add hero intro CSS"
  ```

---

## Chunk 3: CSS — Education Timeline

### Task 3: Education timeline styles

**Files:**
- Modify: `_sass/_custom.scss` (append after hero block)

- [ ] **Step 1: Add timeline CSS block**

  ```scss
  // -------------------------------------------------------------
  // Education Timeline
  // -------------------------------------------------------------

  .edu-timeline {
    position: relative;
    padding-left: 1.6rem;
    border-left: 2px solid $primary-color;
    margin: 1rem 0 1.5rem;
  }

  .edu-item {
    position: relative;
    margin-bottom: 1.5rem;

    &:last-child {
      margin-bottom: 0;
    }
  }

  .edu-dot {
    position: absolute;
    left: -2.1rem;
    top: 0.35rem;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: $accent-color;
    border: 2px solid #fff;
    box-shadow: 0 0 0 2px $accent-color;
  }

  .edu-body {
    // wrapper for date/school/degree — flex-ready for future layout changes
  }

  .edu-date {
    font-size: 0.75rem;
    color: $accent-color;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 0.1rem;
  }

  .edu-school {
    font-size: 0.95rem;
    font-weight: 600;
    color: $primary-color;
    margin-bottom: 0.1rem;
  }

  .edu-degree {
    font-size: 0.85rem;
    color: #6b7280;
  }

  @media (max-width: 600px) {
    .edu-timeline {
      padding-left: 1.2rem;
    }

    .edu-dot {
      left: -1.7rem;
      width: 10px;
      height: 10px;
    }
  }
  ```

- [ ] **Step 2: Build and verify no SCSS errors**

  ```bash
  bundle exec jekyll build 2>&1 | tail -5
  ```
  Expected: clean build.

- [ ] **Step 3: Commit**

  ```bash
  git add _sass/_custom.scss
  git commit -m "feat(style): add education timeline CSS with mobile fallback"
  ```

---

## Chunk 4: HTML — Update about.md

### Task 4: Add hero block to about.md

**Files:**
- Modify: `_pages/about.md`

The current intro starts at line 24 with `My name is...`. We insert a `.hero-intro` div before it. The intro paragraph text must use HTML `<strong>` tags instead of Markdown bold (`**...**`) since it sits inside a raw HTML block — Kramdown does not process Markdown inside raw HTML unless `markdown="1"` is set.

- [ ] **Step 1: Insert hero HTML block**

  In `_pages/about.md`, find and replace:

  **Find:**
  ```
  <span class='anchor' id='about-me'></span>
  My name is <span class="accent-text">Yuxing Dai</span> (戴裕兴, you can call me Frank). I'm currently a PhD candidate in Pharmacology at **Sun Yat-sen University (SYSU)**. My research spans translational pharmacology and novel drug target discovery, integrating wet lab experiments with single-cell RNA sequencing and bioinformatics analysis.
  ```

  **Replace with:**
  ```html
  <span class='anchor' id='about-me'></span>

  <div class="hero-intro">
    <div class="hero-eyebrow">PhD Candidate &middot; Sun Yat-sen University &middot; Pharmacology</div>
    <div class="hero-name">Yuxing Dai (Frank)</div>
    <div class="hero-divider"></div>
    <p>My name is <span class="accent-text">Yuxing Dai</span> (戴裕兴, you can call me Frank). I'm currently a PhD candidate in Pharmacology at <strong>Sun Yat-sen University (SYSU)</strong>. My research spans translational pharmacology and novel drug target discovery, integrating wet lab experiments with single-cell RNA sequencing and bioinformatics analysis.</p>
    <div class="hero-pills">
      <span class="hero-pill">Cardiovascular Pharmacology</span>
      <span class="hero-pill">Neuroprotection</span>
      <span class="hero-pill">Drug Discovery</span>
      <span class="hero-pill">Single-cell RNA-seq</span>
      <span class="hero-pill">Deep Learning</span>
    </div>
  </div>
  ```

  > The `quote-accent` block and `highlight-blocks` section remain unchanged, placed after the closing `</div>`.

- [ ] **Step 2: Build and verify**

  ```bash
  bundle exec jekyll build 2>&1 | tail -5
  ```
  Open `http://localhost:4000` and confirm:
  - Eyebrow text (small, uppercase, green) appears above the name
  - Name renders in large DM Serif Display
  - Short gradient divider visible
  - Intro paragraph text renders correctly (no literal asterisks)
  - Research pills appear below intro

### Task 5: Replace education list with timeline HTML

**Files:**
- Modify: `_pages/about.md`

- [ ] **Step 1: Replace the education markdown list**

  **Find:**
  ```markdown
  <span class='anchor' id='-educations'></span>
  # 🏫 Educations
  - *2025.09 - Present*: &nbsp;PhD Candidate in Pharmacology, Sun Yat-sen University (SYSU).
  - *2022.09 - 2025.06*: &nbsp;Master in Pharmacology, Sun Yat-sen University (SYSU).
  - *2018.06 - 2022.09*: &nbsp;Bachelor, Southern Medical University.
  ```

  **Replace with:**
  ```html
  <span class='anchor' id='-educations'></span>

  ## 🏫 Educations

  <div class="edu-timeline">
    <div class="edu-item">
      <div class="edu-dot"></div>
      <div class="edu-body">
        <div class="edu-date">2025.09 – Present</div>
        <div class="edu-school">Sun Yat-sen University (SYSU)</div>
        <div class="edu-degree">PhD Candidate in Pharmacology</div>
      </div>
    </div>
    <div class="edu-item">
      <div class="edu-dot"></div>
      <div class="edu-body">
        <div class="edu-date">2022.09 – 2025.06</div>
        <div class="edu-school">Sun Yat-sen University (SYSU)</div>
        <div class="edu-degree">Master in Pharmacology</div>
      </div>
    </div>
    <div class="edu-item">
      <div class="edu-dot"></div>
      <div class="edu-body">
        <div class="edu-date">2018.06 – 2022.09</div>
        <div class="edu-school">Southern Medical University</div>
        <div class="edu-degree">Bachelor</div>
      </div>
    </div>
  </div>
  ```

- [ ] **Step 2: Update remaining section headings from h1 to h2**

  In `_pages/about.md`, change:
  - `# 📃 Publications` → `## 📃 Publications`
  - `# 🏆 Awards` → `## 🏆 Awards`
  - `# 🛠️ Projects` → `## 🛠️ Projects`

- [ ] **Step 3: Build and verify**

  ```bash
  bundle exec jekyll build 2>&1 | tail -5
  ```
  Open `http://localhost:4000` — confirm:
  - Education section shows vertical timeline with accent dots and left border
  - All four section headings (Educations, Publications, Awards, Projects) have trailing gradient lines
  - No layout breakage on publications / awards / projects

- [ ] **Step 4: Commit**

  ```bash
  git add _pages/about.md
  git commit -m "feat(content): hero intro block + education timeline + h2 section headers"
  ```

---

## Chunk 5: Final Visual Check

- [ ] **Serve locally and do full visual pass**

  ```bash
  bundle exec jekyll serve --livereload
  ```
  At `http://localhost:4000`, verify:
  1. Hero area: eyebrow / large name / divider / pills visible and correctly styled
  2. All section headings have a trailing gradient line
  3. Education timeline: left border, dots, date/school/degree layout correct
  4. No regressions: publications filter, paper-box cards, awards, projects all render normally

- [ ] **Mobile check** — resize browser to < 600px and confirm:
  - Timeline left border and dots stay inside the content column (no overflow)
  - Dot `left` offset reduced by the media query (10px dot, `-1.7rem` offset)
  - Hero pills wrap correctly without horizontal scroll

  If the dots still clip outside the viewport, add an additional CSS fix:
  ```scss
  @media (max-width: 400px) {
    .edu-timeline {
      padding-left: 1rem;
      border-left-width: 2px;
    }
    .edu-dot {
      left: -1.4rem;
    }
  }
  ```

- [ ] **Commit any mobile fixes**

  ```bash
  git add _sass/_custom.scss
  git commit -m "fix(style): timeline mobile overflow adjustments"
  ```
