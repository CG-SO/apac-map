# CGIAR.org — Design System Reference

A base reference for building CGIAR.org interfaces. Everything below is transcribed
from the production front-end style guide (`ui.min.css` + the per-brand token files),
not invented. Use it as the source of truth for tokens, class names and markup shape.

- **Stylesheet:** `assets/css/ui.min.css` (all components + semantic tokens)
- **Brand token layers:** `assets/css/props-cgiar.css`, `props-africarice.css`, `props-worldfish.css`
- **Icon sprite:** `assets/icons.svg` (48 single-stroke icons)
- **Root font size:** `62.5%` → **1rem = 10px**. All token values below are given in rem with px in parentheses.

```html
<link rel="stylesheet" href="assets/css/ui.min.css">
<link rel="stylesheet" href="assets/css/props-cgiar.css">
<html data-theme="cgiar">
  <body data-area="light"> … </body>
</html>
```

---

## 1. Architecture

Three layers, in order of precedence:

1. **Primitive tokens** — raw ramps and dimensions (`--color-brand-50`, `--dimension-16`, `--s5`). Live in the brand props file. Never referenced directly in components.
2. **Semantic tokens** — role-named aliases (`--color-foreground-subtle`, `--color-background-solid-neutral-strong`). Live in `ui.min.css` and **remap depending on `data-area`**.
3. **Components** — BEM classes prefixed by atomic level: `a-` atoms, `m-` molecules, `o-` organisms, `cms-` CMS content modules, `h-` helpers, `state-` state modifiers.

### Two switches control everything

| Attribute | Values | Effect |
|---|---|---|
| `data-theme` | `cgiar`, `africarice`, `worldfish` | Swaps the brand ramp, radii and nav colors |
| `data-area` | `light`, `dark` | Re-points every semantic token for that surface |

`data-area` is nestable. Put `data-area="dark"` on any section and all descendants
inherit the dark mapping — components need no dark variants of their own.

---

## 2. Color

### 2.1 Brand ramps (11 steps, per theme)

| Step | CGIAR | Africa Rice | WorldFish |
|---|---|---|---|
| 5 | `#e6fff9` | `#f8fff5` | `#eef2ff` |
| 10 | `#9dffe8` | `#ffee8d` | `#d6e1ff` |
| 20 | `#58fbd4` | `#ffee8d` | `#adc3ff` |
| 30 | `#18f2be` | `#ffee8d` | `#85a6ff` |
| 40 | `#08d9a8` | `#95dd7d` | `#5c89ff` |
| 50 | `#00b389` | `#77bb60` | `#336cff` |
| 60 | `#008c6b` | `#5b9946` | `#1a4dcc` |
| 70 | `#06604b` | `#427730` | `#153999` |
| 80 | `#033529` | `#0b5823` | `#0b2566` |
| 90 | `#02221a` | `#08481c` | `#040f33` |
| 99 | `#010d0a` | `#001900` | `#020716` |

### 2.2 Neutral ramp (shared)

`--color-solid-grey-0` `#fff` · `10` `#fafafa` · `20` `#f5f5f5` · `30` `#ececec` ·
`40` `#d9d9d9` · `50` `#cdcdcd` · `60` `#a8a8a8` · `70` `#747474` · `80` `#393939` ·
`90` `#1e1d1d` · `99` `#080808` · `100` `#000`

### 2.3 Dim ramp (brand alpha overlays)

`--color-dim-0 … --color-dim-90` — the brand accent at 0 → 90 % alpha.
Base color per theme: CGIAR `#08d9a8`, Africa Rice `#00f600`, WorldFish `#336cff`.
There are also neutral `--color-dim-black-*` / `--color-dim-white-*` sets used by the
semantic layer.

### 2.4 Semantic tokens — the ones you actually write

| Token | `data-area="light"` | `data-area="dark"` |
|---|---|---|
| `--color-background-solid-neutral-strong` | grey-0 `#fff` | brand-90 |
| `--color-background-solid-neutral-default` | grey-10 | brand-80 |
| `--color-background-solid-neutral-subtle` | grey-20 | brand-70 |
| `--color-background-solid-neutral-hover` | grey-30 | brand-60 |
| `--color-background-solid-invert-default` | brand-80 | grey-10 |
| `--color-background-solid-invert-strong` | brand-90 | grey-0 |
| `--color-background-dim-neutral-default` | black 10 % | white 15 % |
| `--color-background-dim-neutral-hover` | black 15 % | white 20 % |
| `--color-foreground-emphasis` | grey-99 | grey-0 |
| `--color-foreground-default` | grey-80 | grey-10 |
| `--color-foreground-subtle` | grey-70 | grey-30 |
| `--color-foreground-solid-accent-default` | **brand-60** | **brand-20** |
| `--color-foreground-solid-accent-strong` | brand-70 | brand-10 |
| `--color-foreground-solid-accent-subtle` | brand-40 | — |
| `--color-border-default` | hairline divider (black 10 %) | white 15 % |

