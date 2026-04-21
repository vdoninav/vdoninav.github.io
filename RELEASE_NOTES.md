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
