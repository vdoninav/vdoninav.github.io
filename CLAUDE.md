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

## Content Conventions

- **Language:** user-facing copy is in **Russian**. Match the existing tone (first person, professional) when editing. Comments inside `index.html` are also in Russian — keep them consistent.
- **Project cards** (`#projects > .projects-grid > .project-card`) follow a fixed structure: `<h4>` title, optional `<h5><i>` subtitle, `<ul>` of bullets, then one or more `<a target="_blank">` links. New cards should replicate this structure so the grid stays visually uniform.
- **NDA notice on card 1:** the first project card carries an explicit NDA disclaimer ("*Все задачи описаны максимально абстрактно для строгого соблюдения NDA..."). Do not add specifics about the Yandex/Alice work beyond what is already abstracted there.
- Rows of projects are separated by a bare `<br>` between `.projects-grid` divs — matches the existing visual rhythm.

## Release Workflow

Release commits follow the pattern `v<major>.<minor>` (e.g. `v1.4`, `v1.45`). When making a user-visible change:
1. Update `index.html` (and assets if needed).
2. Append a new versioned section to `RELEASE_NOTES.md` following the existing format (Overview → Main Changes grouped by section → Technical Details).
3. Commit with the version as the message (`v1.X`).