Note the accent inverts: on light surfaces the accent is a **deep** brand step (60);
on dark surfaces it's a **bright** one (20). Never hard-code an accent hex.

### 2.5 Feedback & focus

| Role | 400 | 500 | 600 |
|---|---|---|---|
| Green (success) | `#04e200` | `#03aa00` | `#026100` |
| Red (error) | `#f16f6f` | `#db1616` | `#910d0d` |
| Focus (CGIAR) | `#8964ff` | `#3c00ff` | `#2700a6` |
| Focus (Africa Rice / WorldFish) | `#f16f6f` | `#db1616` | `#910d0d` |

`--color-opacity-disabled: 0.6` · placeholder fill `--color-non-brand-placeholder: #e2eef1`

---

## 3. Typography

Two families, strict roles:

- **Noto Sans** (`--typography-font-families-font-1`) — headlines, UI, links, tags.
- **Noto Serif** (`--typography-font-families-font-2`) — hero display, the `-2` headline
  variants, small headlines, and long-form copy.

Weights available: Thin 100 · ExtraLight 200 · Light 300 · Regular 400 · Medium 500 ·
SemiBold 600 · Bold 700 · ExtraBold 800 · Black 900.

Line-height tokens: `lh0` 100 % · `lh1` 110 % · `lh2` 120 % · `lh3` 130 % · `lh4` 140 % · `lh5` 150 %.
Letter-spacing: `x-tight` −0.4rem · `tight` −0.1rem · `none` 0 · `wide` +0.1rem · `x-wide` +0.4rem.

### 3.1 The scale

Sizes are **fluid**: base → `md` (≥768px) → `lg` (≥1024px). Always use the class, never an ad-hoc size.

| Class | Family | Weight | Size (base → md → lg) | LH | Tracking |
|---|---|---|---|---|---|
| `h-typo-hero-l` | Serif | 300 | 8.4rem (84px), static | 120 % | −0.4rem |
| `h-typo-hero-m` | Serif | 300 | 7.2rem (72px), static | 120 % | −0.4rem |
| `h-typo-headline-xl` | Sans | 300 | 4.0 → 4.8 → 5.6rem | 120 % | −0.1rem |
| `h-typo-headline-l` | Sans | 300 | 3.2 → 3.6 → 4.0rem | 130 % | 0 |
| `h-typo-headline-l-2` | **Serif** | 400 | 3.2 → 3.6 → 4.0rem | 130 % | 0 |
| `h-typo-headline-m` | Sans | 400 | 2.4 → 2.8 → 3.2rem | 130 % | 0 |
| `h-typo-headline-m-2` | **Serif** | 400 | 2.4 → 2.8 → 3.2rem | 130 % | 0 |
| `h-typo-headline-s` | Sans | 700 | 2.0 → 2.4 → 2.8rem | 130 % | 0 |
| `h-typo-headline-xs` | **Serif** | 400 | 1.8 → 2.0 → 2.2rem | 130 % | 0 |
| `h-typo-copy-xl` | Serif | 600 *italic* | 1.8 → 2.0 → 2.2rem | 130 % | 0 |
| `h-typo-copy-l` | Sans | 400 | 1.6 → 1.8 → 2.0rem | 150 % | 0 |
| `h-typo-copy-m` | Sans | 400 | 1.4 → 1.6 → 1.8rem | 150 % | 0 |
| `h-typo-copy-m-2` | Sans | 700 | 1.4 → 1.6 → 1.8rem | 150 % | 0 |
| `h-typo-copy-s` | Sans | 400 | 1.2 → 1.4 → 1.6rem | 130 % | 0 |
| `h-typo-copy-s-2` | Sans | 700 | 1.2 → 1.4 → 1.6rem | 130 % | 0 |
| `h-typo-copy-xs` | Sans | 400 | 1.0 → 1.2 → 1.4rem | 130 % | 0 |
| `h-typo-copy-xs-2` | Sans | 700 | 1.0 → 1.2 → 1.4rem | 130 % | 0 |
| `h-typo-caption` | Sans | 400 | 1.0 → 1.2rem | 120 % | 0 |
| `h-typo-link` | Sans | 600 | 1.6rem, static | 120 % | 0 |
| `h-typo-link-s` | Sans | 600 | 1.4rem, static | 100 % | −0.1rem |
| `h-typo-tag` | Sans | 600 | 1.2rem, static | 130 % | +0.1rem |

