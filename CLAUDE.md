# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A self-contained vacation planning website. Each destination is a single HTML file with all CSS and JavaScript inline — no build step, no dependencies, no framework.

Live site: **https://wolf-020.github.io/vacation-planning/**
GitHub repo: **https://github.com/Wolf-020/vacation-planning**

## Previewing changes

Open the file directly in a browser:
```
open index.html
```

No server, compiler, or install step needed. Reload the browser tab after any edit to see changes.

## Deploying

Push to `main` — GitHub Pages deploys automatically within ~60 seconds:
```
git push
```

## Git workflow

**Commit and push after every meaningful unit of work** — do not batch multiple unrelated changes into one commit. The goal is that the GitHub remote always reflects the current state so no work is ever lost.

Always stage specific files, never `git add .`:
```
git add index.html
git commit -m "feat/fix/update: short description of what changed and why"
git push
```

Commit prefix conventions: `feat:` new content/feature, `update:` changes to existing content, `fix:` corrections, `chore:` non-content changes (renaming, config).

Commit at minimum after each of these: adding a new section or destination, updating itinerary content, changing styles or layout, fixing links or copy, and any structural refactor. When in doubt, commit.

## Architecture of index.html

The file has three sections in order: `<style>`, HTML body, `<script>`. Everything lives in one file — no external CSS or JS files.

**CSS design tokens** (`:root` at top of `<style>`):
- `--blue-deep / --blue-mid / --blue-light` — the primary color ramp
- `--t1 / --t2 / --t3` — text hierarchy (primary / secondary / tertiary)
- `--s1 / --s2 / --s3` — surface hierarchy (white / off-white / subtle border)
- `--sh-sm / --sh-md / --sh-lg` — shadow scale
- `--r1 / --r2 / --r3` — border-radius scale (12 / 18 / 24 px)

**HTML sections** (in document order):
1. `nav` — fixed glassmorphism pill nav; gets `.scrolled` class via JS on scroll
2. `.hero` — full-viewport gradient with animated rings, stat strip pinned to bottom
3. `#itinerary` — day-tab switcher (`.day-tab` / `.day-panel`) + expandable timeline (`.t-item`)
4. `#dining` — restaurant cards grid (`.card`)
5. `#stay` — accommodation cards grid (`.stay-card`)
6. `#tips` — tips grid on dark gradient background
7. `footer`

**JavaScript** (bottom of file, ~40 lines, no libraries):
- Scroll listener: adds `.scrolled` to nav, highlights active nav link
- `switchDay(id, btn)` — swaps `.active` between `.day-panel` divs and `.day-tab` buttons
- `toggle(el)` — expands/collapses a `.t-item` (closes siblings first)
- `IntersectionObserver` — adds `.vis` to `.reveal` elements as they scroll into view, with staggered `data-delay`

**Interactive expand pattern** (timeline items): collapsed state uses `max-height:0; opacity:0` on `.t-desc` and `.t-links`; `.t-item.open` transitions them to `max-height:200px; opacity:1`. Adding a new timeline activity requires both the collapsed and open CSS rules to already be in place — they are inherited automatically from the existing `.t-desc` / `.t-links` selectors.

**Card link pattern**: all external links use `target="_blank"`. Apple Maps links follow the format `https://maps.apple.com/?q=Place+Name&ll=lat,lon`. AllTrails links go to `https://www.alltrails.com/trail/us/oregon/<trail-slug>`.

## Adding a new destination

Create a new HTML file (e.g., `yosemite.html`) using `index.html` as the template. Update the hero gradient colors, stat strip values, section content, and `<title>`. The nav, timeline, card, and reveal patterns are all reusable as-is.
