# Site Redesign v3 — Strict Dark, Bilingual

**Date:** 2026-10-29
**Scope:** Full restyle of vdonin.com, content sync with CV v3.6, second language (EN). Still a static GitHub Pages site with no build step.

## Goals

1. Bring the content in line with the current CV (v3.6, October 2026).
2. Replace the "bold gradient" look (v2) with a strict, professional, dark-first design.
3. Dark mode on by default, with a light mode the visitor can switch to.
4. Add an English version while keeping plain HTML.

## Decisions

- **Bilingual mechanism:** two static pages. Russian stays at the root (`index.html`), English lives at `en/index.html`. A link to the other language sits in the header (EN / RU) and the footer (English version / Русская версия); `<link rel="alternate" hreflang>` tags point both ways. One page per language keeps `lang`, SEO and screen readers correct, at the cost of duplicated CSS/JS that must be kept in sync.
- **Theme:** dark always, unless the visitor toggles. The choice is stored in `localStorage` and applied by a 6-line inline script in `<head>` before first paint, so there is no flash. `prefers-color-scheme` is deliberately not consulted (the owner asked for dark by default).
- **CV download:** link to `cv/Aleksei_Vdonin_CV.pdf` from the hero and the contacts block. The PDF itself is supplied by the owner and must not contain the phone number.
- **Phone number:** never published on the site.
- **Skills:** CV list plus "Claude Code" (owner's explicit request).

## Design system

```
Dark (default)              Light (data-theme="light")
--bg          #0b0e14       #ffffff
--bg-elev     #10141c       #f6f8fa
--line        #1f2633       #e3e7ec
--line-strong #2d3644       #cbd3dc
--text        #e6e9ee       #121820
--text-2      #a9b2bf       #475262
--text-3      #7d8898       #66717f
--accent      #6ea8fe       #1f5fd1
--accent-ink  #0b0e14       #ffffff
```

- One accent colour, flat 1px borders, 4–6px radii, no shadows, no gradients, no blobs, no animated elements.
- System sans stack for prose; system monospace (`ui-monospace, SF Mono, Menlo, …`) for labels, dates, section numbers and small links.
- Section headers: mono accent number (`01`…`06`) + h2.
- Experience and education use a two-column timeline (date column 180px, stacks below 600px).
- All icons inline SVG with `aria-hidden="true"`. No web fonts, no external requests.

## Information architecture

Header (sticky, solid): wordmark `vdonin.com`, nav (Навыки / Опыт / Образование / Проекты / Контакты), language switch, theme toggle.

1. Hero / profile: eyebrow (role · city), name, lead paragraph from the CV profile, actions (Telegram, CV PDF), three facts (current role, teaching, education — the education fact states the degree at first glance: «ФКН НИУ ВШЭ — бакалавриат „Прикладная математика и информатика“», then «Магистратура „Науки о данных“, 2026–2028»; EN: BSc in Applied Mathematics and Computer Science / MSc in Data Science), photo (220px square, 112px on phones).
2. `#skills` — four rows: Языки, ML / DL, Данные, Инструменты.
3. `#experience` — four dated entries (two marked "Текущая позиция").
4. `#education` — MSc 2026–2028, BSc 2022–2026, three achievements.
5. `#projects` — thesis (full width), HSE App X (full width), NER + DE→EN translator side by side, «Словарь ударений» (full width).
6. `#interests` — one line.
7. `#contact` — Email, Telegram, GitHub, LinkedIn, CV.

Footer: copyright, link to the other language.

## Responsive

Breakpoints: 900px (facts stack), 760px (hero stacks, photo first, tighter section padding), 720px (header wraps, nav drops to its own row and is sized to fit one line down to 360px; section scroll offset 100px), 700px (project grid → 1 column), 600px (timeline/skills/contacts → 1 column), 340px (scroll offset 128px for a wrapped nav). Verified at 320, 390, 768, 1024, 1440 px in both themes.

## Accessibility

Skip link, `:focus-visible` rings, `aria-current` on the active nav link, `aria-label`/`title` on the theme toggle updated on each switch, `lang`/`hreflang` on language links, reduced-motion support, print stylesheet (light, no header/actions).
