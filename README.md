# Aleksei Vdonin — Personal Site

[![Live site](https://img.shields.io/badge/live-vdonin.com-1f5fd1?style=flat-square)](https://vdonin.com) [![Hosted on GitHub Pages](https://img.shields.io/badge/hosted-GitHub_Pages-222?style=flat-square&logo=github)](https://pages.github.com)

One-page personal site for **Aleksei Vdonin** — ML Engineer / Research Engineer. Available in Russian (root) and English (`/en/`).
Личный одностраничный сайт — профиль, навыки, опыт, образование, проекты, контакты.

## Tech

- Pure **HTML** + inline **CSS**, no build step, no framework, no external fonts or requests
- Dark theme by default with a light-theme toggle (remembered in `localStorage`)
- ~80 lines of vanilla **JS**: theme toggle + scroll-position scrollspy
- All icons inlined as SVG
- Deployed via **GitHub Pages**

## Structure

```
├── index.html         # Russian site (markup + styles + script)
├── en/index.html      # English site — same markup/styles, translated content
├── cv/                # Aleksei_Vdonin_CV.pdf — public CV linked from both pages
├── main_photo.jpg     # profile photo
├── thumbs/            # favicons + webmanifest
├── CNAME              # custom domain → vdonin.com
├── RELEASE_NOTES.md   # changelog per version
└── docs/superpowers/  # design specs and plans for past redesigns
```

The two HTML files share identical `<style>` and `<script>` blocks. When changing styles or behaviour, apply the change to both.

## Local preview

```sh
# quickest
open index.html

# or serve locally
python3 -m http.server 8000
```

## Deploy

Push to `master` → GitHub Pages rebuilds automatically at [vdonin.com](https://vdonin.com).

## Sections

| # | RU           | EN         | Description                                              |
| - | ------------ | ---------- | -------------------------------------------------------- |
| – | Профиль      | Profile    | Role, lead paragraph, Telegram + CV actions, key facts   |
| 1 | Навыки       | Skills     | Languages, ML/DL, data, tools                            |
| 2 | Опыт         | Experience | Four dated positions                                     |
| 3 | Образование  | Education  | HSE FCS — MSc Data Science, BSc AMCS, achievements        |
| 4 | Проекты      | Projects   | Thesis + four projects with links                        |
| 5 | Интересы     | Interests  | One line                                                 |
| 6 | Контакты     | Contact    | Email, Telegram, GitHub, LinkedIn, CV                    |

## Changelog

See [RELEASE_NOTES.md](./RELEASE_NOTES.md) — latest: **v3.0** (October 2026: strict dark redesign, CV sync, English version).

---

© Вдонин А. В., 2026
