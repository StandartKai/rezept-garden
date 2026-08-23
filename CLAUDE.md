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
src/site/styles/           ← Sass source
.env                       ← site config (name, theme, feature flags)
eleventy.config.js         ← Eleventy config, markdown-it plugins, image transforms
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

`src/site/notes/rezepte/Home.md` is the landing page. It has `gardenEntry` in its tags. When adding a new recipe, add a wikilink to it in `Home.md` under the appropriate section (Recipes or Ingredients).

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
