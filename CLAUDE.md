# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Personal portfolio website for Erald Shahinas, hosted on GitHub Pages. Built with Astro and Tailwind CSS. Deploys automatically via GitHub Actions on push to `main`.

## Commands

```bash
npm install       # install dependencies (first time)
npm run dev       # local dev server at localhost:4321
npm run build     # production build → dist/
npm run preview   # preview the production build locally
```

## Architecture

Single-page site assembled in `src/pages/index.astro`. All sections are isolated Astro components in `src/components/`. The shared HTML shell (nav, footer, fonts, scroll-reveal script) lives in `src/layouts/Layout.astro`.

**Component order:** Hero → About → Experience → Publications → Projects → Education → Skills → Contact

All content (job roles, publications, projects, skills) is hardcoded as data arrays at the top of each component file — no CMS or external data fetching. To update content, edit the relevant component directly.

## Deployment

- GitHub Actions workflow: `.github/workflows/deploy.yml`
- Triggers on push to `main`, builds with `npm ci && npm run build`, publishes `dist/` to GitHub Pages
- Enable GitHub Pages in repo Settings → Pages → Source: GitHub Actions
- The `site` URL in `astro.config.mjs` must match the actual GitHub Pages URL

## Design System

Light/white Material-influenced palette. Tokens in `tailwind.config.mjs`:
- `bg`: `#fafafa`, `surface`: `#ffffff`, `accent`: `#2563eb` (blue-600)
- Fonts: Inter (body) + Inter Tight (headings, loaded via Google Fonts in Layout.astro)
- Cards: `bg-white rounded-2xl border border-slate-100 shadow-sm hover:shadow-md`
- Section alternation: white → slate-50 → white (Hero, Experience, Skills = white; About, Publications, Education, Contact = slate-50)
- Scroll reveal: `.reveal` class + IntersectionObserver → adds `.visible` in Layout.astro

## File Map

```
src/
├── pages/
│   ├── index.astro                        # Single-page shell — imports all section components in order
│   └── projects/
│       ├── world-model-nn.astro           # RL world model architecture comparison (Sep–Dec 2024)
│       ├── toxicity-detection.astro       # NLP toxicity benchmark: SVM/LSTM/BERT (Mar–Jun 2024)
│       └── folgpt.astro                   # Multilanguage voice assistant, Flask+GPT-4+Azure (Jun–Jul 2023)
├── layouts/
│   ├── Layout.astro                       # Main shell: nav, footer, Google Fonts, IntersectionObserver scroll-reveal
│   └── ProjectLayout.astro                # Shell for project pages: back link, hero (title/period/tags/GitHub btn), <slot/>
└── components/                            # All content hardcoded as data arrays at top of each file
    ├── Hero.astro                         # Name, tagline, GitHub + contact CTAs (left-aligned, Inter Tight)
    ├── About.astro                        # Bio paragraph + 3 stat cards (MSc, years experience, languages)
    ├── Experience.astro                   # Timeline cards: Aalto RA, Futurice intern, NovaStar intern
    ├── Publications.astro                 # 2 IEEE ETFA 2025 papers with ieeexplore.ieee.org links
    ├── Projects.astro                     # 3 project cards linking to /projects/<slug> pages
    ├── Education.astro                    # Aalto MSc + Metropolia BSc cards with GPA + courses
    ├── Skills.astro                       # Skill pill-cloud grouped by category
    └── Contact.astro                      # Email + GitHub buttons

tailwind.config.mjs                        # Color tokens (bg, surface, accent) + Inter Tight font family
astro.config.mjs                           # Site URL (must match GitHub Pages URL for deploy)
.github/workflows/deploy.yml               # CI: npm ci && npm run build → publish dist/ to GitHub Pages
```
