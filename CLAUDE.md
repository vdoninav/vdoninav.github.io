# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

Personal one-page website for Aleksei Vdonin, deployed via **GitHub Pages** at **vdonin.com**. The repo name (`vdoninav.github.io`) is the GitHub Pages convention for a user site — pushes to `master` are the deploy.

## Build / Run / Test

There is no build system, no package manager, no test suite. The site is plain HTML + inline CSS + one small inline script.

- **Local preview:** open `index.html` directly in a browser (assets load, but the RU/EN switch opens a folder listing under `file://`), or serve the directory (e.g. `python3 -m http.server 8000`) to test language switching.
- **Screenshots for checking layout:** headless Chrome works — `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu --hide-scrollbars --window-size=1280,4300 --screenshot=out.png <url>`. Chrome enforces a minimum window width (~500px), so for phone widths render the page inside an `<iframe width="390">` on a wrapper page served from the same local server.
- **Deploy:** `git push origin master` — GitHub Pages serves the repo root.
- **Custom domain:** the `CNAME` file (contents: `vdonin.com`) is what binds the domain. Do not delete or rename it; removing it will break the custom domain mapping.

## Architecture

Two pages, one per language, each fully self-contained:

- **`index.html`** — Russian, served at `/`. `<html lang="ru">`.
- **`en/index.html`** — English, served at `/en/`. `<html lang="en">`. Asset and language links are relative (`../main_photo.jpg`, `../thumbs/…`, `../cv/…`, `../` for RU).

The `<style>` and `<script>` blocks are **byte-identical** in both files; only content, `lang`, metadata and link paths differ. Any CSS or JS change must be applied to both files — check with `diff <(sed -n '/<style>/,/<\/style>/p' index.html) <(sed -n '/<style>/,/<\/style>/p' en/index.html)` and the same for `<script>`.

Inside each file:

- `<head>`: metadata (`canonical`, `hreflang` alternates for ru/en/x-default, Open Graph, JSON-LD `Person`), a 6-line theme bootstrap script (applies the `localStorage` theme before first paint), then the `<style>` block.
- `<style>`: design tokens on `:root` (dark, the default) and `:root[data-theme="light"]`; then reset, header, hero, sections, skills, timeline, projects, interests, contacts, footer, responsive, reduced-motion, print.
- `<body>` sections keyed by anchor IDs used by the header nav: `#skills`, `#experience`, `#education`, `#projects`, `#interests` (no nav link), `#contact`. The hero has `id="top"`.
- Footer script (~80 lines): theme toggle (sets `data-theme` on `<html>`, saves to `localStorage`, updates the button's `aria-label`/`title` and `meta[name=theme-color]`) and a scroll-position scrollspy (rAF-throttled scroll/resize handler; the last linked section stays active at the bottom of the page) that toggles `.active` + `aria-current` on nav links.

Supporting files:
- `main_photo.jpg` — profile photo, shown as a 220px square (112px on phones) with `object-position: 35% center`.
- `cv/Aleksei_Vdonin_CV.pdf` — the public CV linked from the hero button and the contacts block on both pages. The file is managed by the owner. **It must not contain the phone number.** The original Word export (`Vdonin-CV-V*.pdf` in the repo root) contains the phone number and is git-ignored (see `.gitignore`) — never commit or force-add it.
- `thumbs/` — favicons, `apple-touch-icon`, and `site.webmanifest` (icon paths are relative to the manifest).
- `RELEASE_NOTES.md` — changelog, newest version first.
- `README.md` — pointer to the live site and structure overview.
- `docs/superpowers/specs/` and `docs/superpowers/plans/` — dated design specs and implementation plans from past redesigns. The current design is `specs/2026-10-29-v3-strict-dark-bilingual-design.md`. Check here before starting new design work.

## Content Conventions

- **Language:** Russian copy on `index.html`, English on `en/index.html`. Keep both pages in sync: every content change on one page needs its counterpart on the other. Russian is first person, professional; English mirrors it in the same register (US spelling, matching `og:locale` en_US; no slang).
- **Source of truth for facts** is the owner's CV. Do not invent dates, employers, metrics or titles. The phone number is never published.
- **Design system:** strict and flat. Dark-first tokens (`--bg #0b0e14`, `--text #e6e9ee`, `--accent #6ea8fe`; light: `--bg #ffffff`, `--text #121820`, `--accent #1f5fd1`). One accent colour, 1px borders, 4–6px radii, no shadows, no gradients, no animations. System sans for prose; system monospace (`.mono`) for labels, dates, section numbers and small links.
- **Section header pattern:** `<header class="sec-head"><span class="num mono" aria-hidden="true">0N</span><h2>…</h2></header>`.
- **Experience / education** use `ol.timeline > li` with `.when.mono` (dates, optional `.current` label) and `.what` (h3, `.org`, `ul.bullets`, optional `.note`). Current positions get `<span class="current">Текущая позиция</span>` / `Current position`.
- **Projects** use `article.proj` (`.kicker.mono`, h3, `ul.bullets`, `p.links`). The thesis card has `.wide` to span both columns.
- **NDA notice** lives on the Яндекс Алиса entry. Do not add Alice-specific details beyond what is already abstracted there.
- **Icons:** inline SVG only — no external icon libraries, no web fonts. Brand icons in Контакты use the real marks from simple-icons.org (GitHub, Telegram, LinkedIn); the rest are Feather-style 24×24 stroke icons. All decorative SVGs must have `aria-hidden="true"`.
- **Accessibility:** keep the skip link, `:focus-visible` outline, `aria-label` on the language link and theme toggle, `rel="noopener"` on `target="_blank"` links.

## Release Workflow

Release commits follow the pattern `v<major>.<minor>` (e.g. `v1.4`, `v2.0`, `v3.0`). When making a user-visible change:
1. Update `index.html` **and** `en/index.html` (and assets if needed).
2. Prepend a new versioned section to `RELEASE_NOTES.md` following the existing format (Overview → Main Changes grouped by section → Technical Details).
3. Update the "latest" line in `README.md` if the version changed.
4. Commit with the version as the message (`v3.X`). Pushing to `master` deploys.