The `-2` suffix means "the alternate cut of this role" — for headlines that's the
**serif** version, for copy it's the **bold** version.

### 3.2 Casing

- UPPERCASE for tags, eyebrows and category labels (`PUBLICATION · 2026`), with `wide` tracking.
- Sentence case for headlines and body.
- No emoji, ever. Iconography is the sprite.

---

## 4. Spacing

Ten-step scale on a 0.4rem base, fluid at two breakpoints.

| Token | base | ≥768px | ≥1024px |
|---|---|---|---|
| `--s1` | 0.4rem (4px) | — | — |
| `--s2` | 0.8rem (8px) | — | — |
| `--s3` | 1.2rem (12px) | — | — |
| `--s4` | 1.6rem (16px) | — | — |
| `--s5` | 2.0rem (20px) | — | 2.4rem |
| `--s6` | 2.4rem (24px) | — | 3.2rem |
| `--s7` | 3.2rem (32px) | 4.0rem | 4.8rem |
| `--s8` | 4.8rem (48px) | 5.6rem | 6.4rem |
| `--s9` | 6.4rem (64px) | 7.2rem | 9.6rem |
| `--s10` | 9.6rem (96px) | 11.2rem | 12.8rem |

**Spacer helpers.** Instead of margins, the system inserts empty divs:
`<div class="h-s7"></div>` renders a `--s7` vertical gap. Negative variants
`h-s1-neg … h-s10-neg` pull content back.

**Dimension scale** (`--dimension-N` = N ÷ 10 rem): 0, 1, 2, 4, 6, 8, 10, 12, 14, 16,
18, 20, 22, 24, 28, 32, 36, 40, 48, 52, 56, 60, 64, 72, 80, 84, 96, 100, 110, 112, 120,
128, 130, 150, 152, 160, 232, 280, 288, 296, 408.

**Layout.** `.container` sets `padding-inline: var(--layout-page-context)` and
`max-width: min(82vw, 1920px)`. Breakpoints in use: 768, 1024, 1440, 1540 px.

---

## 5. Radius & border

Radii are **theme-dependent** — this is the main structural difference between brands.

| Token | CGIAR | Africa Rice | WorldFish |
|---|---|---|---|
| `--dimension-border-radius-button` | 0.8rem | **0** | 0.8rem |
| `--dimension-border-radius-button-s` | 0.6rem | **0** | 0.6rem |
| `--dimension-border-radius-button-xs` | 0.4rem | **0** | 0.4rem |
| `--dimension-border-radius-card` | 0.8rem | **0** | 0.8rem |
| `--dimension-border-radius-component` | 0.8rem | 0.8rem | 0.8rem |
| `--dimension-border-radius-dropdown-menu` | 0.8rem | 0.8rem | 0.8rem |
| `--dimension-border-radius-image` | **0** | **0** | **0** |
| `--dimension-border-radius-module` | **0** | **0** | **0** |

Africa Rice runs entirely square-cornered. **Imagery is always square-cornered in every
theme.**

Border widths: `thin` 0.1rem · `medium` 0.2rem · `thick` 0.4rem. Dividers are hairline
`1px` in `--color-border-default`. Elevation is deliberately flat — **no drop shadows**;
hierarchy comes from color and rules.

---

## 6. Iconography

One SVG sprite, referenced with `<use>`. Icons inherit `currentColor` and size to `1em`
of their text context, so set `font-size` to size them.

```html
<svg class="a-icon" aria-hidden="true"><use href="assets/icons.svg#arrow-long-right"></use></svg>
```

**Full set (48):** `account` `apple` `arrow-down` `arrow-left` `arrow-long-left`
`arrow-long-right` `arrow-long-up` `arrow-right` `arrow-up` `bluesky` `budget`
`calendar` `check` `check-filled` `clock` `close` `download` `external-link` `facebook`
`filter` `gender` `globe` `home` `instagram` `jumpmark-left` `jumpmark-right` `language`
`leaf` `less` `link` `linkedin` `member` `menu` `more` `orcid` `pause` `pause-filled`
`pin` `play` `play-filled` `researchgate` `reset` `search` `shelter` `x` `youtube`
`zoom-in` `zoom-out`

