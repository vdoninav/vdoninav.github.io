# Site Redesign — Design Spec

**Date:** 2026-04-21
**Scope:** Full visual redesign of `index.html` for vdonin.com. Single-file static GitHub Pages site.

## Goal

Replace the current plain layout with a modern "bold gradient" visual language while preserving:

- The single-file HTML + inline-CSS deployment model (no build, no npm, no JS framework).
- The GitHub Pages deploy path (push to `master`).
- All existing content, with the specific deletions/edits enumerated under **Content Changes** below.

No loss of information, no feature additions, no infrastructure changes.

## Design Direction

**Aesthetic:** Bold Gradient — white canvas with soft radial gradient blobs (purple / cyan / pink), heavy-weight gradient typography for display elements, rounded corners, subtle shadows. Content-forward, not decorative.

**Tone:** Confident, professional, tech-native. No emojis in production copy. Strict, minimal bullet treatments.

**Accessibility / robustness:**
- All iconography is inline SVG — no external requests, no web-font dependencies.
- Uses system font stack (matches current site), so the page renders identically offline.
- Honors `prefers-reduced-motion` — disables hover transforms when the user has requested reduced motion.
- Responsive down to 375px-wide viewports. Breakpoints at 800px (desktop → tablet) and 520px (tablet → mobile).

**No dark mode.** Light theme only, for single-file simplicity.

## Color System

```
Primary gradient:   linear-gradient(135deg, #7c3aed 0%, #2563eb 45%, #0891b2 100%)
Accent soft-blobs:  #a78bfa (purple), #22d3ee (cyan), #f0abfc (pink) — radial, ~50% opacity, blurred
Canvas:             #ffffff (hero), #fafbfc (sections)
Header/footer:      #0d1117 (header uses rgba(13,17,23,.92) + backdrop-blur for sticky)
Text strong:        #0f172a
Text body:          #334155
Text muted:         #475569 / #64748b
Borders:            #e2e8f0
Surface (chips):    #f1f5f9
Featured card:      linear-gradient(135deg, #1e1b4b 0%, #0c4a6e 60%, #164e63 100%)
```

A single utility class `.grad-text` applies the primary gradient to text via `-webkit-background-clip: text`.

## Typography

- **Stack:** unchanged from current site — `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif`.
- **Display (hero role):** `5rem / 800 / -0.04em` desktop, `2.8rem` mobile.
- **Section headings (h2):** `1.75rem / 700 / -0.02em`.
- **Card titles (h4):** `1.02rem / 700`, explicit color `#0f172a`.
- **Category labels (uppercase):** `0.75rem / 700 / letter-spacing 0.08em`, color `#7c3aed`.
- **Body:** `1rem / 1.65` for prose, `0.88rem` for card bodies.

## Content Changes

These are explicit edits requested during brainstorming. Preserve everything else verbatim.

1. **New current position** (to be featured in the dark-gradient hero card in Опыт работы):
   - Title: `Разработчик-исследователь в команде Машинного обучения и Антиспама`
   - Body: `Занимаюсь полным циклом выкатки моделей - от сбора датасета до обучения и инференса. Разрабатываю архитектуры, подбираю подходящие решения под них, обсуждаю и внедряю передовые технологии`
   - Badge: `Текущая позиция` (with pulse dot)
   - Single CTA link: "Связаться со мной" → `https://t.me/vdoninav`

2. **Previous position** (demoted from current — was the old Yandex Alice card):
   - Title: `Аналитик-разработчик в команде этики Яндекс Алисы`
   - Subtitle: changes from `Текущая позиция` → `Предыдущая позиция`
   - Body bullets: unchanged (4 items), NDA disclaimer unchanged.

3. **Hero copy:** drop `Аналитик-разработчик +` from the role line. Role is now only `ML-engineer`. Retain name "Алексей Вдонин" as a small preamble above the role, in the same gradient as "ML-engineer".

4. **Skills — drop the following items** (user-requested removals):
   - From ML / DL: `Анализ данных`, `Алгоритмы и структуры данных`
   - From Инфраструктура: `Remote Servers`

5. **Footer year:** `2025` → `2026`.

## Information Architecture

