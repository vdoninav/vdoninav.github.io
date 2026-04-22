# Aleksei Vdonin — Personal Site

[![Live site](https://img.shields.io/badge/live-vdonin.com-7c3aed?style=flat-square)](https://vdonin.com) [![Hosted on GitHub Pages](https://img.shields.io/badge/hosted-GitHub_Pages-222?style=flat-square&logo=github)](https://pages.github.com)

One-page personal site for **Aleksei Vdonin** — ML-engineer. Source is English, content is Russian.
Личный одностраничный сайт — обо мне, навыки, образование, деятельность, контакты.

## Tech

- Pure **HTML** + inline **CSS**
- ~30 lines of vanilla **JS** (`IntersectionObserver`-based scrollspy)
- Single `index.html` — no build, no framework, no external fonts
- All icons inlined as SVG
- Deployed via **GitHub Pages**

## Structure

```
├── index.html         # the entire site (markup + styles + tiny script)
├── main_photo.jpg     # hero photo
├── thumbs/            # favicons + webmanifest
├── CNAME              # custom domain → vdonin.com
└── RELEASE_NOTES.md   # changelog per version
```

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

| # | Section      | Description                                                |
| - | ------------ | ---------------------------------------------------------- |
| 1 | Обо мне      | Short intro                                                |
| 2 | Навыки       | Skills — Python/C++, ML/DL, databases, infra               |
| 3 | Образование  | НИУ ВШЭ, ФКН — Прикладная математика и информатика          |
| 4 | Деятельность | Work experience + personal projects                        |
| 5 | Контакты     | Email, Telegram, GitHub, LinkedIn                          |

## Changelog

See [RELEASE_NOTES.md](./RELEASE_NOTES.md) — latest: **v2.0** (April 2026 redesign).

---

© Вдонин А.В., 2026
