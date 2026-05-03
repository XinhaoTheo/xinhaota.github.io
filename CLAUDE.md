# CLAUDE.md

Project-level notes for Claude Code sessions on this repo.

## What this is

Personal academic website built from the **HugoBlox `academic-cv`** starter template (Hugo + Tailwind v4 + Hugo Modules). Owner: 上海交大本科, 方向 ML Systems / AI Infra, ACL 2025 Findings 一作, 求 AI Infra 实习。

## Stack & versions

- **Hugo**: extended edition required (HugoBlox needs it). Module path declares `go 1.19`. Use latest Hugo extended (>= 0.125 recommended for Tailwind v4 pipeline).
- **Package manager**: `pnpm@10.14.0` (per `package.json`). `package-lock.json` and `pnpm-lock.yaml` both exist; prefer pnpm.
- **Modules**: theme delivered via Hugo Modules (`go.mod`) — `HugoBlox/kit/modules/{blox,slides,integrations/netlify}`. Do NOT vendor or fork these unless overriding a layout.
- **CSS**: Tailwind v4 via `@tailwindcss/cli`, processed by Hugo asset pipeline.
- **Search**: Pagefind (built post-`hugo --minify`).

## Common commands

```bash
pnpm install              # first time / after package.json changes
pnpm dev                  # local preview at http://localhost:1313 (hugo server --disableFastRender)
pnpm build                # production build → public/, then pagefind index
hugo mod get -u            # update theme modules
hugo mod tidy              # clean go.sum
```

If `hugo` not on PATH: `brew install hugo` (make sure it's the **extended** build: `hugo version` should print `+extended`).

## Directory map — what to edit vs leave alone

### ✅ Edit these (your content + config)

| Path | Purpose |
| --- | --- |
| `content/_index.md` | Homepage section blocks (bio, featured papers, news, talks, CTA) |
| `data/authors/me.yaml` | **Author profile** (name, bio, education, experience, skills, links). HugoBlox v2 schema lives in `data/`, NOT `content/authors/`. The `resume-biography-3` block reads from here via `username: me`. |
| `assets/media/authors/me.jpg` | Avatar — looked up by author slug |
| `content/authors/_index.md` | Has `build.render: never` cascade — leave as is unless you want public author pages |
| `content/publications/` | One folder per paper; replace the 3 demo entries with your ACL 2025 Findings paper |
| `content/experience.md` | Experience / internships timeline |
| `content/projects/` | Project cards (drop the pytorch/scikit/pandas demos) |
| `content/events/` | Talks (drop demo entry if no talks yet) |
| `content/blog/` | Blog posts (optional — can hide via menu if unused) |
| `content/courses/` | Courses page (likely drop entirely as a student) |
| `config/_default/hugo.yaml` | `title`, `baseURL` (set to `https://xinhaota.github.io/`) |
| `config/_default/params.yaml` | `hugoblox.identity` (name, tagline, description, social), theme colors, header config |
| `config/_default/menus.yaml` | Top nav — remove Courses if dropped |
| `config/_default/languages.yaml` | Per-language metadata if you keep i18n |
| `static/uploads/` | Drop `resume.pdf` here (homepage CTA links to `uploads/resume.pdf`) |
| `static/` | Favicons, OG images, other static assets |
| `assets/` | Custom CSS / images you import via Hugo pipeline |

### ⛔ Don't touch (managed/generated)

- `node_modules/`, `public/`, `resources/`, `.hugo_build.lock`, `hugo_stats.json` — build output / deps
- `go.sum`, `pnpm-lock.yaml`, `package-lock.json` — lockfiles (let tools update)
- `layouts/` — currently empty/sparse; only add files here when **overriding** a module template (advanced)
- `data/` — same; override module data only if needed
- `.github/`, `.devcontainer/`, `netlify.toml` — CI / deploy config; touch only when changing deploy target
- `LICENSE.md`, `README.md` (template's, can be replaced later)
- `hugoblox.yaml` — template metadata
- `.gitignore`, `.npmrc`, `.vscode/`

## Homepage structure (content/_index.md)

Driven by `sections:` list using HugoBlox blocks:
1. `resume-biography-3` — pulls from `content/authors/me/`
2. `markdown` — "📚 My Research" free-text
3. `collection` (id: `papers`) — featured publications grid
4. `collection` — all publications (citation view)
5. `collection` (id: `talks`) — events
6. `collection` (id: `news`) — recent blog posts as news cards
7. `cta-card` — has `demo: true`, **delete this section**

## Deploy

- `netlify.toml` present → Netlify-ready out of the box.
- For GitHub Pages (`xinhaota.github.io`): add a `.github/workflows/` action that runs `pnpm build` and publishes `public/`. Set `baseURL: https://xinhaota.github.io/` in `hugo.yaml`.

## Known gotchas

- **Author profile lives in `data/authors/<slug>.yaml`, NOT `content/authors/<slug>/_index.md`.** HugoBlox v2 schema (`schema: hugoblox/author/v1`) — homepage `resume-biography-3` block resolves `username: me` against `data/authors/me.yaml`. The `content/authors/` tree only exists so the taxonomy works; don't put profile data there or it will silently be ignored.
- `content/authors/_index.md` has `build.render: never` — author pages are not published as standalone routes (intentional). Don't change unless you want public author pages.
- Demo `cta-card` block has `demo: true` flag — visible in the kit demo site only, but cleaner to delete.
- Module updates: re-run `hugo mod get -u && hugo mod tidy` to pull latest theme.
