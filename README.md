# Neta Summer Camp

A hands-on Ruby on Rails apprenticeship built around the [Neta](https://neta.now) codebase. Mentees get a working machine, ship real features against a shared backlog, and finish by building a small app of their own.

**Live site:** https://hecbuma.github.io/summer-camp/

## What's here

| File | What it is |
|---|---|
| `neta_summer_program.md` | The full program — Week 0 through the capstone, the ticket backlog, concept maps, and coaching notes. |
| `lane0_foundations.md` | A from-scratch track for total beginners: Ruby from zero, in the terminal, up to understanding what Rails is. |
| `rails_fundamentals.md` | The follow-on beginner lane: scaffold a working Rails app, take it apart, then make a first change to Neta. Includes resource links. |
| `week0_setup_and_fundamentals.md` | Step-by-step machine setup for Windows (WSL2, Ruby, Rails, Postgres, git) plus a backend primer and first-PR exercise. |
| `mockups/` | Reference UI designs for the Phase 1 tickets, in the Neta visual language. |
| `index.md`, `mockups.md` | The site's landing and design-reference pages. |
| `_layouts/`, `assets/`, `_config.yml` | Jekyll scaffolding that renders the docs as a styled site. |

The docs are plain Markdown — edit them directly and the site rebuilds on push.

## Running the site locally

The published site is built by GitHub Pages, so you don't need to build anything to make changes — just edit the Markdown and push. To preview locally:

```bash
# one-time setup
gem install bundler jekyll

# from the repo root
jekyll serve
```

Then open http://localhost:4000/summer-camp/.

## Publishing

GitHub Pages serves the site from the `main` branch root. Push to `main` and the site updates automatically. To change the Pages source: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)`**.