Single-page scroll, anchor-driven nav (same pattern as today). Sections in order:

1. **Header** (sticky)
2. **Hero** (no anchor)
3. **About / Обо мне** — informally co-located with hero, not a nav target
4. **Skills** — `#skills` — nav label "Навыки"
5. **Education** — `#education` — nav label "Образование"
6. **Work + Projects** — `#work` — nav label "Деятельность" (one nav item, two visually-distinct sub-sections inside)
7. **Contacts** — `#contact` — nav label "Контакты"
8. **Footer**

## Section-by-Section Design

### Header (sticky)

- Dark bar `rgba(13,17,23,.92)` with `backdrop-filter: blur(10px)` — `position: sticky; top: 0`.
- Left: logotype `Vdonin.com`, where `Vdonin` is italicized and uses `.grad-text` (existing pattern, carried over).
- Right: nav `<ul>` of 4 links (Навыки, Образование, Деятельность, Контакты).
- Active-section indicator: 2px purple underline under the current section's link, driven by scrollspy (see below).
- Mobile (< 800px): padding tightens; nav wraps or uses smaller font size. No hamburger — the 4 links fit.

### Hero

- Centered layout, `~4rem` vertical padding desktop, `~3rem` mobile.
- Three decorative gradient blobs (purple top-right, cyan bottom-left, pink center) — absolutely positioned, blurred, low-opacity, non-interactive (`pointer-events: none`).
- Stack (top to bottom):
  - Eyebrow: `Привет` — uppercase, letter-spaced, slate `#64748b`.
  - Name line: `Меня зовут <gradient>Алексей Вдонин</gradient>, я —` (connector words in slate, name in gradient).
  - Display: `ML-engineer` — `5rem` gradient, the hero statement.
  - Greeting: `Рад видеть Вас на моём личном сайте` — muted slate, max-width 480px.
  - CTA button: `Связаться со мной →` — dark fill `#0f172a`, white text, `10px` radius, subtle shadow. Links to `https://t.me/vdoninav`.

### About / Обо мне

- Two-column grid: photo (left) + text (right) on desktop; stacks on mobile with center alignment.
- Photo: `main_photo.jpg` shown at 240×240px circle, `object-position: 35% center` (preserve the current crop), wrapped in an 8px-padded gradient ring (subtle purple→cyan).
- Text:
  - `<h2>` "Обо мне"
  - Unchanged paragraph: *"Я занимаюсь аналитикой и ML-разработкой: пишу SQL-запросы, код, обучаю классификаторы, оцениваю их качество и эффект от их внедрения. Независимо от задачи, я всегда стремлюсь найти оптимальное решение как с точки зрения эффективности, так и алгоритмической сложности."*
  - Skill-tag chip row below the paragraph (visual bridge to Skills section): `Python · C++`, `Classic ML · DL`, `NLP · CV`, `SQL · YTsaurus`. Four chips, `#fff` background, `1px solid #e2e8f0` border, `999px` radius.

### Skills / Навыки

- Section header pattern (reused across every section):
  - `<h2>` title + small `.count` pill (e.g., "4 категории") + flex-1 fading rule `linear-gradient(90deg, #e2e8f0, transparent)`.
- Grid of 4 category cards, 2×2 desktop, 1-col mobile.
- Each category card:
  - White background, `1px solid #e2e8f0`, `12px` radius, `~1.1rem` padding.
  - Label row: inline SVG icon (18×18, purple stroke) + uppercase category name in purple.
  - Row of pill chips `.pill` — `#f1f5f9` fill, `1px solid #e2e8f0` border, `8px` radius, `0.82rem` text.
- Four categories and their items (after deletions):
  - **Языки программирования** — Python, C++
  - **ML / DL** — Classic ML, DL (NLP, CV)
  - **Базы данных** — SQL, YTsaurus, PostgreSQL
  - **Инфраструктура** — CI/CD (GitLab, Proprietary), Linux, CLI
- SVG icons: code brackets, neural-graph dots, database cylinder, server stack. All same 18×18 box, 2px stroke — no unicode glyphs (they rendered inconsistently).

### Education / Образование

Two stacked cards.

