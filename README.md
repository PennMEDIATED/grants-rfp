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
## Embedding this page

WordPress renders the real site; this repo is the source. The launch plan is direct-to-disk deployment, which needs no iframe — but iframe embedding still works and is the documented fallback, so keep this snippet accurate if you rename the repo or change its Pages URL.

Paste into a **Custom HTML block** as one line. The site runs a WordPress block theme (Twenty Twenty-Four / Twenty Twenty-Five), so a Custom HTML block has no width control of its own — wrap it in a **Group block set to Full width** if the page should run edge to edge, otherwise the theme constrains it to the `contentSize` from `theme.json` and the page renders in a narrow column:

```html
<iframe id="pm-grants-rfp" src="https://pennmediated.github.io/grants-rfp/" title="Grants Request for Proposals — Penn MEDIATED" loading="lazy" style="width:100%;height:1250px;border:0;display:block"></iframe><script>(function(){var f=document.getElementById('pm-grants-rfp');window.addEventListener('message',function(e){if(e.source!==f.contentWindow)return;var d=e.data||{},h=d.frameHeight||(d.type==='partners-page-resize'?d.height:0);if(h)f.style.height=h+'px';});})();</script>
```

The `height` in the snippet is only the starting value. Every Penn MEDIATED page posts its real height to the parent as `{ frameHeight: <int> }` — on load, on resize, once webfonts settle, and on any `ResizeObserver` change, so reveal animations, expanding cards and `<details>` toggles all resize the frame. The listener in the snippet applies it. `grants-rfp` also emits an older `{ type: 'partners-page-resize', height }` message; the snippet accepts both.

The page checks `window.self === window.top` before posting, so opening it directly does nothing. If you add a new page repo, copy the script from the bottom of this `index.html` so it behaves the same way.


## Images and video

This applies to every image, GIF and video added to any Penn MEDIATED repo. It is written to be followed directly — by a person or by a Claude session — without further instruction.

### The one rule that is never optional

**Every `<img>` and `<video>` carries explicit `width` and `height` attributes, holding the file's real intrinsic pixel dimensions.**

```html
<img src="assets/example.webp" width="640" height="334" alt="…">
```

They do not set the display size — CSS does. They give the browser the aspect ratio *before* the file downloads, so it reserves a correctly shaped box instead of collapsing to nothing and shoving everything below it down the page as each file lands. That shift is measured by search engines (Cumulative Layout Shift) and is worse for a reader, who loses their place or clicks a link that just moved.

Every repo has a global `img, video { max-width: 100%; height: auto; display: block; }` reset, so the CSS keeps winning and the attributes only ever contribute the ratio. **Never guess the numbers** — read them off the file.

### Pick the format by what the file is

| Content | Format | Never use |
| --- | --- | --- |
| Photo, screenshot, artwork | **WebP**, quality 88 | PNG or JPEG at full camera resolution |
| Logo, wordmark, icon | **SVG** if you have it, else WebP | — |
| Anything that moves | **MP4** (H.264) + a WebP poster | **GIF, ever** |

GIF is the big one. It has no interframe compression, so a screen recording is roughly ten times the size it needs to be: `research-compendium.gif` was 11.3MB for 290 frames; the identical recording as H.264 is 1.2MB.

### Size it to the box it displays in, not to what you were sent

Find the CSS box the image renders into, then export at **2×** that width for retina. Anything beyond that is bytes the browser downloads and immediately throws away. (`gni-membership.png` was 7992px wide, rendering into a 319px box — a 470KB file doing a 33KB job.)

In this repo:

| Where | CSS box at 1440px | Export at |
| --- | --- | --- |
| School logo (`.school-block__logo`) | up to 226px wide, height-capped | ~452px, or SVG |

If you are adding an image somewhere not listed, measure the box first (`getBoundingClientRect().width` in the browser, at a 1440px viewport) and double it.

### Commands

Stills — resize and convert in one pass:

```python
from PIL import Image
TARGET = 640                      # 2x the CSS box
im = Image.open('source.png')
w, h = im.size
if w > TARGET:
    im = im.resize((TARGET, round(h * TARGET / w)), Image.LANCZOS)
im.save('out.webp', quality=88, method=6)
print(im.size)                    # <- these are the width/height attributes
```

Animation — MP4 plus a poster frame:

