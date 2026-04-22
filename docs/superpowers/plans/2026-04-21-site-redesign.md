# Site Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace `index.html` with the "bold gradient" redesign described in `docs/superpowers/specs/2026-04-21-site-redesign-design.md`, preserving every piece of existing content except the explicit edits enumerated in the spec.

**Architecture:** Single HTML file with one inline `<style>` block and one tiny `<script>` (scrollspy). No build system, no external fonts, no external CSS. All SVG icons inlined. Sections are assembled incrementally so every commit leaves the site in a working state.

**Tech Stack:** HTML5, CSS3 (grid, flexbox, backdrop-filter, gradient text via `-webkit-background-clip`), one IntersectionObserver-based scrollspy in vanilla JS. Deploys via GitHub Pages on push to `master`.

**Verification model:** There is no test framework in this repo and we're not introducing one (single-file static site, YAGNI). Each task has a concrete manual verification step: open the file in a browser and check specific, named conditions. Content fidelity is verified against the snapshot below.

---

## Reference: Content snapshot of current `index.html` (pre-change)

Use this to verify no content is silently lost. Every item below must appear in the new file unless listed in **Explicit Deletions**.

**Nav links:** Навыки (`#skills`), Образование (`#education`), Деятельность (`#projects`), Контакты (`#contact`). (Note: current site has `<!-- <li><a href="#about">Обо мне</a></li> -->` commented out — stays commented out / omitted.)

**Hero H2:** "Привет, я Алексей Вдонин"
**Hero sub:** "Аналитик-разработчик + ML-engineer / Рад видеть Вас на моём личном сайте!"

**About paragraph:** *"Я занимаюсь аналитикой и ML-разработкой: пишу SQL-запросы, код, обучаю классификаторы, оцениваю их качество и эффект от их внедрения. Независимо от задачи, я всегда стремлюсь найти оптимальное решение как с точки зрения эффективности, так и алгоритмической сложности."*

**Skills list (current):** Python/C++, SQL, Classic ML/DL (NLP, CV), Анализ данных, Алгоритмы и структуры данных, CI/CD (GitLab, Proprietary Solutions), YTsaurus/PostgreSQL, Linux/CLI/Remote Servers.

**Education list:**
- г. Москва, НИУ ВШЭ, ФКН, Прикладная математика и информатика, специализация Машинное обучение и приложения
- Бакалавриат, 2022-2026
- Участник ICPC
- Участник заключительного этапа ВСОШ по физике
- Золотой медалист Международной олимпиады по экспериментальной физике (IEPhO)
- Призер множества олимпиад по физике и математике

**Projects section (current — 6 cards, heading "Опыт работы + Проекты"):**
1. **Аналитик-разработчик в команде этики Яндекс Алисы** — "Текущая позиция" — 4 bullets + NDA note + Telegram CTA "Связаться со мной" → `https://t.me/vdoninav`
2. **Распознавание и суммаризация документов в мобильном приложении HSE App X** — "Один из лучших проектов ПМИ 2025" — 3 bullets + links: GitHub `https://github.com/vdoninav/hse_ocr_summ`, HSE Best Project `https://cs.hse.ru/cppr/best_projects/hse_app_document_ai`, Report `https://drive.google.com/file/d/15s1vvJh0rRfqZ_qWpkAo9C-Rkl1l--Nh/view`
3. **Стажировка в группе исследования новых методов ИБ в Яндекс Такси** — "Первый индустриальный опыт" — 3 bullets
4. **Исследование уголовных дел с помощью NER** — 2 bullets + links: GitHub `https://github.com/vdoninav/hse_criminal_cases/`, WebApp `https://hse-criminal-cases.streamlit.app/`
5. **Real Estate Analysis** — 2 bullets + links: GitHub `https://github.com/vdoninav/real_estate_analysis?tab=readme-ov-file`, WebApp `https://vdoninav-real-estate-analysis.streamlit.app/`
6. **Расширение «Словарь ударений»** — 2 bullets + link: GitHub `https://github.com/vdoninav/stress_spotter_extension`

**Contacts:**
- Email `aleksei@vdonin.com` → `mailto:aleksei@vdonin.com`
- GitHub `github.com/vdoninav` → `https://github.com/vdoninav`
- Telegram `@vdoninav` → `https://t.me/vdoninav`
- LinkedIn `linkedin.com/in/vdoninav/` → `https://www.linkedin.com/in/vdoninav/`

**Footer:** "© Вдонин А.В., 2025. Все права защищены."

**Explicit Deletions (per spec, do NOT carry over):**
- From Skills: `Анализ данных`, `Алгоритмы и структуры данных`, `Remote Servers`.
- From Hero: `Аналитик-разработчик +` prefix to role.

**Explicit Edits (per spec):**
- Add new current position card: `Разработчик-исследователь в команде Машинного обучения и Антиспама` with body "Занимаюсь полным циклом выкатки моделей - от сбора датасета до обучения и инференса. Разрабатываю архитектуры, подбираю подходящие решения под них, обсуждаю и внедряю передовые технологии". CTA: Telegram.
- Demote old Яндекс Алиса card from "Текущая позиция" → "Предыдущая позиция". Content otherwise unchanged.
- Footer year 2025 → 2026.

---

## File Structure

One file changes in the deployable site; two metadata files updated.

- **Modify: `index.html`** — complete replacement in Task 1, then section-by-section additions.
- **Modify: `RELEASE_NOTES.md`** — prepend v2.0 entry (Task 12).
- **Modify: `CLAUDE.md`** — update card-structure notes (Task 12).

No new files in the deployable site.

---

