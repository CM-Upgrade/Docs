# UpgradeMate Brand Guidelines

> Single source of truth for visual identity across the documentation site
> (and any future UpgradeMate web property built on Hugo + Hextra).
>
> All values below are mirrored as CSS custom properties in
> [`assets/css/custom.css`](../assets/css/custom.css) — edit one place,
> the site updates everywhere.

---

## 1. Color palette

### Core dark blues — backgrounds

| Role | Hex | RGB | CSS variable |
| ---- | --- | --- | ------------ |
| Main background | `#0F2033` | 15, 32, 51 | `--um-bg` |
| Card / panel background | `#162D46` | 22, 45, 70 | `--um-bg-elevated` |
| Subtle hover surface | `#1B3556` | 27, 53, 86 | `--um-bg-hover` |
| 1px borders / dividers | `#233A5C` | 35, 58, 92 | `--um-border` |

### Accent colors

| Role | Hex | Usage | CSS variable |
| ---- | --- | ----- | ------------ |
| **Action Blue** | `#0078D4` | Primary CTAs, links, focus rings, "primary button" | `--um-action` |
| Action Blue (hover) | `#2A8FE0` | Hover state for primary | `--um-action-hover` |
| **Success Green** | `#92C353` | Positive features, completed states | `--um-success` |
| **Warning Orange** | `#EB6B23` | Critical alerts, blockers | `--um-warning` |

### Text

| Role | Hex | Usage | CSS variable |
| ---- | --- | ----- | ------------ |
| Header text | `#FFFFFF` | All `h1`–`h6` headings | `--um-text` |
| Body text | `#A0B1C5` | Paragraph copy, list items | `--um-text-body` |
| Muted text | `#7B8CA3` | Captions, metadata, timestamps | `--um-text-muted` |

> **Hextra primary mapping.** Hextra/Tailwind reads HSL components, not hex, for
> its primary color. `#0078D4` decomposes to:
> ```css
> --hextra-primary-hue:        207deg;
> --hextra-primary-saturation: 100%;
> --hextra-primary-lightness:  42%;   /* 52% in dark mode for AA contrast */
> ```
> If you change `--um-action`, also update these three values to match.

---

## 2. Typography

| Token | Value | Usage |
| ----- | ----- | ----- |
| Primary font | **Segoe UI**, fallback **Inter**, then system sans | Body and headings |
| Monospace font | Cascadia Code → Fira Code → JetBrains Mono → Consolas | Code blocks, inline `code` |
| H1 | Bold, ~32pt | Page titles |
| H2 | Semibold, ~24pt | Major sections |
| Body | Regular, 16pt | Default paragraph copy |
| Chart / table text | Regular, 12pt (~`0.875rem`) | Tables, captions |

CSS variables:

```css
--um-font-sans: "Segoe UI", "Inter", system-ui, -apple-system,
                "Helvetica Neue", Arial, sans-serif;
--um-font-mono: "Cascadia Code", "Fira Code", "JetBrains Mono",
                Consolas, "Courier New", monospace;
```

---

## 3. Visual style

### Geometry

| Token | Value | Usage |
| ----- | ----- | ----- |
| `--um-radius-sm` | `4px` | Inline code chips, small badges |
| `--um-radius-md` | `8px` | Cards, code blocks, buttons, callouts, tables |

> **Brand rule:** every container uses **4 px or 8 px** corner radius — never
> sharp corners, never pill shapes (except inline code chips).

### Iconography

- Minimalist line icons, derived from the **'OK' / 'HWBlocked'** style.
- Use Hextra's built-in icon set first (`{{< icon name="..." >}}`).
- For brand-specific icons, drop SVG files into `static/images/icons/` and
  reference inline with raw HTML in markdown.

### Elevation & depth

- Default surface: `--um-bg`.
- Elevated surface: `--um-bg-elevated` (no shadow needed — color alone signals depth).
- Optional drop shadow for raised CTAs:
  `box-shadow: 0 4px 12px rgba(0, 120, 212, 0.25);`