**Main card** — HSE credentials:
- Background: `linear-gradient(135deg, #eef2ff 0%, #ecfeff 100%)`, `1px solid #c7d2fe`, `14px` radius.
- Three-column grid: emblem | text | dates pill.
- Emblem: 56×56 rounded square, primary gradient background, white "ВШЭ" label.
- Text: `<h4>` "НИУ ВШЭ, ФКН — Прикладная математика и информатика" + subtitle line "г. Москва · специализация «Машинное обучение и приложения» · Бакалавриат".
- Dates pill: `2022 — 2026`, white semi-transparent bg, purple border.

**Awards card** — Достижения:
- White card, same border/radius as skill cards.
- Category label "Достижения" with a five-point star SVG icon (purple stroke).
- 2-column `<ul>` (1-col on mobile).
- Bullets: **no emojis**. Each `<li>` prefixed by a 7×7 `border-radius: 2px` square with the primary gradient background.
- Four items, unchanged content:
  - Участник ICPC
  - Участник заключительного этапа ВСОШ по физике
  - Золотой медалист IEPhO (Международная олимпиада по экспериментальной физике)
  - Призёр множества олимпиад по физике и математике

### Work + Projects / Деятельность (#work)

Two labelled sub-sections sharing one nav anchor.

**Опыт работы (3)**

- Featured **dark-gradient hero card** (current position, full-width):
  - Background: `linear-gradient(135deg, #1e1b4b 0%, #0c4a6e 60%, #164e63 100%)`, plus two decorative radial blobs (purple top-right, cyan bottom-left) inside the card.
  - `.badge` pill — frosted glass (`rgba(255,255,255,.14)` + `backdrop-filter: blur(10px)` + thin white border) — reads `● Текущая позиция` with a small pulsing green dot (`#6ee7b7` with expanded shadow).
  - `<h3>` title, white text: "Разработчик-исследователь в команде Машинного обучения и Антиспама".
  - Body paragraph in `rgba(255,255,255,.9)`: "Занимаюсь полным циклом выкатки моделей - от сбора датасета до обучения и инференса. Разрабатываю архитектуры, подбираю подходящие решения под них, обсуждаю и внедряю передовые технологии."
  - Primary-style CTA link: "Связаться со мной →" — white fill, dark `#1e1b4b` text — links to `https://t.me/vdoninav`.
- Below: 2-column grid of **regular work cards** (previous positions):
  - Regular card style: white background, `1px solid #e2e8f0`, `12px` radius, 3px-wide gradient bar on the left edge. Title `#0f172a` weight 700 (the contrast fix from the prior iteration). Purple italic sub-title. Slate body `<ul>`.
  - Card 1 — Аналитик-разработчик в команде этики Яндекс Алисы
    - Sub: "Предыдущая позиция"
    - 4 bullets (verbatim from current site).
    - NDA disclaimer at bottom (italic, small, muted): "*Все задачи описаны максимально абстрактно для строгого соблюдения NDA."
  - Card 2 — Стажировка в группе исследования новых методов ИБ — Яндекс Такси
    - Sub: "Первый индустриальный опыт"
    - 3 bullets (verbatim from current site).

**Проекты (4)**

- Regular-card grid, 2×2 desktop, 1-col mobile. Same regular-card style as work cards but with a cyan→purple left-edge bar variant (different direction) to differentiate subtly.
- Four cards (verbatim content from current site):
  - **Распознавание и суммаризация документов — HSE App X** — sub "Один из лучших проектов ПМИ 2025", 3 bullets, links: GitHub, HSE Best Project, Report.
  - **Исследование уголовных дел — NER** — 2 bullets, links: GitHub, WebApp.
  - **Real Estate Analysis** — 2 bullets, links: GitHub, WebApp.
  - **Расширение «Словарь ударений»** — 2 bullets, links: GitHub.
- Link presentation: small pill buttons, `#f1f5f9` fill, `1px solid #e2e8f0`, `0.76rem` text, `#0f172a` label. Render the same existing URLs from the current site — no link-target changes.

### Contacts / Контакты