```bash
ffmpeg -i source.gif -movflags +faststart -pix_fmt yuv420p \
       -vf "scale=1280:-2:flags=lanczos" -crf 24 out.mp4
ffmpeg -i source.gif -frames:v 1 -vf "scale=1280:-2:flags=lanczos" poster.png
python3 -c "from PIL import Image; Image.open('poster.png').convert('RGB').save('out-poster.webp', quality=80, method=6)"
ffprobe -v error -show_entries stream=width,height -of default=nw=1 out.mp4
```

`-crf 24` is a good default; raise it toward 30 for a smaller file, lower it toward 20 for a sharper one. `-pix_fmt yuv420p` is required for Safari and iOS.

### Markup for video

```html
<video src="assets/name.mp4" poster="assets/name-poster.webp" width="1280" height="622"
       autoplay muted loop playsinline preload="metadata" aria-label="…"></video>
```

Each attribute earns its place: `muted` is what permits autoplay at all, `playsinline` stops iOS opening it fullscreen, `poster` means the slot is never empty while the video loads, and `aria-label` replaces `alt` (a `<video>` has no `alt`).

CSS cannot stop autoplay, so **a page with video needs the reduced-motion script** at the end of `<body>`. If the page already has one, leave it alone; if you are adding the first video to a page, add it:

```html
<script>
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    document.querySelectorAll('video[autoplay]').forEach(function (v) {
      v.autoplay = false; v.pause(); v.currentTime = 0; v.removeAttribute('loop');
    });
  }
</script>
```

Also check the CSS: any rule that sizes or crops an image needs to name `video` too, or the video slot will not match the image slot it replaced (`.card__image img` becomes `.card__image img, .card__image video`).

### Before you call it done

- [ ] File is WebP, SVG or MP4 — no GIF, no full-resolution PNG or JPEG
- [ ] Its width is about 2× the CSS box it renders into
- [ ] `width`/`height` attributes match the file's real dimensions
- [ ] Real `alt` text (or `aria-label` on a video) that describes the image; empty `alt=""` only if it is purely decorative
- [ ] Lives in this repo's `assets/`, not hotlinked from another site
- [ ] Page opened in a browser at 1440px and ~400px — nothing overflows, nothing jumps on load
- [ ] Originals are not committed alongside the optimised file; git history is the backup

Do not commit an unoptimised original "just in case" — the previous commit already holds it, and a duplicate in the working tree also ships to the server.

## Hyperlinks

One taxonomy, five categories, shared by every page repo. Pick the category by what the link *is*, not by which repo you happen to be editing.

**1. In-text links** — embedded mid-sentence in flowing prose.

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-red-dark` | none | fade to `opacity: 0.7` |
| colour / gradient | `--c-white` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` |

Both grounds use `font-weight: 500` and `transition: opacity 0.15s`, and both fade rather than change hue. On a white ground **colour is the affordance** — no underline; the underline is category 2's job. On a coloured ground the red is invisible, so the link goes white and takes the hairline rule instead. Where an underline is used it is a `border-bottom`, never `text-decoration`.

#### Why interactive red is `--c-red-dark`, not `--c-red`

`--c-red-dark` (`#df3611`) is the closing stop of `--c-gradient`, promoted to a token of its own and declared in all twelve repos.

`--c-red` (`#f03d1f`) measures roughly **3.9:1** against white — under the 4.5:1 WCAG AA threshold for body text, and the same 3.9:1 applies to white text sitting on a `--c-red` fill. `--c-red-dark` measures about **4.5:1** either way and clears it. The two are near-indistinguishable at text sizes, so this is a contrast fix, not a visual change.

**The rule: anything you click is `--c-red-dark`.** Links and buttons take it wherever they would otherwise be red-orange — as text colour, as a box fill, as a hover or active state, and on the markers inside them (disclosure chevrons and their labels). It applies in every category and every state.

**`--c-red` stays the brand accent for everything you don't click**: section headings, eyebrow and metadata labels, tag and pill backgrounds, accent bars and card borders, full-width colour bands, the `.card-arrow` hover gradient, and focus rings. These are either large text, non-text UI at the 3:1 threshold, or sit on a tinted rather than white ground.

