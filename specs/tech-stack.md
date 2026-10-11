# Tech Stack

## Static Site Generator
- **[Zola](https://www.getzola.org/)** — a fast, single-binary static site generator written in Rust.
  - Identified by `config.toml` keys (`compile_sass`, `build_search_index`, `[markdown]`) and TOML front matter (`+++`), which are Zola-specific (Hugo uses YAML/TOML front matter delimited by `---` and different config keys).

## Templating
- **[Tera](https://keats.github.io/tera/)** — Zola's built-in templating engine (Jinja2/Django-template-inspired), seen in `templates/index.html` (`{{ config.title }}`, `{{ section.content | safe }}`).

## Content
- **Markdown** with TOML front matter for page content (`content/_index.md`).

## Styling
- **Sass/SCSS**, compiled by Zola (`compile_sass = true` in `config.toml`); source lives in `sass/` (currently empty).
- Plain **CSS** output/override in `static/style.css`.

## Config
- `config.toml` — site title, base URL, markdown syntax highlighting, search index generation.

## Project Layout
```
config.toml      # Zola site configuration
content/         # Markdown pages (TOML front matter)
templates/       # Tera HTML templates
static/          # Static assets served as-is (CSS, images, etc.)
sass/            # Sass/SCSS sources compiled by Zola
themes/          # Zola themes (currently empty, no theme in use)
```

## Notable Gaps
- No theme installed under `themes/`.
- No build scripts, CI, or package manifest present — site is built/served via the Zola CLI directly (`zola build` / `zola serve`).
