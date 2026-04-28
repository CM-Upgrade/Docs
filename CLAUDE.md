# CLAUDE.md

> Project context for Claude Code (and other AI coding assistants).
> Read this file first before making changes to the repository.

## What this repo is

The **UpgradeMate documentation site** — a Hugo static site built on the
[Hextra](https://github.com/imfing/hextra) theme, branded for UpgradeMate.

- **Stack:** Hugo Extended ≥ 0.160 + Hextra v0.12.x (installed as a Hugo Module).
- **Output:** Static HTML in `public/` after `hugo --gc --minify`.
- **Local dev:** `hugo server -s . --port 1313` (live reload).

## Repository layout

```
upgrademate-docs/
├── hugo.yaml                  # All site config + Hextra params
├── go.mod                     # Hextra theme pulled as a Hugo Module
├── assets/
│   └── css/custom.css         # Brand stylesheet (CSS variables → tokens)
├── static/
│   └── images/                # Logos served verbatim at /images/...
├── content/                   # Markdown pages (flat — no /docs/ subfolder)
│   ├── _index.md              # Landing page (cascade: type=docs)
│   ├── getting-started.md
│   └── configuration.md
├── i18n/
│   └── en.yaml                # Overrides Hextra strings (e.g. footer copyright)
└── docs/
    └── BRANDING.md            # ← Brand guidelines (read before any visual change)
```

## Branding

**All visual identity decisions live in [`docs/BRANDING.md`](docs/BRANDING.md).**

Two implementation files mirror that spec:

1. [`assets/css/custom.css`](assets/css/custom.css) — color tokens, typography,
   dark-mode overrides for Hextra's Tailwind classes.
2. [`hugo.yaml`](hugo.yaml) — logo config, default theme, navbar/footer behavior.

When asked to change colors, fonts, logos, or any visual element:

1. Read [`docs/BRANDING.md`](docs/BRANDING.md) first.
2. Edit `:root` tokens in `assets/css/custom.css` rather than scattering hex
   values throughout the codebase.
3. If `--um-action` (the primary blue) changes, also update the
   `--hextra-primary-hue/saturation/lightness` HSL trio — Tailwind reads HSL
   components, not hex.
4. Update `docs/BRANDING.md` so the spec stays in sync.

## Hextra-specific gotchas (verified against v0.12.2)

These tripped us up while building the site — record them here so we don't
repeat them.

| Issue | Cause | Fix |
| ----- | ----- | --- |
| Logo not showing — only a tiny circle in navbar | `params.logo.*` is the **wrong** key in v0.12+; Hextra reads `params.navbar.logo.*` | Nest `logo:` under `navbar:` in `hugo.yaml` |
| Logo squashed into a square | Hextra renders `<img width=.. height=..>` and defaults to 20×20 | Set both `width` and `height` to match the source aspect ratio |
| `default: dark` in config but site shows light | Hextra stores user choice in `localStorage["color-theme"]`; that wins over config | Open in incognito, OR run `localStorage.removeItem('color-theme')` in DevTools |
| Footer says "© 2024 Hextra." | Hardcoded fallback in Hextra footer template | Override via `i18n/en.yaml` → `copyright: "..."` (no template editing needed) |
| "Powered by Hextra" credit visible | Default-on Hextra option | Set `params.footer.displayPoweredBy: false` |
| `HAHAHUGOSHORTCODE…` placeholder leaks into the TOC | Shortcodes inside markdown headings don't get expanded before TOC building | Keep headings as plain text; use `{{< param >}}` only in body paragraphs |
| Empty black rectangle in TOC sidebar (dark mode) | Hextra always renders the "TOC bottom" container (tags/edit/back-to-top); when all are hidden it still draws bg+border+shadow | CSS rule in `custom.css` section #8 hides it; remove rule to re-enable |
| Menu item won't go away when "removed" | YAML indentation — only the `name:` line was deleted, leaving orphan keys parsed as the previous item's continuation | Delete the **entire** 4-line block including `weight:`, `url:`, `params:` |

## Conventions

- **URLs are flat.** Documentation lives at `/getting-started`, `/configuration`,
  not `/docs/...`. The site root *is* the docs landing page.
- **Cascade pattern:** `content/_index.md` declares `cascade: { type: docs }`,
  giving every descendant page Hextra's docs layout (sidebar + TOC + prev/next)
  without per-file front matter.
- **YAML config only.** Don't introduce TOML or JSON config files.
- **No template overrides** unless absolutely necessary. Prefer:
  1. `params.*` knobs in `hugo.yaml`,
  2. `i18n/<lang>.yaml` string overrides,
  3. CSS overrides in `assets/css/custom.css`.
  Only fall back to copying a Hextra partial into `layouts/_partials/` when
  the above three options can't achieve the goal.

## Useful commands

```powershell
# Dev server with live reload
hugo server -s c:\Temp\hugo_test\upgrademate-docs --port 1313

# Production build
hugo -s c:\Temp\hugo_test\upgrademate-docs --gc --minify

# Update Hextra to latest
hugo mod get -u github.com/imfing/hextra
hugo mod tidy

# Reset browser theme override during dev (paste into DevTools console)
localStorage.removeItem('color-theme'); location.reload();
```

## Where Hextra source lives (for diagnostics)

When something looks wrong, inspect the actual theme template in the Hugo
module cache rather than guessing:

```
%LOCALAPPDATA%\hugo_cache\modules\filecache\modules\pkg\mod\github.com\imfing\hextra@v0.12.2\
```

Key partials:
- `layouts/_partials/navbar-title.html` — logo + site title rendering
- `layouts/_partials/toc.html` — "On this page" sidebar
- `layouts/_partials/footer.html` — footer (copyright, powered-by)

## Out of scope

- Marketing site copy (lives at `https://www.upgrademate.io`, separate codebase).
- Blog / changelog / about pages — explicitly removed; the site is docs-only.
- Light-mode brand palette — not yet defined; Hextra defaults are acceptable.
