# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

Personal one-page website for Aleksei Vdonin, deployed via **GitHub Pages** at **vdonin.com**. The repo name (`vdoninav.github.io`) is the GitHub Pages convention for a user site — pushes to `master` are the deploy.

## Build / Run / Test

There is no build system, no package manager, no test suite. The site is plain HTML + inline CSS, no JavaScript.

- **Local preview:** open `index.html` directly in a browser, or serve the directory (e.g. `python3 -m http.server 8000`).
- **Deploy:** `git push origin master` — GitHub Pages serves the repo root.
- **Custom domain:** the `CNAME` file (contents: `vdonin.com`) is what binds the domain. Do not delete or rename it; removing it will break the custom domain mapping.

## Architecture

Everything of substance lives in **`index.html`** — a single file containing:

- `<style>` block (lines ~12–294): all CSS is inline. There is no external stylesheet.
- `<body>` sections keyed by anchor IDs used by the header nav: `#about`, `#skills`, `#education`, `#projects`, `#contact`. Note that the "Обо мне" (`#about`) link is intentionally commented out in the nav but the section itself is still rendered — keep them in sync if restoring.
- Responsive breakpoint at `max-width: 768px` (mobile: stacks columns, single-column skill list).

Supporting files:
- `main_photo.jpg` — hero photo referenced from `#about`. Cropped in CSS via `object-position: 35% center` on a circular 300px frame.
- `thumbs/` — favicons, `apple-touch-icon`, and `site.webmanifest`. Linked from the `<head>`.
- `RELEASE_NOTES.md` — human-readable changelog maintained per version.
- `README.md` — minimal pointer to the live site.
- `docs/superpowers/specs/` and `docs/superpowers/plans/` — dated design specs and implementation plans from past redesigns. Check here before starting new design work.

## Content Conventions

- **Language:** user-facing copy is in **Russian**. Match the existing tone (first person, professional) when editing. Comments inside `index.html` are in a mix of Russian and English — keep them consistent.
- **Design system:** primary gradient is `linear-gradient(135deg, #7c3aed 0%, #2563eb 45%, #0891b2 100%)`, applied via the `.grad-text` utility class. Cards use `.feat-card` (dark gradient, for the featured/current-position card) or `.reg-card` (white with a 3px gradient left-edge bar).
- **Projects area:** lives under one `<section id="projects">` with nav label "Деятельность". Inside there are two labelled sub-sections — "Опыт работы" and "Проекты" — each with its own count pill. The current position gets the `.feat-card` treatment; everything else uses `.reg-card`.
- **NDA notice** currently lives on the "Предыдущая позиция" card (Аналитик-разработчик в команде этики Яндекс Алисы). Do not add Alice-specific details beyond what's already abstracted there.
- **Icons:** inline SVG only — no external icon libraries, no web fonts. Skill-category icons are 18×18 stroke SVGs; brand icons in Контакты use the real marks from simple-icons.org (GitHub, Telegram, LinkedIn) + a Feather-style envelope for Email. All decorative SVGs must have `aria-hidden="true"`.

## Release Workflow

Release commits follow the pattern `v<major>.<minor>` (e.g. `v1.4`, `v1.45`). When making a user-visible change:
1. Update `index.html` (and assets if needed).
2. Append a new versioned section to `RELEASE_NOTES.md` following the existing format (Overview → Main Changes grouped by section → Technical Details).
3. Commit with the version as the message (`v1.X`).
