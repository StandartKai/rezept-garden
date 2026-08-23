# Rezept Garden

Recipe website built with Eleventy + Digital Garden, deployed on Vercel.
Live site: https://rezept-garden.vercel.app

## Stack

- **Eleventy 3** static site generator
- **Obsidian Digital Garden** template (notes are Obsidian-flavored markdown)
- **Sass** for styles, theme pulled from env `THEME` URL
- **Vercel** auto-deploys on push to `main`

## Project structure

```
src/site/notes/rezepte/   ← recipe markdown files (this is the content)
src/site/notes/templates/  ← note templates (Recipe.md)
src/site/img/user/images/  ← recipe images (referenced as /img/user/images/...)
src/site/_data/            ← meta.js (reads .env), eleventyComputed.js
src/site/_includes/        ← Nunjucks layouts and partials
src/site/styles/           ← Sass source (edit custom-style.scss only)
.env                       ← site config (name, theme, feature flags)
.eleventy.js               ← Eleventy config, markdown-it plugins, image transforms
```

## Recipe format

Every recipe is a markdown file in `src/site/notes/rezepte/`. Frontmatter must include:

```yaml
---
dg-publish: true
permalink: /rezepte/slug-here/
---
```

The `gardenEntry` tag marks the home page (`Home.md`). Follow the structure in `src/site/notes/templates/Recipe.md` for new recipes: title, optional image, ingredients list, numbered instructions with steps, optional notes section.

Images go in `src/site/img/user/images/` and are referenced in markdown as `![alt|width](/img/user/images/filename.png)`.

Internal links between recipes use Obsidian wikilink syntax: `[[rezepte/Recipe Name\|Display Name]]`.

## Home page

`src/site/notes/rezepte/Home.md` is the landing page. It has `gardenEntry` in its tags.

It is a hero block plus two grids of image cards (raw HTML — `markdown-it` runs with `html: true`). When adding a new recipe, add a card to the appropriate grid:

```html
<a class="rg-card" href="/rezepte/slug/"><img src="/img/user/images/file.png" alt="Alt text" width="600"><span class="rg-card-body"><span class="rg-card-title">Short Title</span><span class="rg-card-note">One-line description.</span></span></a>
```

Two constraints:

- **No blank lines inside an HTML block** — markdown-it ends the raw-HTML block at the first blank line and would escape the rest. Keep each grid on one long line.
- The `href` is the recipe's `permalink`, not a wikilink. Card titles may be shortened; the full name lives on the recipe page.

The `picture` build transform rewrites each `<img>` into a `<picture>` with generated webp/jpeg sources, so card images get responsive variants for free.

## Styling

All custom design lives in `src/site/styles/custom-style.scss` — it loads last (after `obsidian-base`, the remote `_theme`, and `digital-garden-base`), so it wins. Do not edit the other files in `styles/`; they carry "do not modify" headers and are overwritten on template updates.

The design is a warm editorial system: Fraunces (display serif) for headings, Inter for body, a cream/terracotta palette. Tokens are `--rg-*` on `body`, with a `body.theme-dark` block remapping them. Layout knobs are the template's `--dg-*` variables.

Overriding an upstream *theme* variable (e.g. `--text-normal`) needs the `body.theme-light` / `body.theme-dark` selector — the remote theme defines them on `.theme-light`, which outranks a bare `body`.

## Commands

```bash
npm install          # install dependencies
npm run build        # full production build → dist/
npm run start        # dev server with hot reload
```

## Deployment

Push to `main` on GitHub (`StandartKai/rezept-garden`). Vercel picks it up automatically.

## Key conventions

- Recipe filenames use title case with spaces (e.g. `Smash Burger.md`) — Eleventy slugifies them for URLs
- The `.env` file is committed and contains non-secret site configuration
- Keep recipes self-contained; link to ingredient sub-recipes (like Pickled Onions) with wikilinks
