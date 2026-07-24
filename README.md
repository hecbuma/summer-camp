# Neta Summer Camp

A hands-on Ruby on Rails apprenticeship built around the [Neta](https://neta.now) codebase. Mentees get a working machine, ship real features against a shared backlog, and finish by building a small app of their own.

**Live site:** https://hecbuma.github.io/summer-camp/

## What's here

| File | What it is |
|---|---|
| `neta_summer_program.md` | The full program — Week 0 through the capstone, the ticket backlog, concept maps, and coaching notes. |
| `lane0_foundations.md` | A from-scratch track for total beginners: Ruby from zero, in the terminal, up to understanding what Rails is. |
| `rails_fundamentals.md` | Beginner lane 02: build a movie watchlist app, take it apart, add a feature by hand. Includes resource links. |
| `data_fundamentals.md` | Beginner lane 03: the Rails console, tables as persistent data, and `has_many`/`belongs_to` relationships. |
| `frontend_fundamentals.md` | Beginner lane 04: ERB, layouts, partials, and styling with Tailwind. |
| `putting_it_together.md` | Beginner lane 05: ship a full feature through the whole Git workflow, then the first change to Neta. |
| `real_data.md` | Beginner lane 06: posters, the OMDb API, service objects, caching, and a computed score. The Neta-shaped finale. |
| `git_fundamentals.md` | A beginner Git guide: the mental model and the daily branch → commit → push → PR loop. |
| `fundamentals.md`, `setup.md` | Hub pages that group the beginner lanes and the OS setup guides. |
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