- 4-column grid desktop, 2-col tablet (< 800px), 1-col phone (< 520px).
- Each card is an `<a>` element — white bg, `1px solid #e2e8f0`, `12px` radius, lift-on-hover (`translateY(-3px)` + shadow), top-right ↗ that slides up-right on hover.
- Icon well: 42×42 `#f8fafc` rounded square with the real brand SVG inside at 22×22.
- Card stack: icon | uppercase `.label` in muted color | `.value` in strong `#0f172a`.
- The four cards (unchanged targets):
  - **Email** — `aleksei@vdonin.com` — generic outlined envelope SVG (Feather-style), currentColor `#0f172a`.
  - **Telegram** — `@vdoninav` — official Telegram mark SVG (simple-icons path), fill `#229ED9`.
  - **GitHub** — `github.com/vdoninav` — official GitHub Mark SVG (simple-icons), fill `#181717`.
  - **LinkedIn** — `linkedin.com/in/vdoninav` — official LinkedIn SVG (simple-icons), fill `#0A66C2`.

All SVG `<path>` data is inlined verbatim in the HTML; no external assets.

### Footer

- Full-width dark bar `#0d1117`.
- Flex layout: copyright left (`© Вдонин А.В., 2026. Все права защищены.`), small `Vdonin.com` wordmark right with the italic-gradient "Vdonin" treatment. Centers on narrow viewports.

## Interactions

Kept minimal and CSS-only where possible.

1. **Scrollspy for active nav link.** Tiny IntersectionObserver script (~15 lines) watches the 4 sections; adds `.active` class to the corresponding nav `<a>`. This is the only JavaScript on the page.
2. **Hover lift** on project cards and contact cards: `transform: translateY(-3px)` + softer drop shadow, 180ms ease.
3. **Pulse dot** on the featured card badge — pure CSS keyframe (`box-shadow` expand/fade), 2s loop.
4. **Smooth scroll** for anchor links via `html { scroll-behavior: smooth; }` plus `scroll-margin-top` on each `<section>` to offset the sticky header.
5. **Reduced motion**: `@media (prefers-reduced-motion: reduce)` disables hover transforms and the pulse animation.

## File Deliverables

Single modified file + two metadata updates. No new files in the deployable site.

- **`index.html`** — full replacement:
  - `<head>`: unchanged favicon/manifest links, unchanged title, unchanged charset/viewport meta.
  - `<style>`: new stylesheet (estimate ~500–700 lines, single `<style>` block, same pattern as today).
  - `<body>`: new markup per the sections above.
  - One small `<script>` block at the end of `<body>` for the scrollspy observer.
  - All SVG icons inline.
- **`RELEASE_NOTES.md`** — prepend a new top section "v2.0 — Redesign" summarizing aesthetic + content changes (new current position, skill deletions, footer year, layout overhaul).
- **`CLAUDE.md`** — minor update: the project-card structure and content-conventions notes should reflect the two-section split (Опыт работы / Проекты) and the gradient-accent language. The NDA disclaimer note is still valid (now on the "Предыдущая позиция" card).

No changes to: `CNAME`, `thumbs/**`, `main_photo.jpg`, `README.md`, `.gitignore` (already contains `.superpowers/`).

## Out of Scope

- No build pipeline, no bundler, no minifier (keeps the zero-dependency deploy).
- No web fonts (Inter, Geist, etc.) — system stack is sufficient.
- No dark mode / theme toggle.
- No JavaScript framework. The only JS is the ~15-line scrollspy.
- No CMS, no content management layer — content stays inline.
- No changes to external project URLs.

## Verification / Acceptance

Before calling the work done:

1. `index.html` renders at 1440px, 1024px, 768px, 414px, 375px widths without layout breakage.
2. Every item of content listed in the current `index.html` is either present in the new file, or is one of the explicit deletions in **Content Changes**. No silent losses.
3. All external links (GitHub, HSE, Streamlit apps, Telegram, email, LinkedIn) open to the same targets as today.
4. Photo (`main_photo.jpg`) loads and is cropped correctly.
5. Keyboard navigation (Tab) cycles through nav, CTAs, contact cards, and project links in logical order. Visible focus ring.
6. Open `index.html` directly as `file://` — site must render (no external font/CSS/JS dependencies).
7. Push to `master` — GitHub Pages serves the new site at `vdonin.com`.
