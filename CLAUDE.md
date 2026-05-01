# lintune-www — Claude Context

## What this repo is
Marketing website and asset pack for [lintune.xyz](https://lintune.xyz) — the public-facing site for the Lintune MSP platform.

## Repo structure
```
website/
  index.html          — Main landing page (light theme)
  lintune-dark.html   — Dark theme variant (not actively maintained)
  install.html        — Install docs page (fetches install.md from GitHub at runtime)

favicon/              — SVG favicons, light + dark, 16/32/48px variants
lockup/               — Horizontal and stacked logo lockups, light + dark
mark/                 — Mark-only SVGs (the four-rect icon), light + dark
wordmark/             — Wordmark-only SVGs, light + dark
```

## Tech stack
- Plain HTML + CSS + JS, no build step, no framework.
- `install.html` fetches `install.md` from `raw.githubusercontent.com` at runtime and renders it with [marked.js](https://marked.js.org/) (CDN). Relative links in the markdown are rewritten to absolute GitHub blob URLs after render.
- Fonts: Geist (sans) + Geist Mono via Google Fonts / CDN.

## Design system
| Token   | Light     | Dark      |
|---------|-----------|-----------|
| Ink     | `#0e0f12` | `#fafaf7` |
| Paper   | `#fafaf7` | `#0e0f12` |
| Green   | `#16a34a` | `#22c55e` |
| Muted   | `#888`    | `#888`    |
| Border  | `#eee`    | (dark equivalent) |

Font stack: `'Geist Mono', ui-monospace, monospace` for nav, meta, footer. `'Geist', sans-serif` for body/headings.

## Key UI patterns
- `.menu a` — nav links: `color: inherit` (#888), no underline, hover to ink color. CTA button is ink bg + paper text.
- `.hero .meta a` — inline service links (keycloak, mailcow, nextcloud) share the same style as `.menu a`.
- All pages wrap content in a `.browser` chrome (traffic lights + URL bar) to simulate a browser window.
- Fixed `.alpha-banner` at top warns about alpha status.

## Service links in index.html
The `.hero .meta` line links to the three upstream projects:
- `keycloak` → https://www.keycloak.org/
- `mailcow` → https://mailcow.email/
- `nextcloud` → https://github.com/nextcloud/all-in-one (AIO)

## Logo geometry (viewBox 0 0 64 64)
```
rect 14,10  14×12  ink   ← Keycloak
rect 14,24  14×12  ink   ← Mailcow
rect 14,38  14×12  ink   ← Nextcloud
rect 30,38  20×12  green ← Lintune (binding stub)
```

## install.html — docs fetching
- Source: `https://raw.githubusercontent.com/scraane/lintune-admin/refs/heads/dev/docs/install.md`
- After `marked.parse()`, a JS post-process rewrites relative `href` values to `https://github.com/scraane/lintune-admin/blob/dev/docs/<href>` so "Further reading" links work.
- This approach (fetch at runtime) is intentional — no build pipeline, always in sync with the repo.

## What NOT to do
- Don't add a build step or bundler — the site is intentionally zero-dependency static HTML.
- Don't maintain the dark theme variant (`lintune-dark.html`) — it exists for reference only.
- Don't hardcode markdown content in `install.html` — it should always pull from the GitHub source.