`arrow-long-right` is the standard "read more / next" affordance. `more` / `less` are the
accordion +/− pair.

---

## 7. Components

Naming: `a-` atom, `m-` molecule, `o-` organism, `cms-` content module, `state-` state.
Typography is **not** baked into components — pair every component with an `h-typo-*` class.

### 7.1 Buttons — `a-button`

Variants: `--primary` `--secondary` `--tertiary` `--global`
Sizes: `--size-big` `--size-small`
State: `state-a-button--disabled` (plus the real `disabled` attribute)

```html
<button class="a-button a-button--primary a-button--size-big h-typo-link" type="button">
  <span class="a-button__text">Discover our impact</span>
  <span class="a-button__icon">
    <svg class="a-icon" aria-hidden="true"><use href="assets/icons.svg#arrow-long-right"></use></svg>
  </span>
</button>
```

Icon-only: drop `__text`. Small buttons pair with `h-typo-link-s`. Works as `<a>` or `<button>`.

### 7.2 Links — `a-link`

Modifier `a-link--nav` for header navigation. `__icon` may lead or trail `__text`.

```html
<a class="a-link h-typo-link" href="#" target="_blank">
  <span class="a-link__text">External link</span>
  <span class="a-link__icon"><svg class="a-icon"><use href="assets/icons.svg#external-link"></use></svg></span>
</a>
```

### 7.3 Tags — `a-tag` / `o-tag-collection`

```html
<ul class="o-tag-collection__list">
  <li class="a-tag"><span class="h-typo-tag">CLIMATE</span></li>
</ul>
```

### 7.4 Search field — `m-search-field`

```html
<div class="m-search-field">
  <input type="search" class="m-search-field__input" placeholder="Enter search term" aria-label="Search">
  <button class="m-search-field__btn m-search-field__btn--search" type="button" aria-label="Submit">
    <svg class="a-icon"><use href="assets/icons.svg#search"></use></svg>
  </button>
</div>
```

### 7.5 Media control — `a-control`

`__pagination` (e.g. `01 / 04`) plus `__button` children for prev / play / next.

### 7.6 Other atoms

`a-icon` sprite icon · `a-img` responsive image wrapper · `a-txt` rich-text/WYSIWYG
container (styles `p`, `ul`, `a` inside CMS content) · `a-pattern` decorative brand
pattern.

### 7.7 Molecules

`m-profile` person card · `m-result` search result row · `m-tile` grid tile.

---

## 8. Patterns (organisms & CMS modules)

### 8.1 Header — `o-header`

Sits on `data-area="dark"`. `o-header__line` holds `o-header__logo` on the left and
`o-header__nav` on the right (CGIAR System · language · Search · Menu, all `a-link--nav`).
Related: `o-header-languages`, `o-top`, `o-announcement`.

### 8.2 Navigation overlays — `o-nav*`

`o-nav` shell · `o-nav-main` + `o-nav-main-link` (primary level) · `o-nav-meta` ·
`o-nav-ecosystem` (cross-Center links) · `o-nav-search`. Opened states are
`state-o-nav-main--open` / `state-o-nav-main-link--active`, over `o-backdrop`
(`state-o-backdrop--visible`). Generic overlays use `o-overlay` / `o-overlay-content`.

The production menu reveals hierarchy one column at a time: sections first, then the
second level, then the third.

### 8.3 Stage / hero — `cms-stage`, `cms-stage-hero`, `cms-stage-hero-item`

Dark surface (`--color-special-stage-bg`), headline + lead + CTA on one third,
full-bleed documentary photography on the other. Square-cornered imagery.

### 8.4 Teasers — `cms-small-teaser`, `cms-hero-teaser`

Text-led editorial grid. Each `cms-small-teaser-item` is
`__link > __article > __headline / __text / __indicators`, separated by hairline rules.
The indicator row carries the arrow, external mark or topic icon.

### 8.5 Facts — `cms-facts-teaser`, `cms-facts-teaser-item`

Big figure in `h-typo-hero-l` tinted `--color-foreground-solid-accent-default`, then a
label in `h-typo-headline-xs`, then a supporting sentence in `h-typo-copy-s-2`.

### 8.6 Accordion — `cms-accordion-item`

