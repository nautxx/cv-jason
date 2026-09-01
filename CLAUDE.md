# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Jason Barnachea's personal portfolio/CV site. Plain HTML/CSS/JS — no build step, no framework, no package manager, no tests.

## Commands

Run locally (no dependencies required):
```bash
python3 -m http.server 8080
```
Then open `http://localhost:8080`. (`.claude/launch.json` runs the same thing on port 8420.)

Run via Docker (nginx serving the static files):
```bash
docker compose up --build
```
Serves at `http://localhost:8077`. `Dockerfile` just copies the repo root into `/usr/share/nginx/html` on `nginx:alpine` — there's nothing to build/compile.

There is no lint, test, or build command — changes are verified by loading the page in a browser.

## Commit messages

Use a Conventional Commits prefix on every commit title: `type: short imperative summary`.

- `feat` — new capability or behavior (a new flag, a new source, a new output format)
- `fix` — bug fix
- `chore` — maintenance with no user-visible behavior change (deps, config, cleanup)
- `docs` — README/CLAUDE.md/comment-only changes
- `refactor` — code restructuring with no behavior change
- `test` — test-only changes
- `style` — formatting/whitespace, no logic change
- `perf` — performance improvement
- `revert` — reverts a previous commit

Keep the summary lowercase after the colon, imperative mood ("add", not "added"/"adds"), no trailing period, ideally under ~70 characters. Example: `feat: add minimal session panel to CLI output`.

For a commit that only touches one thing, the title alone is enough — skip the body. When a commit bundles several distinct changes (e.g. a new flag plus a rewritten function plus a doc update), add a blank line after the title and a short bullet per change:

```
feat: add minimal session panel to CLI output

- add --no-banner flag to suppress it for scripts/redirected logs
- add print_session_hud() — boxed Title/Source/Chapters/Workers/Output summary
- drop the old plain "Downloading chapters..." print in favor of the panel
```

## Architecture

Two static pages, `index.html` (the CV/portfolio) and `privacy.html`, sharing `static/css/styles.css` and `static/js/main.js`. `index.html` is long (~860 lines) and organized as sequential `<section id="...">` blocks (`intro`, `experience`, `projects`, `credentials`, `skills`, `contact`) — CSS in `styles.css` is organized the same way, in matching `/* ---------- Name ---------- */` comment blocks, so when editing a section's markup, find its counterpart block in the CSS by that section name rather than searching stylesheet-wide.

`main.js` is a handful of independent, self-contained features gated by `if (elementExists)` checks rather than a single init/router — each block owns one DOM feature and can be read/edited in isolation:
- theme toggle (writes `data-theme="dark"` on `<html>` + `localStorage`)
- scroll-spy sitemap nav (`IntersectionObserver` pair: one to reveal the sitemap once the hero scrolls out, one to highlight the active section link)
- typewriter role text cycling through a fixed list of role strings, synced to `.role-pill` highlighting
- a canvas "snakes" animation in the hero, paused via `IntersectionObserver` + `visibilitychange` when off-screen/tab hidden

Dark mode: an inline `<script>` in `index.html`'s `<head>` (before any stylesheet) sets `data-theme="dark"` synchronously from `localStorage`/`prefers-color-scheme` to avoid a flash of the wrong theme; `styles.css` defines light tokens on `:root` and overrides on `:root[data-theme="dark"]`. Keep these three pieces (inline head script, `main.js` toggle handler, CSS dark tokens) in sync if changing theme behavior.

Analytics: Plausible (self-hosted at `plausible.nautsfw.com`) is loaded inline in `<head>`; Vercel Insights is loaded via `/_vercel/insights/script.js` before `main.js` at the bottom of `<body>` (implies deployment target is Vercel).