---

## 4. Brand assets

Logo files live under [`static/images/`](../static/images/) so they are served
verbatim at `/images/...`.

| File | Size | When shown |
| ---- | ---- | ---------- |
| `UpgradeMateLogoBig.png` | 2200 × 500 | Original (high-res, light variant) — **do not touch** |
| `LogoBig_Dark.png` | 2200 × 500 | Original (high-res, dark variant) — **do not touch** |
| `UpgradeMateLogo-navbar.png` | 280 × 64 | Navbar logo, **light mode** |
| `UpgradeMateLogo-navbar-dark.png` | 280 × 64 | Navbar logo, **dark mode** |

Configured in [`hugo.yaml`](../hugo.yaml) under `params.navbar.logo`:

```yaml
navbar:
  logo:
    path: images/UpgradeMateLogo-navbar.png        # light mode
    dark: images/UpgradeMateLogo-navbar-dark.png   # dark mode
    width: 140
    height: 32
    link: "https://www.upgrademate.io"
```

### Resizing recipe (PowerShell)

For new logo / favicon sizes, use this snippet — the `& { ... }` block keeps
GDI+ objects alive:

```powershell
& {
  Add-Type -AssemblyName System.Drawing
  $src = 'static\images\UpgradeMateLogoBig.png'
  $dst = 'static\images\UpgradeMateLogo-<size>.png'
  $tw  = 280   # target width in px

  $img = [System.Drawing.Image]::FromFile((Resolve-Path $src))
  $th  = [int][Math]::Round($img.Height * ($tw / $img.Width))
  $bmp = New-Object System.Drawing.Bitmap $tw, $th
  $g   = [System.Drawing.Graphics]::FromImage($bmp)
  $g.InterpolationMode  = [System.Drawing.Drawing2D.InterpolationMode]::HighQualityBicubic
  $g.SmoothingMode      = [System.Drawing.Drawing2D.SmoothingMode]::HighQuality
  $g.PixelOffsetMode    = [System.Drawing.Drawing2D.PixelOffsetMode]::HighQuality
  $g.CompositingQuality = [System.Drawing.Drawing2D.CompositingQuality]::HighQuality
  $g.Clear([System.Drawing.Color]::Transparent)
  $g.DrawImage($img, 0, 0, $tw, $th)
  $bmp.Save((Join-Path $PWD $dst), [System.Drawing.Imaging.ImageFormat]::Png)
  $g.Dispose(); $bmp.Dispose(); $img.Dispose()
}
```

---

## 5. Helper classes (markdown-friendly)

These are defined in [`assets/css/custom.css`](../assets/css/custom.css) and
work in any `.md` file because `markup.goldmark.renderer.unsafe: true` is set.

```html
<div class="brand-callout">Branded note with action-blue accent</div>
<span class="brand-success">✓ Migration complete</span>
<span class="brand-warning">⚠ Breaking change in v2</span>
<span class="brand-action">Click here</span>
```

---

## 6. Theme defaults

- **Default mode:** dark (`params.theme.default: dark` in `hugo.yaml`).
- **Toggle:** visible in the navbar; user preference persists in `localStorage`
  under key `color-theme`. To reset during testing:
  ```js
  localStorage.removeItem('color-theme'); location.reload();
  ```
- **Light mode** uses Hextra defaults plus the brand action blue and Segoe UI
  font. A full light palette is **not yet defined** in the brand guidelines.

---

## 7. Editing checklist

When evolving the brand:

1. Update hex / token values at the top of
   [`assets/css/custom.css`](../assets/css/custom.css) (`:root` block).
2. If `--um-action` changed, also update the `--hextra-primary-*` HSL trio.
3. Keep this file in sync — it is the human-readable spec.
4. Hard-refresh the browser (`Ctrl+F5`) to clear cached CSS.
5. Test **both** light and dark modes before committing.