```html
<div class="cms-accordion-item">
  <button class="cms-accordion-item__toggle" type="button" aria-expanded="false">
    <span class="cms-accordion-item__title"><span class="h-typo-copy-m-2">Question</span></span>
    <span class="cms-accordion-item__icon cms-accordion-item__icon--more"><svg class="a-icon"><use href="assets/icons.svg#more"></use></svg></span>
    <span class="cms-accordion-item__icon cms-accordion-item__icon--less"><svg class="a-icon"><use href="assets/icons.svg#less"></use></svg></span>
  </button>
  <div class="cms-accordion-item__content"><div class="a-txt"><p>Answer</p></div></div>
</div>
```
Open state: `state-cms-accordion-item--open`.

### 8.7 Search & discovery

`cms-search-filter` / `-group` / `-link` (faceted sidebar) · `cms-search-result` ·
`cms-search-pagination` / `-link` · `cms-collection` (result grid).

### 8.8 Breadcrumb — `o-breadcrumb`

`__list > __item > __link` with `__link-icon` (home) and `__link-text`; the current page
is `__text` with `aria-current="page"`.

### 8.9 Footer — `o-footer`

Dark. `o-footer-nav-group` + `o-footer-nav-group-link` columns,
`o-footer-nav-social` + `-social-link` icon row, `o-footer-banner` + `-logo` for the
network lockups, then `o-footer__line` for the legal row.

### 8.10 Content modules

`cms-section` (section wrapper) · `cms-image-text` · `cms-image-collection` (+ `-item`) ·
`cms-video` · `cms-lightbox` (+ `-content`, `-img`, `-caption`, `-nav`, `-prev`, `-next`,
`-close`) · `cms-download-group` / `-item` · `cms-link-list` / `-group` / `-item` ·
`cms-inline-link-list-group` · `cms-partners` · `cms-profiles-list` ·
`cms-map-countries` / `cms-map-facilities` · `cms-loader` ·
`cms-acquia-dam-*` (DAM asset cards, image containers, overlays, search module) ·
`o-inpage` (in-page jumpmark nav).

---

## 9. Multi-brand theming

One component library, many Research Centers. A brand is a **token layer only** —
components are never forked.

Each `props-*.css` file defines, scoped to `[data-theme="…"]` and `[data-theme="…"] [data-area]`:

- the 11-step brand ramp and the dim ramp
- the eight radius tokens
- focus colors
- `--brand-*` chrome tokens: `--brand-body-bg`, `--brand-body-fg`,
  `--brand-button-tertiary-bg`, `--brand-footer-bg/-fg/-fg-subtle/-hover-fg`,
  `--brand-nav-bg/-fg/-fg-divider`, `--brand-nav-main-bg/-fg/-fg-divider/-fg-meta/-fg-tag`

Notable divergences beyond color:

| | CGIAR | Africa Rice | WorldFish |
|---|---|---|---|
| Body background | neutral-**strong** | neutral-default | neutral-default |
| Header bar | dark (neutral-strong) | **light** (`--color-fixed-light`) | **light** |
| Mega-menu background | neutral-strong | brand-80 | brand-80 |
| Corners | rounded | **square** | rounded |
| Focus color | violet `#3c00ff` | red `#db1616` | red `#db1616` |

**To add a brand:** copy a props file, change the selector, supply the 11-step ramp, the
dim ramp derived from the accent, the radii and the `--brand-*` chrome tokens. Nothing
else changes.

---

## 10. Working rules

**Do**
- Reach for a semantic token before a primitive; a primitive before a literal.
- Set `data-area` on a section and let components adapt, rather than restyling them.
- Pair every component class with an `h-typo-*` class.
- Use `h-sN` spacer divs for vertical rhythm inside CMS modules.
- Keep imagery square-cornered and documentary — real fieldwork, never staged stock.
- Use `arrow-long-right` for forward affordances; keep CTAs short and active.

**Don't**
- Hard-code a hex, a font size, or a corner radius. All three are theme-dependent.
- Add drop shadows. Hierarchy is color and hairline rules.
- Use gradients as backgrounds, or emoji as icons.
- Assume the accent is dark — on `data-area="dark"` it resolves to a bright brand step.
- Bake typography into a component class.

---

## 11. Live reference

- `CGIAR Design System.dc.html` — the browsable specimen page: every ramp, the full type
  scale, spacing, radii, the icon sprite, all components and patterns, with a live
  brand-theme switcher.
- `CGIAR Sample Implementation.dc.html` — a full homepage assembled only from the classes
  and tokens above: sticky header, three-level mega-menu, hero, facts, issue selector,
  card grids, FAQ, footer.
