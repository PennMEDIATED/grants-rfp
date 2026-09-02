# Penn MEDIATED — Grants RFP

The "About MEDIATED Grants" page for the [Center on Media, Technology and Democracy](https://infodem.upenn.edu). Static HTML/CSS, no build step. Introduces the Grants program, surfaces the current (2026) request for proposals, and archives past (2025) RFP guidelines — an interactive dashboard elsewhere on the site covers the funded-grants data itself.

Same conventions as the [`about`](https://github.com/PennMEDIATED/about), [`home`](https://github.com/PennMEDIATED/home), and [`grants`](https://github.com/PennMEDIATED/grants) repos — shared spacing tokens, brand colors, and fonts. This page intentionally has no top nav bar and no newsletter/supporters block; embedding via WordPress iframe is handled by the resize script at the bottom of `index.html`.

- `index.html` — page markup
- `styles.css` — all styling (design tokens live at the top in `:root`)
- `assets/` — logos (school crests, funder logos), shared with `about`/`home`/`grants` where applicable

## Style guide (shared across `about`, `home`, `grants`, and this repo)

All four repos are static HTML/CSS built off the same design system. If you're adding or editing anything, pull values from here rather than guessing new ones — that's what keeps the sites looking like one brand instead of drifting apart.

### Design tokens (`:root` in `styles.css`)

**Spacing** — Atlassian's 8px scale. Always use the variable, never a raw pixel value:

```
--space-025: 2px   --space-100: 8px   --space-300: 24px  --space-600: 48px
--space-050: 4px   --space-150: 12px  --space-400: 32px  --space-800: 64px
--space-075: 6px   --space-200: 16px  --space-500: 40px  --space-1000: 80px
--space-250: 20px
```

**Color:**

| Token | Hex | Use |
|---|---|---|
| `--c-bg` | `#ffffff` | Page background |
| `--c-dark` | `#0d0d0c` | Primary text, dark backgrounds |
| `--c-accent` | `#5533ee` | Brand purple |
| `--c-red` | `#f03d1f` | Brand red/orange — the orange RFP band, tags, arrow-badge gradient |
| `--c-gray` | `#888680` | Secondary/muted text |
| `--c-gray-dark` | `#54534f` | ~8:1 contrast on white — available, but this repo uses `--c-dark` for body copy instead, matching `grants` |
| `--c-light-bg` | `#f8f7f4` | Dropdown box background, neutral card backgrounds |
| `--c-pale-orange` | `#fce4dc` | `--c-red` tinted ~12% onto white — callout boxes (Secondary Goals cards, eligible-schools grid, closed-status tag) |
| `--c-white` | `#ffffff` | — |

**Brand gradient** — used on every purple-to-red surface (see `about`'s orbital section, `home`'s newsletter block): `linear-gradient(150deg, #5533ee 0%, #df3611 81%)` via `--c-gradient`. This repo doesn't have a full-bleed gradient surface, but reuses the same purple→red pairing for the dropdown arrow badge's hover animation (see Shared components below). Never write the gradient stops out by hand — reference the variable, or the two colors it interpolates between, so a future palette tweak only has to happen in one place per repo.

**Type:**
- `--f-serif`: `'EB Garamond', Georgia, 'Times New Roman', serif` — page-level headlines only (`.intro__title`, `.rfp-archive__past-title`), quotes, the "MEDIATED" wordmark. Not used for mid-page section headings.
- `--f-sans`: `'DM Sans', system-ui, -apple-system, sans-serif` — everything else, including mid-page section headings (`.rfp-archive__title`), small meta labels, and body copy.

**Layout:** `--max-w: 1440px` page cap, `--pad-x: var(--space-1000)` (80px) side padding on the shared `*__inner` containers, scaling down responsively to `--space-400` (32px) under 900px and `--space-250` (20px) under 480px — same breakpoints as `home`/`grants`. Grid and flex children shrink below their content: grid tracks are `minmax(0, 1fr)` rather than `1fr`, and flex items that hold text carry `min-width: 0`. Without those, a track or item is pinned to its widest child and pushes the page wider than the viewport on small screens.

### Layout conventions

- Every section's content wrapper is named `.<section>__inner` and shares one rule (`width:100%; max-width:var(--max-w); margin-inline:auto; padding-inline:var(--pad-x);`). Add new sections to that shared selector list instead of writing a one-off inner container.
- Section-to-section vertical rhythm uses `--space-1000` (80px) for genuine color/background transitions and for a section that reads as its own standalone white section (e.g. `.rfp-archive__below`, which mirrors `home`'s `.whats-new` padding exactly: 80px top, 64px bottom).
- A heading immediately followed by body copy uses a flat `--space-300` (24px) gap where practical.
- BEM-ish naming: `.block__element`, modifiers as `.block--variant` or `.block__element--variant` (e.g. `.school-block__logo--seas`, `.tag--closed`, `.rfp-archive__box--static`).
- **No eyebrow/kicker label above hero or section headings, anywhere, no exceptions.** This is a hard sitewide rule (see `about`/`team-leadership` READMEs, which had one removed) — this repo does not use one above `.intro__title`, `.rfp-archive__title`, or `.rfp-archive__past-title`. If a future edit is tempted to add one back (e.g. a small "Grants" label above the H1), don't — it's been tried and explicitly removed twice.

### Shared components

- **Numbered lists**: `.criteria-list__num` and `.req-list__num` use the same treatment — `--f-sans`, weight 800, `--c-red`, `align-items: baseline` on the row — rather than a fixed-position badge.
- **Pale-orange callout boxes**: the Secondary Goals cards, the eligible-schools grid, and the "Closed" status tag all share `background: var(--c-pale-orange)`. One consistent tint for "supplementary info," distinct from `--c-light-bg`'s neutral gray (used for the dropdown boxes themselves).
- **Logo grid (`.school-grid`)**: a 4-column (2-col tablet, 1-col mobile) grid of bordered white tiles. These are static, non-interactive display tiles — no external links, no hover state, no arrow badge. (This is a deliberate departure from `about`'s and `grants`' equivalent logo grids, which do link out and use the `.card-arrow` hover treatment — this repo's eligibility grid is illustrative only, not a set of external links.)
- **Image sizing — avoid the squash bug**: size logos with `max-height` + `max-width: 100%` and `width: auto; height: auto;` — not a fixed `height` + `max-width: 100%` pair, which distorts wide lockups once the grid column narrows enough to hit the `max-width` cap while height stays pinned. Use a `--modifier` class (e.g. `.school-block__logo--seas`) to bump `max-height` for a lockup that reads too small next to its neighbors at the shared size.
- **Dropdown arrow badge**: `.rfp-archive__summary::after` is a 26px circular badge (dark background, white glyph) — the same visual language as `about`'s/`grants`' `.card-arrow` external-link badge, but pointing down (▾, rotating 180° when the box is `[open]`) instead of diagonal, since this signals an in-page toggle rather than an external link. On hover of the box, the badge switches to the same continuously sliding purple-to-red gradient used by `.card-arrow` (`linear-gradient(100deg, var(--c-accent), var(--c-red), var(--c-accent))`, animated via `background-position`) rather than a flat color swap — keep this consistent if either badge changes.
- **Dropdown box (`.rfp-archive__box`)**: neutral border (`rgba(13,13,12,0.08)`) at rest and on hover — no border-color change on hover (the color signal lives entirely in the arrow badge now, not the box border).
- **Static vs. collapsible boxes**: `.rfp-archive__box--static` (used for the "Coming Soon" 2026 box) reuses the exact same box/header markup and look as a collapsible box, permanently in its "open" state — no arrow, no pointer cursor, content always visible. Use this modifier for any future box that shouldn't be collapsible rather than inventing a new component.
- **Framed card (`.rfp-archive__past`)**: a bordered, softly-shadowed white card (title stacked above content) used to set the "Past Requests for Proposals" section apart from the plain page background. `border: 1px solid rgba(13,13,12,0.08)` + `box-shadow: 0 24px 48px rgba(13,13,12,0.06)`.

### Keeping the repos in sync

`about`, `home`, `grants`, and this repo are separate repos with duplicated CSS, not a shared stylesheet — so consistency is a discipline, not something enforced automatically. When you change a shared token or component in one repo, check whether the same change belongs in the others before considering the task done.

## Typography

Sitewide convention. The `--fs-*`/`--lh-*` block at the top of `styles.css` is canonical and identical in every page repo.

**Two families, no third.** `--f-serif` (EB Garamond) for page and section titles and pull-quote copy; `--f-sans` (DM Sans) for everything else. There is no monospace face — uppercase micro-labels are DM Sans 700 uppercase with `letter-spacing: 0.08em`.

**Sizes come from tokens, never raw px.**

| Token | Mobile (=<480px) | Desktop (>=1440px) | Used for |
| --- | --- | --- | --- |
| `--fs-display` | 36px | 76px | full-bleed hero |
| `--fs-h1` | 36px | 56px | page title |
| `--fs-h2` | 26px | 40px | section titles |
| `--fs-h3` | 20px | 24px | card and third-level titles |
| `--fs-lede` | 18px | 20px | intro paragraphs |
| `--fs-body` | 16px | 16px | body copy |
| `--fs-small` | 14px | 14px | captions, meta, form controls |
| `--fs-small-serif` | 15px | 15px | EB Garamond at small sizes |
| `--fs-micro` | 12px | 12px | uppercase labels, tags, counts |

The top five are `clamp()` values that interpolate across the viewport, so tablet widths need no separate `@media` override. Only add a breakpoint font-size when a specific layout actually demands it.

**12px is the floor.** Nothing ships smaller. EB Garamond and uppercase-with-letter-spacing both read smaller than their nominal size, which is what `--fs-small-serif` and the 12px floor exist to absorb.

**Line heights are tokens too** — `--lh-display` 1.05, `--lh-heading` 1.15, `--lh-lede` 1.26, `--lh-title` 1.3, `--lh-body` 1.55. Never set a line-height in px; it breaks the fluid sizes.

**Heading gaps.** Section title to first content is `var(--space-300)` (24px); page or hero title to content is `var(--space-250)` (20px).

**Section rhythm.** A full-width colored section carries `var(--space-1000)` (80px) top and bottom padding, so its heading never sits flush against the band's edge. The page hero's bottom padding is `var(--space-600)` (48px) — shorter than 80px because the section below supplies its own.
