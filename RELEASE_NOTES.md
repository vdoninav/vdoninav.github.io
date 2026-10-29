# Release Notes v3.0 — October 29, 2026

## Overview
Strict, professional redesign with a dark theme on by default, content synchronised with CV v3.6, and a full English version of the site. Still a static GitHub Pages site: plain HTML, inline CSS, a small inline script, no build step.

## Main Changes

### New visual language
- Dark-first design tokens (`--bg`, `--text`, `--accent`, …) with a complete light palette under `:root[data-theme="light"]`. One accent colour (blue) replaces the purple→cyan gradient.
- Removed: gradient text, background blobs, pulsing badge, hover lifts, frosted glass. Flat 1px borders, 4–6px radii, no shadows.
- System monospace is used for labels, dates, section numbers and small links; system sans for prose. Still no web fonts and no external requests.
- Theme toggle (sun/moon) in the header. Dark is the default; the visitor's choice is stored in `localStorage` and applied before first paint.

### Second language
- New `en/index.html` with the same markup and styles and fully translated content. a link to the other language in the header (EN / RU) and the footer (English version / Русская версия), `hreflang` alternates, `og:locale`, translated `<title>`/description.

### Content updates (from CV v3.6)
- **Profile** rewritten: ML Engineer / Research Engineer; NLP models (embeddings, rerankers, text classification, LLMs); full ML cycle; teaching DL at HSE FCS. Three key facts under the actions; the Образование fact leads with the degree, «ФКН НИУ ВШЭ — бакалавриат „Прикладная математика и информатика“», followed by the Master's.
- **Actions:** "Написать в Telegram" and "Скачать CV" (links to `cv/Aleksei_Vdonin_CV.pdf`).
- **Skills** now follow the CV in four rows: Языки (Python, C++, SQL, YQL), ML / DL (ML/DL NLP+CV, PyTorch, vLLM, Multi-GPU workflow), Данные (NumPy, scikit-learn, Pandas, Polars, YTsaurus, PostgreSQL), Инструменты (Git, Linux, Jupyter, Claude Code). Dropped: CI/CD, CLI, Hadoop, Spark, S3.
- **Experience** is a dated timeline of four entries: Разработчик-исследователь, команда ML и антиспама (Mar 2026 –, three concrete bullets); Преподаватель по DL, ФКН НИУ ВШЭ (Sep 2026 –, new); Аналитик-разработчик, команда этики Яндекс Алисы (Feb 2025 – Mar 2026, NDA note kept); Стажёр-аналитик, Яндекс Такси (Sep 2024 – Feb 2025).
- **Education** adds the Master's (Науки о данных, ФКН, 2026–2028) above the Bachelor's (ПМИ, 2022–2026). Achievements follow the CV: ВСОШ physics final, «Высшая лига» prize, IEPhO gold. Dropped: ICPC, "many olympiad prizes", the «Машинное обучение и приложения» specialization line.
- **Projects** are the CV's five: thesis on time-series tokenization (new, full-width card, GitHub + PDF), HSE App X (full-width), criminal-cases NER and DE→EN Transformer translator (new, W&B report) side by side, «Словарь ударений» (full-width). Dropped: Real Estate Analysis.
- **Interests** section added (padel, IPSC, track driving, DL pet projects).
- **Contacts** gain a CV row; the phone number from the CV is deliberately not published.

### Navigation
- Nav is now Навыки / Опыт / Образование / Проекты / Контакты. Anchors: `#skills`, `#experience`, `#education`, `#projects`, `#interests`, `#contact` (`#projects` kept for old links).

### Assets
- `main_photo.jpg` recompressed from 6.4 MB to 474 KB.
- `thumbs/site.webmanifest` gets a name, correct relative icon paths and the dark theme colour.

## Technical Details
- Two HTML files (`index.html`, `en/index.html`) with identical `<style>` and `<script>` blocks; content differs only. Keep them in sync.
- Inline script (~80 lines): theme bootstrap in `<head>`, toggle handler (updates `data-theme`, `localStorage`, `aria-label`, `meta[name=theme-color]`), scroll-position scrollspy (rAF-throttled; Контакты stays active at the bottom of the page).
- Breakpoints: 900px, 760px, 720px, 700px, 600px, 340px. Verified at 320 / 390 / 768 / 1024 / 1440 px in both themes.
- Skip link, `:focus-visible` rings, `aria-current` on the active nav link, reduced-motion support, print stylesheet, JSON-LD `Person` schema.

---

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
- Single `index.html`, one inline `<style>` block, one small `<script>` (~30 lines, IntersectionObserver scrollspy that toggles `.active` class and `aria-current="location"`).
- Responsive breakpoints at 800px and 520px.
- Honors `prefers-reduced-motion`.

---

# Release Notes v1.4 - October 12, 2025

## Overview
This release adds enhanced navigation, expands the education section with additional achievements, and adds a second row of project cards to showcase more work.

## Main Changes

### Navigation Enhancement
- **Added "Образование" button**: New navigation link added to the header menu between "Навыки" and "Проекты"
- **Removed "Обо мне" button**: Commented out the "About Me" navigation link to streamline the header menu
- Provides direct access to the education section for better user experience

### Education Section Expansion
- **Added competitive programming and olympiad achievements**:
  - ICPC participant
  - All-Russian School Olympiad in Physics (final stage participant)
  - Gold medalist at International Experimental Physics Olympiad (IEPhO)
  - Multiple awards in physics and mathematics olympiads
- **Clarified university program**: Updated description to include specialization "Machine Learning and Applications"
- Abbreviated faculty name to "ФКН" for consistency

### Projects Section Expansion
- **Added second row of projects**: Three additional project cards added below the main projects
- **Section title updated**: Changed from "Проекты" to "Деятельность" in navigation for broader scope

#### Project 4 - Criminal Cases NER Research
- BERT-based model trained for extracting keywords from court decision texts for specific articles
- Interactive web application deployed for easy use
- Removed placeholder bullet point (commented out)
- Links to GitHub repository and deployed WebApp

#### Project 5 - Real Estate Analysis
- Analysis of housing prices across different price segments based on factors potentially affecting cost
- User-friendly web application deployed on Streamlit with interactive visualizations
- Removed redundant description bullet point (commented out)
- Links to GitHub repository and deployed WebApp

#### Project 6 - "Словарь ударений" Extension
- Chrome browser extension that automatically highlights Russian words on web pages and shows normative stress in tooltips
- Fully open-source, natively integrated with Google Chrome
- Removed placeholder bullet point (commented out)
- Link to GitHub repository added

### Minor Text Improvements
- **Project 2 description refined**: Changed "одного из лучших проектов студентов ПМИ" to "одного из лучших проектов среди студентов ПМИ" for better grammar

## Technical Details
- Maintained responsive design and existing styling
- Added `<br>` tag for visual separation between project rows
- Used existing `projects-grid` class for consistent layout
- All new project cards follow the established card structure and styling

---

*This release expands the portfolio section and improves navigation to showcase a more comprehensive professional profile.*