The one deliberate hold-out is red link text on a **dark** ground (`home`'s `.footer__email`), where the darker red would *reduce* contrast rather than improve it. That link has a separate outstanding issue — on a dark ground the standard is white text with an opacity fade, not red at all.

**2. Independent links** — a standalone text link that isn't inside a sentence ("Learn More About the Center", "Download the Full Schedule"). Unlike category 1 these carry the underline and are set in the body colour, so they read as a control rather than as emphasis inside a sentence:

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-dark`, `font-weight: 600` | `border-bottom: 1px solid rgba(13, 13, 12, 0.35)` | text and underline both turn `--c-red-dark` (`transition: color 0.15s, border-color 0.15s`) |
| colour / gradient | `--c-white`, `font-weight: 600` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` |

Plus a **thin arrow** `⟶` after the text. Use `⟶` (`&#10230;`), not the `↗` badge from category 4.

**3. Document buttons** — an independent link that opens a document (a PDF, a report). A filled button box, not text:

| ground | box | text |
| --- | --- | --- |
| white / light | `--c-red-dark` | `--c-white` |
| colour / gradient | `--c-white` | `--c-dark` |

Hover is **movement, not colour** — a lift or nudge. Do not darken or recolour the box.

**4. Links to another web page** — this site or an external one. The containing box carries the shared `.card-arrow`: a 26px dark circle with a white `↗`, in the box's top corner. On hover the arrow scales slightly and its background becomes a sliding purple-to-orange gradient (`@keyframes card-arrow-slide`), and the box itself animates. No separate text button — the whole box is the link.

**Exception:** a link to a research paper is category 2, not this — thin arrow, no badge.

**5. Hyperlinked headings** — a heading that is itself a link (a post title, a card title). Sits in the body colour and shifts to `--c-red-dark` on hover (or fades, on a coloured ground), with **no arrow and no underline**.

### Dropdowns and disclosures

A dropdown, `<details>` block or expand/collapse control uses one affordance sitewide: a **chevron SVG** (`M2 5l5 5 5-5`, 13×13, `--c-red-dark` stroke, `stroke-width: 1.8`) beside a `--c-red-dark` label at `--fs-small`, rotating `180deg` on open with `transition: transform 0.25s`. See `llm-civic-discourse`'s "Full summary & details" toggle for the reference implementation.

Never leave the marker to the browser — style `<select>` with `appearance: none` and supply the chevron, and hide the native `<summary>` marker. The `↗` circle badge is category 4's language and does not belong on a disclosure control.

- **Static vs. collapsible boxes**: `.rfp-archive__box--static` (used for the "Coming Soon" 2026 box) reuses the collapsible box's markup, permanently in its "open" state — no arrow, no pointer cursor, content always visible. It carries no panel of its own: it sits directly on the orange band, so it has no background, border or padding, and the band's `--space-1000` padding plus `.rfp-archive__inner`'s `--pad-x` do the insetting. Copy on a colored band goes `--c-white`, including inline links, since the red link color is invisible against `--c-red` — the same treatment as `team-leadership`'s `.team-section--gradient`. Use this modifier for any future box that shouldn't be collapsible rather than inventing a new component. Its copy is all one size — the lead-in `.rfp-archive__subhead` drops to `--fs-body` there and carries its emphasis with weight alone, since the block is a short note rather than a document. The collapsible 2025 box keeps `--fs-lede` subheads, where they head real sections of RFP content.
- **Framed card (`.rfp-archive__past`)**: a bordered, softly-shadowed white card (DM Sans 700 title stacked above content) used to set the "Past Requests for Proposals" section apart from the plain page background. `border: 1px solid rgba(13,13,12,0.08)` + `box-shadow: 0 20px 60px rgba(13,13,12,0.08)` + `var(--space-800)` padding, stepping to `--space-400` under 900px and `--space-250` under 480px — the same white-block treatment as `home`'s `.about-center__card` and `grants-overview`'s `section.body-section`. All three carry identical values; change one and change the others.

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

**Heading weight.** 700 on white backgrounds (`.rfp-archive__past-title`), 600 on colored blocks (`.rfp-archive__title` on the orange band). Page titles stay EB Garamond 600 in `--c-accent`.

**Section rhythm.** A full-width colored section carries `var(--space-1000)` (80px) top and bottom padding, so its heading never sits flush against the band's edge. The page hero's bottom padding is `var(--space-600)` (48px) — shorter than 80px because the section below supplies its own.