## Task 1: Skeleton with base CSS and shared utilities

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html` (full replacement)

- [ ] **Step 1: Replace `index.html` with the skeleton**

Use Write to replace the file with:

```html
<!DOCTYPE html>
<html lang="ru">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="apple-touch-icon" sizes="180x180" href="thumbs/apple-touch-icon.png">
    <link rel="icon" type="image/png" sizes="32x32" href="thumbs/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="thumbs/favicon-16x16.png">
    <link rel="manifest" href="thumbs/site.webmanifest">
    <title>Aleksei Vdonin</title>
    <style>
        /* ===== Reset ===== */
        *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

        /* ===== Base ===== */
        html { scroll-behavior: smooth; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
                Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
            line-height: 1.6;
            color: #0f172a;
            background: #ffffff;
            -webkit-font-smoothing: antialiased;
        }
        a { color: inherit; text-decoration: none; }
        ul { list-style: none; }
        img { display: block; max-width: 100%; }
        section { scroll-margin-top: 72px; }

        /* ===== Shared utility: gradient text ===== */
        .grad-text {
            background: linear-gradient(135deg, #7c3aed 0%, #2563eb 45%, #0891b2 100%);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        /* ===== Section headers (reused across sections) ===== */
        .sec { padding: 3rem 0; }
        .sec-head { display: flex; align-items: baseline; gap: 12px; margin-bottom: 1.25rem; }
        .sec-head h2 { font-size: 1.75rem; color: #0f172a; letter-spacing: -0.02em; font-weight: 700; }
        .sec-head .count {
            font-size: 0.7rem; font-weight: 700; color: #64748b;
            background: #f1f5f9; padding: 3px 10px; border-radius: 999px;
            letter-spacing: 0.05em;
        }
        .sec-head .rule { flex: 1; height: 1px; background: linear-gradient(90deg, #e2e8f0, transparent); }

        /* ===== Container ===== */
        .container { max-width: 1100px; margin: 0 auto; padding: 0 2rem; }

        /* ===== Main backdrop below hero ===== */
        main { background: #fafbfc; }

        /* ===== Reduced-motion ===== */
        @media (prefers-reduced-motion: reduce) {
            html { scroll-behavior: auto; }
            *, *::before, *::after { animation-duration: 0.001ms !important; transition-duration: 0.001ms !important; }
        }

        /* ===== Responsive breakpoints ===== */
        @media (max-width: 800px) {
            .container { padding: 0 1.25rem; }
            .sec { padding: 2rem 0; }
            .sec-head h2 { font-size: 1.4rem; }
        }
    </style>
</head>

<body>
    <!-- Header goes in Task 2 -->
    <!-- Hero goes in Task 3 -->
    <!-- Main with sections goes in Tasks 4-9 -->
    <!-- Footer goes in Task 10 -->
    <!-- Scrollspy script goes in Task 11 -->
</body>

</html>
```

- [ ] **Step 2: Verify it renders**

Open `index.html` in a browser (double-click the file). Expected:
- Browser tab shows "Aleksei Vdonin"
- Favicon loads from `thumbs/favicon-32x32.png`
- Page is blank with white background
- No console errors

- [ ] **Step 3: Commit**

```bash
cd /Users/vdav/Documents/vdoninav.github.io
git add index.html
git commit -m "Skeleton + base CSS for redesign"
```

---

## Task 2: Sticky dark header

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html` — add `<header>` after `<body>` tag, add header CSS before `</style>`

- [ ] **Step 1: Append header CSS just before `</style>`**

Use Edit to add these rules immediately before `/* ===== Responsive breakpoints ===== */`:

```css
/* ===== Header (sticky) ===== */
.site-header {
    position: sticky; top: 0; z-index: 50;
    background: rgba(13, 17, 23, 0.92);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    color: #fff;
    border-bottom: 1px solid rgba(255,255,255,0.08);
}
.site-header .container {
    display: flex; justify-content: space-between; align-items: center;
    padding-top: 0.9rem; padding-bottom: 0.9rem;
}
.site-header .logotype { font-size: 1.2rem; font-weight: 500; }
.site-header .logotype em { font-style: italic; }
.site-header nav ul { display: flex; gap: 1.25rem; font-size: 0.9rem; font-weight: 500; }
.site-header nav a { color: #cbd5e1; transition: color 0.15s, border-color 0.15s; padding-bottom: 2px; border-bottom: 2px solid transparent; }
.site-header nav a:hover { color: #fff; }
.site-header nav a.active { color: #fff; border-bottom-color: #7c3aed; }
@media (max-width: 800px) {
    .site-header .container { padding-top: 0.75rem; padding-bottom: 0.75rem; }
    .site-header nav ul { gap: 0.9rem; font-size: 0.78rem; }
    .site-header .logotype { font-size: 1rem; }
}
```

- [ ] **Step 2: Add the `<header>` element inside `<body>`**

Use Edit to replace `<!-- Header goes in Task 2 -->` with:

```html
    <header class="site-header">
        <div class="container">
            <div class="logotype"><em class="grad-text">Vdonin</em>.com</div>
            <nav>
                <ul>
                    <li><a href="#skills">Навыки</a></li>
                    <li><a href="#education">Образование</a></li>
                    <li><a href="#projects">Деятельность</a></li>
                    <li><a href="#contact">Контакты</a></li>
                </ul>
            </nav>
        </div>
    </header>
```

- [ ] **Step 3: Verify**

Reload `index.html`. Expected:
- Dark bar across the top, `Vdonin.com` on the left (italic "Vdonin" in purple→cyan gradient)
- Four nav links on the right: Навыки / Образование / Деятельность / Контакты
- Clicking a link does nothing yet (anchors don't exist) — OK at this stage
- Resize browser narrower than 800px: nav text shrinks but still fits on one line

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add sticky dark header with gradient wordmark"
```

---

## Task 3: Hero section

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Append hero CSS before `/* ===== Responsive breakpoints ===== */`**

```css
/* ===== Hero ===== */
.hero {
    position: relative; overflow: hidden;
    padding: 4rem 0 4.5rem;
    text-align: center;
    background: #ffffff;
}
.hero .blob { position: absolute; border-radius: 50%; pointer-events: none; filter: blur(6px); }
.hero .blob-purple { width: 420px; height: 420px; background: radial-gradient(circle, #a78bfa 0%, transparent 70%); top: -140px; right: -120px; opacity: 0.45; }
.hero .blob-cyan { width: 360px; height: 360px; background: radial-gradient(circle, #22d3ee 0%, transparent 70%); bottom: -160px; left: -120px; opacity: 0.35; }
.hero .blob-pink { width: 260px; height: 260px; background: radial-gradient(circle, #f0abfc 0%, transparent 70%); top: 30%; left: 40%; opacity: 0.18; filter: blur(4px); }
.hero .container { position: relative; z-index: 2; }
.hero .eyebrow {
    font-size: 0.82rem; color: #64748b; letter-spacing: 0.14em;
    text-transform: uppercase; margin-bottom: 0.75rem; font-weight: 600;
}
.hero .name-line {
    font-size: 1.15rem; font-weight: 500; color: #475569;
    margin-bottom: 0.5rem;
}
.hero .name-line .grad-text { font-weight: 600; }
.hero h1.role {
    font-size: 5rem; font-weight: 800; letter-spacing: -0.04em;
    line-height: 1; margin-bottom: 1.25rem;
}
.hero .greeting {
    font-size: 1rem; color: #475569; max-width: 480px;
    margin: 0 auto 1.75rem; line-height: 1.5;
}
.hero .cta {
    display: inline-flex; align-items: center; gap: 8px;
    background: #0f172a; color: #fff;
    border-radius: 10px; padding: 12px 20px;
    font-size: 0.92rem; font-weight: 600;
    box-shadow: 0 8px 24px rgba(15,23,42,0.18);
    transition: transform 0.15s, box-shadow 0.15s;
}
.hero .cta:hover { transform: translateY(-1px); box-shadow: 0 12px 28px rgba(15,23,42,0.22); }
@media (max-width: 800px) {
    .hero { padding: 3rem 0 3.25rem; }
    .hero h1.role { font-size: 2.8rem; }
    .hero .name-line { font-size: 1rem; }
}
```

- [ ] **Step 2: Insert hero markup after `</header>` (replacing `<!-- Hero goes in Task 3 -->`)**

```html
    <section class="hero">
        <div class="blob blob-purple"></div>
        <div class="blob blob-cyan"></div>
        <div class="blob blob-pink"></div>
        <div class="container">
            <div class="eyebrow">Привет</div>
            <p class="name-line">Меня зовут <span class="grad-text">Алексей Вдонин</span>, я&nbsp;—</p>
            <h1 class="role grad-text">ML-engineer</h1>
            <p class="greeting">Рад видеть Вас на моём личном сайте</p>
            <a class="cta" href="https://t.me/vdoninav" target="_blank" rel="noopener">Связаться со мной <span aria-hidden="true">→</span></a>
        </div>
    </section>
```

- [ ] **Step 3: Verify**

Reload. Expected:
- Uppercase "ПРИВЕТ" eyebrow, slate
- "Меня зовут Алексей Вдонин, я —" with name in purple→cyan gradient
- Huge "ML-engineer" in the gradient, dominant on the page
- Greeting line below, then dark "Связаться со мной →" CTA button
- Soft purple blob top-right, cyan blob bottom-left, faint pink blob mid-screen
- Clicking CTA opens Telegram in a new tab
- At 375px width: role text shrinks to `2.8rem`, everything still centered

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add hero section with gradient blobs and ML-engineer display"
```

---

## Task 4: About section

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Append about CSS**

```css
/* ===== About ===== */
.about { padding: 3rem 0; background: #fafbfc; }
.about .container {
    display: grid; grid-template-columns: auto 1fr;
    gap: 2.25rem; align-items: center;
}
.about .photo-ring {
    padding: 8px; border-radius: 50%;
    background: linear-gradient(135deg, rgba(124,58,237,0.35), rgba(8,145,178,0.35));
}
.about .photo {
    width: 240px; height: 240px; border-radius: 50%;
    object-fit: cover; object-position: 35% center;
    box-shadow: 0 10px 30px rgba(15,23,42,0.12);
}
.about h2 { font-size: 1.75rem; margin-bottom: 0.75rem; letter-spacing: -0.02em; }
.about p { color: #334155; line-height: 1.65; max-width: 640px; }
.about .tag-row { margin-top: 1rem; display: flex; gap: 8px; flex-wrap: wrap; }
.about .tag-row .t {
    background: #fff; border: 1px solid #e2e8f0;
    border-radius: 999px; padding: 4px 12px;
    font-size: 0.78rem; color: #475569; font-weight: 500;
}
@media (max-width: 800px) {
    .about .container { grid-template-columns: 1fr; text-align: center; gap: 1.25rem; }
    .about .photo { width: 180px; height: 180px; }
    .about p { margin: 0 auto; }
    .about .tag-row { justify-content: center; }
}
```

- [ ] **Step 2: Open `<main>` and insert the About section**

Replace `<!-- Main with sections goes in Tasks 4-9 -->` with:

```html
    <main>
        <section class="about">
            <div class="container">
                <div class="photo-ring">
                    <img class="photo" src="main_photo.jpg" alt="Алексей Вдонин" />
                </div>
                <div class="about-text">
                    <h2>Обо мне</h2>
                    <p>Я занимаюсь аналитикой и ML-разработкой: пишу SQL-запросы, код, обучаю классификаторы, оцениваю их качество и эффект от их внедрения. Независимо от задачи, я всегда стремлюсь найти оптимальное решение как с точки зрения эффективности, так и алгоритмической сложности.</p>
                    <div class="tag-row">
                        <span class="t">Python · C++</span>
                        <span class="t">Classic ML · DL</span>
                        <span class="t">NLP · CV</span>
                        <span class="t">SQL · YTsaurus</span>
                    </div>
                </div>
            </div>
        </section>
        <!-- Skills section goes in Task 5 -->
        <!-- Education section goes in Task 6 -->
        <!-- Work + Projects section goes in Tasks 7 + 8 -->
        <!-- Contacts section goes in Task 9 -->
    </main>
```

- [ ] **Step 3: Verify**

Reload. Expected:
- Background below the hero turns to light gray-blue (`#fafbfc`)
- Your photo appears on the left, circular, ~240px, with a soft purple→cyan gradient ring around it
- To the right of the photo: "Обо мне" heading + paragraph
- Below the paragraph: four pill chips "Python · C++", "Classic ML · DL", "NLP · CV", "SQL · YTsaurus"
- At < 800px: photo stacks above the text, everything centers

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add About section with gradient-ringed photo and skill chips"
```

---

## Task 5: Skills section

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Append skills CSS**

```css
/* ===== Skills ===== */
.skills-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
.skill-cat {
    background: #fff; border: 1px solid #e2e8f0;
    border-radius: 12px; padding: 1.1rem 1.25rem;
}
.skill-cat .cat-label {
    display: flex; align-items: center; gap: 10px;
    font-size: 0.75rem; font-weight: 700;
    text-transform: uppercase; letter-spacing: 0.08em;
    color: #7c3aed; margin-bottom: 0.85rem;
}
.skill-cat .cat-label svg {
    width: 18px; height: 18px; flex-shrink: 0;
    stroke: #7c3aed; fill: none;
    stroke-width: 2; stroke-linecap: round; stroke-linejoin: round;
}
.skill-pills { display: flex; flex-wrap: wrap; gap: 6px; }
.skill-pills .pill {
    background: #f1f5f9; border: 1px solid #e2e8f0;
    color: #0f172a; padding: 4px 10px;
    border-radius: 8px; font-size: 0.82rem; font-weight: 500;
}
@media (max-width: 520px) { .skills-grid { grid-template-columns: 1fr; } }
```

- [ ] **Step 2: Insert Skills section (replace `<!-- Skills section goes in Task 5 -->`)**

```html
        <section id="skills" class="sec">
            <div class="container">
                <div class="sec-head">
                    <h2>Навыки</h2>
                    <span class="count">4 категории</span>
                    <span class="rule"></span>
                </div>
                <div class="skills-grid">
                    <div class="skill-cat">
                        <div class="cat-label">
                            <svg viewBox="0 0 24 24"><polyline points="8 6 2 12 8 18"/><polyline points="16 6 22 12 16 18"/></svg>
                            <span>Языки программирования</span>
                        </div>
                        <div class="skill-pills">
                            <span class="pill">Python</span>
                            <span class="pill">C++</span>
                        </div>
                    </div>
                    <div class="skill-cat">
                        <div class="cat-label">
                            <svg viewBox="0 0 24 24"><circle cx="6" cy="6" r="2"/><circle cx="6" cy="18" r="2"/><circle cx="18" cy="12" r="2"/><path d="M8 6l8 6M8 18l8-6"/></svg>
                            <span>ML / DL</span>
                        </div>
                        <div class="skill-pills">
                            <span class="pill">Classic ML</span>
                            <span class="pill">DL (NLP, CV)</span>
                        </div>
                    </div>
                    <div class="skill-cat">
                        <div class="cat-label">
                            <svg viewBox="0 0 24 24"><ellipse cx="12" cy="5" rx="9" ry="3"/><path d="M3 5v6c0 1.7 4 3 9 3s9-1.3 9-3V5"/><path d="M3 11v6c0 1.7 4 3 9 3s9-1.3 9-3v-6"/></svg>
                            <span>Базы данных</span>
                        </div>
                        <div class="skill-pills">
                            <span class="pill">SQL</span>
                            <span class="pill">YTsaurus</span>
                            <span class="pill">PostgreSQL</span>
                        </div>
                    </div>
                    <div class="skill-cat">
                        <div class="cat-label">
                            <svg viewBox="0 0 24 24"><rect x="2" y="3" width="20" height="5" rx="1"/><rect x="2" y="10" width="20" height="5" rx="1"/><rect x="2" y="17" width="20" height="5" rx="1"/><circle cx="6" cy="5.5" r=".5" fill="#7c3aed" stroke="none"/><circle cx="6" cy="12.5" r=".5" fill="#7c3aed" stroke="none"/><circle cx="6" cy="19.5" r=".5" fill="#7c3aed" stroke="none"/></svg>
                            <span>Инфраструктура</span>
                        </div>
                        <div class="skill-pills">
                            <span class="pill">CI/CD (GitLab, Proprietary)</span>
                            <span class="pill">Linux</span>
                            <span class="pill">CLI</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>
```

- [ ] **Step 3: Verify**

Reload. Expected:
- Section heading "Навыки" + count pill "4 категории" + fading rule to the right
- Four cards in a 2×2 grid
- Each card has a small purple 18×18 icon, uppercase purple label, and pill chips below
- Icons are visually equal in size (verify no icon is ~30% smaller than others)
- Click a nav link "Навыки" — page scrolls smoothly to this section, and section top is not clipped by the sticky header (thanks to `scroll-margin-top: 72px`)
- Confirm skill list matches spec exactly — NO "Анализ данных", "Алгоритмы и структуры данных", or "Remote Servers"

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add Skills section with 4 category cards"
```

---

## Task 6: Education section

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Append education CSS**

```css
/* ===== Education ===== */
.edu-main {
    background: linear-gradient(135deg, #eef2ff 0%, #ecfeff 100%);
    border: 1px solid #c7d2fe;
    border-radius: 14px;
    padding: 1.5rem 1.75rem;
    margin-bottom: 1rem;
    display: grid;
    grid-template-columns: auto 1fr auto;
    align-items: center;
    gap: 1.25rem;
}
.edu-main .emblem {
    width: 56px; height: 56px; border-radius: 12px;
    background: linear-gradient(135deg, #7c3aed, #0891b2);
    color: #fff; display: flex; align-items: center; justify-content: center;
    font-weight: 800; font-size: 1.1rem; letter-spacing: -0.02em;
}
.edu-main h4 { font-size: 1.05rem; color: #0f172a; margin-bottom: 0.25rem; }
.edu-main p { color: #475569; font-size: 0.88rem; line-height: 1.5; }
.edu-main .dates {
    font-size: 0.78rem; color: #1e1b4b;
    background: rgba(255,255,255,0.7);
    border: 1px solid rgba(124,58,237,0.25);
    padding: 4px 12px; border-radius: 999px;
    font-weight: 600; white-space: nowrap;
}
.edu-awards {
    background: #fff; border: 1px solid #e2e8f0;
    border-radius: 12px; padding: 1.1rem 1.25rem;
}
.edu-awards .cat-label {
    display: flex; align-items: center; gap: 10px;
    font-size: 0.75rem; font-weight: 700;
    text-transform: uppercase; letter-spacing: 0.08em;
    color: #7c3aed; margin-bottom: 0.85rem;
}
.edu-awards .cat-label svg {
    width: 18px; height: 18px;
    stroke: #7c3aed; fill: none;
    stroke-width: 2; stroke-linecap: round; stroke-linejoin: round;
}
.edu-awards ul {
    padding: 0; margin: 0;
    display: grid; grid-template-columns: 1fr 1fr; gap: 0.5rem 1.25rem;
}
.edu-awards li {
    font-size: 0.9rem; color: #334155;
    padding-left: 1.25rem; position: relative; line-height: 1.5;
}
.edu-awards li::before {
    content: ''; position: absolute; left: 0; top: 0.6em;
    width: 7px; height: 7px; border-radius: 2px;
    background: linear-gradient(135deg, #7c3aed, #0891b2);
}
@media (max-width: 800px) {
    .edu-main { grid-template-columns: auto 1fr; }
    .edu-main .dates { grid-column: 1 / -1; justify-self: start; }
}
@media (max-width: 520px) {
    .edu-awards ul { grid-template-columns: 1fr; }
}
```

- [ ] **Step 2: Insert Education section (replace `<!-- Education section goes in Task 6 -->`)**

```html
        <section id="education" class="sec">
            <div class="container">
                <div class="sec-head">
                    <h2>Образование</h2>
                    <span class="count">НИУ ВШЭ</span>
                    <span class="rule"></span>
                </div>
                <div class="edu-main">
                    <div class="emblem">ВШЭ</div>
                    <div>
                        <h4>НИУ ВШЭ, ФКН — Прикладная математика и информатика</h4>
                        <p>г. Москва · специализация «Машинное обучение и приложения» · Бакалавриат</p>
                    </div>
                    <span class="dates">2022 — 2026</span>
                </div>
                <div class="edu-awards">
                    <div class="cat-label">
                        <svg viewBox="0 0 24 24"><path d="M12 2 L14.5 9 L22 9.3 L16 14 L18 22 L12 17.5 L6 22 L8 14 L2 9.3 L9.5 9 Z"/></svg>
                        <span>Достижения</span>
                    </div>
                    <ul>
                        <li>Участник ICPC</li>
                        <li>Участник заключительного этапа ВСОШ по физике</li>
                        <li>Золотой медалист IEPhO (Международная олимпиада по экспериментальной физике)</li>
                        <li>Призёр множества олимпиад по физике и математике</li>
                    </ul>
                </div>
            </div>
        </section>
```

- [ ] **Step 3: Verify**

Reload. Expected:
- "Образование" heading + count pill "НИУ ВШЭ"
- Gradient-wash card with a gradient "ВШЭ" emblem on the left, program title + subtitle in the middle, "2022 — 2026" pill on the right
- Below: "Достижения" card with a 5-point star icon and 4 items in a 2-column list
- Bullet markers are small 7×7 gradient squares — NO emojis
- At < 800px: dates pill wraps below the text; at < 520px: achievements list becomes single column

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add Education section with HSE card and achievements list"
```

---

## Task 7: Опыт работы sub-section (featured card + 2 previous positions)

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Append work/project CSS (shared by Tasks 7 and 8)**

```css
/* ===== Work + Projects (shared) ===== */
.work-projects { padding-bottom: 3rem; }
.subsection-head { display: flex; align-items: baseline; gap: 12px; margin: 1.5rem 0 1rem; }
.subsection-head h3 { font-size: 1.25rem; color: #0f172a; font-weight: 700; letter-spacing: -0.01em; }
.subsection-head .count {
    font-size: 0.7rem; font-weight: 700; color: #64748b;
    background: #f1f5f9; padding: 3px 10px; border-radius: 999px; letter-spacing: 0.05em;
}
.subsection-head .rule { flex: 1; height: 1px; background: linear-gradient(90deg, #e2e8f0, transparent); }

/* Featured dark-gradient hero card */
.feat-card {
    position: relative; overflow: hidden;
    background: linear-gradient(135deg, #1e1b4b 0%, #0c4a6e 60%, #164e63 100%);
    color: #fff; border-radius: 14px;
    padding: 1.75rem 1.75rem 1.5rem;
    margin-bottom: 1.25rem;
}
.feat-card::before {
    content: ''; position: absolute;
    width: 320px; height: 320px; border-radius: 50%;
    background: radial-gradient(circle, #a78bfa, transparent 70%);
    top: -120px; right: -80px; opacity: 0.5;
}
.feat-card::after {
    content: ''; position: absolute;
    width: 240px; height: 240px; border-radius: 50%;
    background: radial-gradient(circle, #22d3ee, transparent 70%);
    bottom: -80px; left: -60px; opacity: 0.4;
}
.feat-card > * { position: relative; z-index: 2; }
.feat-card .badge {
    display: inline-flex; align-items: center; gap: 6px;
    background: rgba(255,255,255,0.14);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.25);
    border-radius: 999px; padding: 4px 12px;
    font-size: 0.72rem; font-weight: 600;
    letter-spacing: 0.06em; text-transform: uppercase;
    margin-bottom: 0.75rem;
}
.feat-card .badge .pulse {
    width: 7px; height: 7px; border-radius: 50%;
    background: #6ee7b7;
    box-shadow: 0 0 0 0 rgba(110,231,183, 0.6);
    animation: pulse 2s infinite;
}
@keyframes pulse {
    0% { box-shadow: 0 0 0 0 rgba(110,231,183, 0.6); }
    70% { box-shadow: 0 0 0 8px rgba(110,231,183, 0); }
    100% { box-shadow: 0 0 0 0 rgba(110,231,183, 0); }
}
.feat-card h3 {
    font-size: 1.35rem; font-weight: 700;
    letter-spacing: -0.01em; line-height: 1.25;
    margin-bottom: 0.75rem; color: #fff;
}
.feat-card .body {
    font-size: 1rem; line-height: 1.6;
    color: rgba(255,255,255,0.9);
    max-width: 760px; margin-bottom: 1rem;
}
.feat-card .links { display: flex; gap: 8px; flex-wrap: wrap; }
.feat-card .links a {
    font-size: 0.88rem; font-weight: 600;
    padding: 8px 16px; border-radius: 8px;
    background: #fff; color: #1e1b4b;
    border: 1px solid #fff;
    transition: transform 0.15s, box-shadow 0.15s;
}
.feat-card .links a:hover { transform: translateY(-1px); box-shadow: 0 8px 20px rgba(0,0,0,0.2); }

/* Regular card (used in work + projects) */
.reg-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
.reg-card {
    position: relative; overflow: hidden;
    background: #fff; border: 1px solid #e2e8f0;
    border-radius: 12px;
    padding: 1.1rem 1.25rem;
}
.reg-card::before {
    content: ''; position: absolute;
    left: 0; top: 0; bottom: 0;
    width: 3px;
    background: linear-gradient(180deg, #7c3aed, #0891b2);
}
.reg-card.variant-proj::before { background: linear-gradient(180deg, #22d3ee, #7c3aed); }
.reg-card h4 {
    font-size: 1.02rem; font-weight: 700;
    color: #0f172a; line-height: 1.3;
    letter-spacing: -0.01em; margin-bottom: 0.3rem;
}
.reg-card .sub {
    color: #7c3aed; font-size: 0.8rem;
    font-style: italic; font-weight: 600;
    margin-bottom: 0.5rem;
}
.reg-card .body { color: #334155; line-height: 1.55; font-size: 0.9rem; }
.reg-card .body ul { padding-left: 1.1rem; margin: 0; list-style: disc; }
.reg-card .body li { margin-bottom: 0.3rem; color: #334155; }
.reg-card .nda {
    display: block; margin-top: 0.6rem;
    font-size: 0.72rem; color: #94a3b8;
    font-style: italic; line-height: 1.4;
}
.reg-card .links { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 0.6rem; }
.reg-card .links a {
    font-size: 0.76rem; font-weight: 600;
    padding: 4px 10px; border-radius: 6px;
    background: #f1f5f9; color: #0f172a;
    border: 1px solid #e2e8f0;
    transition: background 0.15s, border-color 0.15s;
}
.reg-card .links a:hover { background: #e2e8f0; border-color: #cbd5e1; }

@media (max-width: 800px) {
    .reg-grid { grid-template-columns: 1fr; }
    .feat-card { padding: 1.4rem 1.25rem 1.25rem; }
    .feat-card h3 { font-size: 1.15rem; }
}
```

- [ ] **Step 2: Insert Work + Projects section shell + Опыт работы (replace `<!-- Work + Projects section goes in Tasks 7 + 8 -->`)**

Note: the outer `<section id="projects">` opens in this task and will be closed in Task 8. Only the Опыт работы content goes in now; Проекты content comes in Task 8.

```html
        <section id="projects" class="sec work-projects">
            <div class="container">
                <div class="sec-head">
                    <h2>Деятельность</h2>
                    <span class="count">Опыт + проекты</span>
                    <span class="rule"></span>
                </div>

                <!-- Опыт работы -->
                <div class="subsection-head">
                    <h3>Опыт работы</h3>
                    <span class="count">3</span>
                    <span class="rule"></span>
                </div>

                <div class="feat-card">
                    <span class="badge"><span class="pulse"></span>Текущая позиция</span>
                    <h3>Разработчик-исследователь в команде Машинного обучения и Антиспама</h3>
                    <p class="body">Занимаюсь полным циклом выкатки моделей - от сбора датасета до обучения и инференса. Разрабатываю архитектуры, подбираю подходящие решения под них, обсуждаю и внедряю передовые технологии.</p>
                    <div class="links">
                        <a href="https://t.me/vdoninav" target="_blank" rel="noopener">Связаться со мной →</a>
                    </div>
                </div>

                <div class="reg-grid">
                    <div class="reg-card">
                        <h4>Аналитик-разработчик в команде этики Яндекс Алисы</h4>
                        <div class="sub">Предыдущая позиция</div>
                        <div class="body">
                            <ul>
                                <li>Разработаны и внедрены этические классификаторы для повышения базового качества ответов Алисы</li>
                                <li>Проведены приемки новых базовых моделей, оценена этичность их ответов посредством передовых методов</li>
                                <li>Активно внедрены LLM в существующие процессы, что позволило существенно сократить расходы человеко-часов</li>
                                <li>Сделано множество переиспользуемых инструментов и материалов</li>
                            </ul>
                        </div>
                        <span class="nda">*Все задачи описаны максимально абстрактно для строгого соблюдения NDA, я не разглашаю ничего, что даже потенциально может под него подпадать</span>
                    </div>
                    <div class="reg-card">
                        <h4>Стажировка в группе исследования новых методов ИБ в Яндекс Такси</h4>
                        <div class="sub">Первый индустриальный опыт</div>
                        <div class="body">
                            <ul>
                                <li>Разработан и внедрен алгоритм классификации на основе LLM, который уменьшил затраты человеческих ресурсов на 15%</li>
                                <li>Применены передовые алгоритмы для решения задач дедупликации на основе BERT и графов</li>
                                <li>Осуществлялась поддержка и оптимизация существующих бизнес-процессов</li>
                            </ul>
                        </div>
                    </div>
                </div>

                <!-- Проекты will be added in Task 8, right after this comment -->
```

(Note: the `</div>` for `.container` and `</section>` for `#projects` are intentionally NOT closed here — Task 8 closes them.)

- [ ] **Step 3: Verify**

Reload. Expected:
- Heading "Деятельность" + pill "Опыт + проекты"
- Subsection header "Опыт работы" with count "3"
- Large dark-gradient featured card: pulsing green dot in the "ТЕКУЩАЯ ПОЗИЦИЯ" badge, white title "Разработчик-исследователь в команде Машинного обучения и Антиспама", body paragraph with the new description, white "Связаться со мной →" button
- Below: 2-column grid of two white cards (Аналитик-разработчик / Стажировка), each with a 3px purple-cyan left bar, dark slate titles (high contrast, readable), italic purple sub-labels, bulleted bodies
- First regular card has an NDA disclaimer at the bottom
- "Связаться со мной →" in the featured card opens `https://t.me/vdoninav` in new tab

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add Опыт работы with featured dark-gradient card and 2 previous positions"
```

---

## Task 8: Проекты sub-section (4 cards, close `#projects`)

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Insert the Проекты subsection and close the `#projects` section**

Replace the comment `<!-- Проекты will be added in Task 8, right after this comment -->` with:

```html
                <!-- Проекты -->
                <div class="subsection-head" style="margin-top: 2rem;">
                    <h3>Проекты</h3>
                    <span class="count">4</span>
                    <span class="rule"></span>
                </div>

                <div class="reg-grid">
                    <div class="reg-card variant-proj">
                        <h4>Распознавание и суммаризация документов в мобильном приложении HSE App X</h4>
                        <div class="sub">Один из лучших проектов ПМИ 2025</div>
                        <div class="body">
                            <ul>
                                <li>Разработаны и обучены суммаризаторы на основе самописных архитектур (вкл. RNN + Attention) для внедрения в официальное приложение НИУ ВШЭ HSE App X и систему электронного документооборота НИУ ВШЭ</li>
                                <li>Архитектуры разработаны специально для работы в условиях сверх ограниченных ресурсов — инференс за адекватное время на CPU (несколько архитектур для разных SLA)</li>
                                <li>Данный проект был выбран в качестве одного из лучших проектов среди студентов ПМИ ФКН НИУ ВШЭ за 2025 г.</li>
                            </ul>
                        </div>
                        <div class="links">
                            <a href="https://github.com/vdoninav/hse_ocr_summ" target="_blank" rel="noopener">GitHub</a>
                            <a href="https://cs.hse.ru/cppr/best_projects/hse_app_document_ai" target="_blank" rel="noopener">HSE Best Project</a>
                            <a href="https://drive.google.com/file/d/15s1vvJh0rRfqZ_qWpkAo9C-Rkl1l--Nh/view" target="_blank" rel="noopener">Report</a>
                        </div>
                    </div>
                    <div class="reg-card variant-proj">
                        <h4>Исследование уголовных дел с помощью NER (Named Entity Recognition)</h4>
                        <div class="body">
                            <ul>
                                <li>Обучена модель на основе BERT для извлечения ключевых слов из текстов судебных решений по определенной статье</li>
                                <li>Развернуто интерактивное веб-приложение для удобства использования</li>
                            </ul>
                        </div>
                        <div class="links">
                            <a href="https://github.com/vdoninav/hse_criminal_cases/" target="_blank" rel="noopener">GitHub</a>
                            <a href="https://hse-criminal-cases.streamlit.app/" target="_blank" rel="noopener">WebApp</a>
                        </div>
                    </div>
                    <div class="reg-card variant-proj">
                        <h4>Real Estate Analysis</h4>
                        <div class="body">
                            <ul>
                                <li>Проведен анализ стоимости жилья в различных ценовых сегментах в зависимости от факторов, потенциально влияющих на стоимость</li>
                                <li>Развернуто веб-приложение на Streamlit, сделан user-friendly интерфейс, нарисованы графики</li>
                            </ul>
                        </div>
                        <div class="links">
                            <a href="https://github.com/vdoninav/real_estate_analysis?tab=readme-ov-file" target="_blank" rel="noopener">GitHub</a>
                            <a href="https://vdoninav-real-estate-analysis.streamlit.app/" target="_blank" rel="noopener">WebApp</a>
                        </div>
                    </div>
                    <div class="reg-card variant-proj">
                        <h4>Расширение «Словарь ударений»</h4>
                        <div class="body">
                            <ul>
                                <li>Написано расширение для браузера Chrome, которое автоматически подсвечивает русские слова на веб-страницах и показывает нормативное ударение во всплывающей подсказке</li>
                                <li>Полностью open-source, добавляется и нативно работает с Google Chrome</li>
                            </ul>
                        </div>
                        <div class="links">
                            <a href="https://github.com/vdoninav/stress_spotter_extension" target="_blank" rel="noopener">GitHub</a>
                        </div>
                    </div>
                </div>
            </div>
        </section>
```

- [ ] **Step 2: Verify**

Reload. Expected:
- Below Опыт работы: "Проекты" subsection header with count "4"
- 2×2 grid of 4 project cards (HSE App X, NER, Real Estate, Словарь ударений)
- Each project card has a cyan→purple (reversed) left bar to subtly differ from the work cards
- HSE App X has 3 pill links (GitHub, HSE Best Project, Report), NER and Real Estate have 2 each (GitHub, WebApp), Словарь ударений has 1 (GitHub)
- Hovering a link pill darkens its background slightly
- Clicking each link opens the correct URL in a new tab — spot-check at least GitHub HSE OCR → should land at `github.com/vdoninav/hse_ocr_summ`
- HTML validator (or just a quick visual eyeball of the source) confirms `<section id="projects">` has its closing `</section>` now

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add Проекты sub-section with 4 project cards"
```

---

## Task 9: Contacts section with brand SVGs

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Append contacts CSS**

```css
/* ===== Contacts ===== */
.contact-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1rem; }
.contact-card {
    position: relative; overflow: hidden;
    background: #fff; border: 1px solid #e2e8f0;
    border-radius: 12px; padding: 1.25rem 1rem;
    display: flex; flex-direction: column; gap: 0.4rem;
    transition: transform 0.18s, box-shadow 0.18s;
}
.contact-card:hover { transform: translateY(-3px); box-shadow: 0 10px 30px rgba(15,23,42,0.1); }
.contact-card .icon-wrap {
    width: 42px; height: 42px; border-radius: 10px;
    background: #f8fafc; border: 1px solid #e2e8f0;
    display: flex; align-items: center; justify-content: center;
    margin-bottom: 0.4rem; color: #0f172a;
}
.contact-card .icon-wrap svg { width: 22px; height: 22px; }
.contact-card .label {
    font-size: 0.7rem; font-weight: 700;
    text-transform: uppercase; letter-spacing: 0.08em;
    color: #64748b;
}
.contact-card .value {
    font-size: 0.9rem; font-weight: 600;
    color: #0f172a; word-break: break-all;
}
.contact-card .arrow {
    position: absolute; top: 14px; right: 14px;
    color: #cbd5e1; font-size: 0.8rem;
    transition: transform 0.15s, color 0.15s;
}
.contact-card:hover .arrow { transform: translate(2px, -2px); color: #7c3aed; }
@media (max-width: 800px) { .contact-grid { grid-template-columns: 1fr 1fr; } }
@media (max-width: 520px) { .contact-grid { grid-template-columns: 1fr; } }
```

- [ ] **Step 2: Insert Contacts section (replace `<!-- Contacts section goes in Task 9 -->`)**

```html
        <section id="contact" class="sec">
            <div class="container">
                <div class="sec-head">
                    <h2>Контакты</h2>
                    <span class="count">4</span>
                    <span class="rule"></span>
                </div>
                <div class="contact-grid">
                    <a class="contact-card" href="mailto:aleksei@vdonin.com">
                        <div class="icon-wrap">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M2 7l10 7 10-7"/></svg>
                        </div>
                        <span class="label">Email</span>
                        <span class="value">aleksei@vdonin.com</span>
                        <span class="arrow" aria-hidden="true">↗</span>
                    </a>
                    <a class="contact-card" href="https://t.me/vdoninav" target="_blank" rel="noopener">
                        <div class="icon-wrap" style="color:#229ED9;">
                            <svg viewBox="0 0 24 24" fill="currentColor"><path d="M11.944 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0a12 12 0 0 0-.056 0zm4.962 7.224c.1-.002.321.023.465.14a.506.506 0 0 1 .171.325c.016.093.036.306.02.472-.18 1.898-.962 6.502-1.36 8.627-.168.9-.499 1.201-.82 1.23-.696.065-1.225-.46-1.9-.902-1.056-.693-1.653-1.124-2.678-1.8-1.185-.78-.417-1.21.258-1.91.177-.184 3.247-2.977 3.307-3.23.007-.032.014-.15-.056-.212s-.174-.041-.249-.024c-.106.024-1.793 1.14-5.061 3.345-.48.33-.913.49-1.302.48-.428-.008-1.252-.241-1.865-.44-.752-.245-1.349-.374-1.297-.789.027-.216.325-.437.893-.663 3.498-1.524 5.83-2.529 6.998-3.014 3.332-1.386 4.025-1.627 4.476-1.635z"/></svg>
                        </div>
                        <span class="label">Telegram</span>
                        <span class="value">@vdoninav</span>
                        <span class="arrow" aria-hidden="true">↗</span>
                    </a>
                    <a class="contact-card" href="https://github.com/vdoninav" target="_blank" rel="noopener">
                        <div class="icon-wrap" style="color:#181717;">
                            <svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
                        </div>
                        <span class="label">GitHub</span>
                        <span class="value">github.com/vdoninav</span>
                        <span class="arrow" aria-hidden="true">↗</span>
                    </a>
                    <a class="contact-card" href="https://www.linkedin.com/in/vdoninav/" target="_blank" rel="noopener">
                        <div class="icon-wrap" style="color:#0A66C2;">
                            <svg viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.063 2.063 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
                        </div>
                        <span class="label">LinkedIn</span>
                        <span class="value">linkedin.com/in/vdoninav</span>
                        <span class="arrow" aria-hidden="true">↗</span>
                    </a>
                </div>
            </div>
        </section>
```

- [ ] **Step 3: Verify**

Reload. Expected:
- "Контакты" heading + count "4"
- Row of 4 cards, each with a 42×42 icon well on top, uppercase label underneath, bold value below
- Icons: outlined envelope (dark slate), Telegram paper-plane in #229ED9, GitHub Octocat in #181717, LinkedIn logo in #0A66C2
- Hover any card: lifts up 3px, shadow deepens, arrow slides up-right and turns purple
- Click Email → opens default mail client composing to aleksei@vdonin.com
- Click Telegram → opens `https://t.me/vdoninav` in new tab
- Click GitHub → opens `https://github.com/vdoninav` in new tab
- Click LinkedIn → opens `https://www.linkedin.com/in/vdoninav/` in new tab
- At < 800px: 2×2 grid; at < 520px: single column

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add Контакты with brand SVG icons and hover lift"
```

---

## Task 10: Footer

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Append footer CSS**

```css
/* ===== Footer ===== */
.site-footer {
    background: #0d1117; color: #9da3ad;
    padding: 1.5rem 0;
    font-size: 0.82rem;
}
.site-footer .container {
    display: flex; justify-content: space-between; align-items: center; gap: 1rem;
}
.site-footer .meta { color: #64748b; font-size: 0.78rem; }
.site-footer .meta em { font-style: italic; }
@media (max-width: 520px) {
    .site-footer .container { flex-direction: column; text-align: center; }
}
```

- [ ] **Step 2: Insert `<footer>` before `</body>` (replace `<!-- Footer goes in Task 10 -->`)**

```html
    <footer class="site-footer">
        <div class="container">
            <span>© Вдонин А.В., 2026. Все права защищены.</span>
            <span class="meta"><em class="grad-text">Vdonin</em>.com</span>
        </div>
    </footer>
```

- [ ] **Step 3: Verify**

Reload. Expected:
- Dark bar at the very bottom of the page
- Left: "© Вдонин А.В., 2026. Все права защищены." (year is **2026**, not 2025)
- Right: muted italic gradient "Vdonin" + ".com"
- At < 520px: stacks vertically, centered

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add dark footer with 2026 copyright"
```

---

## Task 11: Scrollspy + smooth scroll polish

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/index.html`

- [ ] **Step 1: Insert `<script>` before `</body>` (replace `<!-- Scrollspy script goes in Task 11 -->`)**

```html
    <script>
        (function () {
            var sections = document.querySelectorAll('section[id]');
            var links = document.querySelectorAll('.site-header nav a[href^="#"]');
            if (!sections.length || !links.length || !('IntersectionObserver' in window)) return;

            var linkById = {};
            links.forEach(function (a) {
                var id = a.getAttribute('href').slice(1);
                linkById[id] = a;
            });

            var setActive = function (id) {
                links.forEach(function (a) { a.classList.remove('active'); });
                if (linkById[id]) linkById[id].classList.add('active');
            };

            var observer = new IntersectionObserver(function (entries) {
                entries.forEach(function (entry) {
                    if (entry.isIntersecting) setActive(entry.target.id);
                });
            }, { rootMargin: '-30% 0px -60% 0px', threshold: 0 });

            sections.forEach(function (s) { observer.observe(s); });
        })();
    </script>
```

- [ ] **Step 2: Verify scrollspy**

Reload. Expected:
- Scroll slowly from top to bottom. As each section crosses the upper third of the viewport, its corresponding nav link gains a 2px purple underline (`.active` class).
- Click a nav link: page smooth-scrolls to the section; the clicked link gains the active underline shortly after.
- The section's top is not hidden under the sticky header (thanks to `scroll-margin-top: 72px`).

- [ ] **Step 3: Verify reduced-motion**

In browser devtools → Rendering → Emulate CSS media feature `prefers-reduced-motion: reduce`. Then:
- Click a nav link: scrolls instantly (no smooth animation)
- Hover a project card: no translateY lift animation

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add scrollspy and smooth-scroll polish"
```

---

## Task 12: Update docs (RELEASE_NOTES.md and CLAUDE.md)

**Files:**
- Modify: `/Users/vdav/Documents/vdoninav.github.io/RELEASE_NOTES.md`
- Modify: `/Users/vdav/Documents/vdoninav.github.io/CLAUDE.md`

- [ ] **Step 1: Prepend a v2.0 entry to `RELEASE_NOTES.md`**

Read the current file. Use Edit to add a new section above the existing "# Release Notes v1.4 — October 12, 2025" heading. The new section:

```markdown
# Release Notes v2.0 — April 21, 2026

## Overview
Full visual redesign around a "bold gradient" aesthetic. All content preserved except the explicit edits listed below. Deployment model unchanged: still a single-file static site on GitHub Pages.

## Main Changes

### New visual language
- Primary gradient: purple (#7c3aed) → blue (#2563eb) → cyan (#0891b2), applied to hero title, the logo wordmark, category labels, and achievement bullets.
- Soft radial background blobs in the hero section (purple, cyan, pink) for depth.
- Sticky dark header with backdrop-blur; scrollspy highlights the active section.
- System font stack, all icons inlined as SVG — zero external requests.

### Content updates
- **Hero** rewritten. Role is now only "ML-engineer" (dropped "Аналитик-разработчик +"). Name "Алексей Вдонин" moved to a preamble line above the role, in the gradient accent color.
- **Current position** updated to "Разработчик-исследователь в команде Машинного обучения и Антиспама" and featured in a dark-gradient hero card.
- **Previous Yandex Alice role** preserved intact but demoted to "Предыдущая позиция".
- **Skills** reorganized into 4 categories (Языки, ML/DL, Базы данных, Инфраструктура). Dropped: "Анализ данных", "Алгоритмы и структуры данных", "Remote Servers".
- **Education** split into an HSE main card and a Достижения card with gradient-square bullets.
- **Projects section** split into two labelled sub-sections: "Опыт работы" (3) and "Проекты" (4). HSE App X moved from work to projects.
- **Contacts** redesigned as 4 icon-forward cards with real brand SVGs (Email envelope, Telegram, GitHub, LinkedIn).
- **Footer** copyright year bumped 2025 → 2026.

## Technical Details
- Single `index.html`, one inline `<style>` block, one small `<script>` (~20 lines, IntersectionObserver scrollspy).
- Responsive breakpoints at 800px and 520px.
- Honors `prefers-reduced-motion`.

---
```

- [ ] **Step 2: Update `CLAUDE.md` content-conventions section**

Open `CLAUDE.md`. The "Content Conventions" section mentions `<h4>` / `<h5>` project card structure and an NDA notice on "card 1". Update it to reflect:
- Projects section now has two labelled sub-sections ("Опыт работы" and "Проекты") sharing the `#projects` anchor
- Current position lives in the dark-gradient `.feat-card`; previous positions and personal projects use `.reg-card`
- NDA disclaimer now lives on the "Предыдущая позиция" card (Яндекс Алиса), not on the featured current-position card

Use Edit to replace the "Content Conventions" section body with:

```markdown
- **Language:** user-facing copy is in **Russian**. Match the existing tone (first person, professional) when editing. Comments inside `index.html` are in a mix of Russian and English — keep them consistent.
- **Design system:** primary gradient is `linear-gradient(135deg, #7c3aed 0%, #2563eb 45%, #0891b2 100%)`, applied via the `.grad-text` utility class. Cards use `.feat-card` (dark gradient, for the featured/current-position card) or `.reg-card` (white with a 3px gradient left-edge bar).
- **Projects area:** lives under one `<section id="projects">` with nav label "Деятельность". Inside there are two labelled sub-sections — "Опыт работы" and "Проекты" — each with its own count pill. The current position gets the `.feat-card` treatment; everything else uses `.reg-card`.
- **NDA notice** currently lives on the "Предыдущая позиция" card (Аналитик-разработчик в команде этики Яндекс Алисы). Do not add Alice-specific details beyond what's already abstracted there.
- **Icons:** inline SVG only — no external icon libraries, no web fonts. Skill-category icons are 18×18 stroke SVGs; brand icons in Контакты use the real marks from simple-icons.org (GitHub, Telegram, LinkedIn) + a Feather-style envelope for Email.
```

- [ ] **Step 3: Verify**

- Run `head -5 RELEASE_NOTES.md` — top line should be `# Release Notes v2.0 — April 21, 2026`.
- Open `CLAUDE.md` and visually confirm the new Content Conventions section reflects the new structure.

- [ ] **Step 4: Commit**

```bash
git add RELEASE_NOTES.md CLAUDE.md
git commit -m "Docs: v2.0 release notes and updated content conventions"
```

---

## Task 13: Final verification pass

**Files:**
- Read-only verification against `/Users/vdav/Documents/vdoninav.github.io/index.html` and the original site state.

- [ ] **Step 1: Responsive check**

Open `index.html` in a browser. Use devtools device mode to set each width below and confirm no layout breakage (no horizontal scrollbar, no text spilling out of cards, no images clipped unexpectedly):
- 1440 px
- 1024 px
- 800 px (breakpoint — desktop → tablet)
- 520 px (breakpoint — tablet → mobile)
- 375 px (iPhone SE baseline)

- [ ] **Step 2: Content checklist**

Use Grep / Read against `index.html` to confirm every item below is present:

| Content item | Present? |
|---|---|
| Hero eyebrow "Привет" | |
| "Меня зовут" + "Алексей Вдонин" + "я —" | |
| `<h1>` "ML-engineer" with `.grad-text` | |
| Greeting "Рад видеть Вас на моём личном сайте" | |
| CTA link to `https://t.me/vdoninav` | |
| About paragraph verbatim | |
| Photo `src="main_photo.jpg"` | |
| Skill categories: Языки, ML/DL, Базы данных, Инфраструктура | |
| Skill items: Python, C++, Classic ML, DL (NLP, CV), SQL, YTsaurus, PostgreSQL, CI/CD (GitLab, Proprietary), Linux, CLI | |
| NO `Анализ данных`, NO `Алгоритмы и структуры данных`, NO `Remote Servers` | |
| Education main: НИУ ВШЭ, ФКН, ПМИ, "Машинное обучение и приложения", "Бакалавриат", "2022 — 2026" | |
| Achievements (4): ICPC, ВСОШ по физике, IEPhO золото, призёр олимпиад | |
| Featured card: "Разработчик-исследователь в команде Машинного обучения и Антиспама" + new body text | |
| Previous position: "Аналитик-разработчик в команде этики Яндекс Алисы" + "Предыдущая позиция" + 4 bullets + NDA note | |
| Стажировка — Яндекс Такси card + "Первый индустриальный опыт" + 3 bullets | |
| Projects: HSE App X + "Один из лучших проектов ПМИ 2025" + 3 bullets + 3 links (GitHub, HSE Best Project, Report) | |
| Projects: NER + 2 bullets + 2 links (GitHub, WebApp) | |
| Projects: Real Estate + 2 bullets + 2 links | |
| Projects: Словарь ударений + 2 bullets + 1 link | |
| Contacts: Email, Telegram, GitHub, LinkedIn — all 4 with correct URLs | |
| Footer: "© Вдонин А.В., 2026. Все права защищены." | |

Concrete Grep commands to sanity-check the removals:

```bash
# These MUST return 0 matches:
grep -c "Анализ данных" index.html          # expected: 0
grep -c "Алгоритмы и структуры данных" index.html  # expected: 0
grep -c "Remote Servers" index.html         # expected: 0
grep -c "2025. Все права" index.html        # expected: 0

# These MUST return at least 1 match:
grep -c "Разработчик-исследователь" index.html   # expected: >= 1
grep -c "Предыдущая позиция" index.html     # expected: >= 1
grep -c "2026. Все права" index.html        # expected: >= 1
grep -c "ML-engineer" index.html            # expected: >= 1
```

- [ ] **Step 3: External-link spot check**

Hover each project and contact link. Each should show the correct `href` in the browser status bar. Compare against the reference URLs in the "Reference: Content snapshot" section at the top of this plan.

- [ ] **Step 4: Offline / file:// check**

Close the tab. Re-open `index.html` by double-clicking it (opens via `file://`). Site must render identically — if it doesn't, an external dependency snuck in. Inspect Network tab: the only requests should be for `main_photo.jpg` and files under `thumbs/`.

- [ ] **Step 5: Keyboard navigation check**

Press Tab repeatedly from page load. Focus should cycle through: nav links (4) → hero CTA → contact/project links inside sections in document order. Each focused element must have a visible focus indicator (browser default is fine).

- [ ] **Step 6: If any verification fails**, fix in place and commit with a message like `"Fix: <issue> during verification pass"`. Re-run the failing check. Only move on once every row in the content checklist passes.

- [ ] **Step 7: Deploy**

```bash
git status  # expect clean working tree
git log --oneline -15  # review the redesign commit series
git push origin master
```

After `git push`, GitHub Pages will redeploy. Wait ~60 seconds, then load `https://vdonin.com` and spot-check:
- Hero renders with gradient
- Photo loads
- Project links work
- Favicon still correct

---

## Self-Review Notes

I walked back through the spec section-by-section and cross-checked against the task list:

- **Design direction, color system, typography** → implemented in Task 1 base CSS + per-section stylesheets.
- **Content Changes (1–5)** → all covered: new current position card in Task 7; demoted Alice card in Task 7; hero copy change in Task 3; skill deletions baked into the Task 5 markup (and explicitly verified in Task 13); footer year in Task 10.
- **Section-by-section design** → each section has a dedicated task (Tasks 2–10).
- **Interactions (scrollspy, hover, pulse, smooth scroll, reduced-motion)** → Task 11 + CSS in earlier tasks (pulse keyframe is in Task 7, hover transforms in Tasks 4/7/9, reduced-motion in Task 1).
- **File Deliverables** → Task 1–11 touch `index.html`; Task 12 touches `RELEASE_NOTES.md` and `CLAUDE.md`.
- **Out of Scope** → no build pipeline, no web fonts, no dark mode, no frameworks. Confirmed none of the tasks introduce these.
- **Acceptance criteria** → Task 13 covers each of the 7 spec acceptance items.

No placeholders found. Type names are consistent across tasks (`.feat-card`, `.reg-card`, `.skill-cat`, `.sec-head`, `.subsection-head`). One reused class (`.reg-card.variant-proj`) differentiates project cards from work cards with just a gradient-direction swap.
