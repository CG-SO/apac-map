# Review note: SFP "Where we work" tier map

Branch `feature/sfp-tier-map`, committed locally and not pushed.

## What was built

| File | Change |
|---|---|
| `tier-map.html` | New choropleth engine with no content in it. Reads `?data=` like `map.html`. Token layer, typography helpers and component classes are copied line for line from `map.html`, with tier-map additions below them. |
| `data/sfp.json` | SFP data: 3 tiers, 20 countries, client name overrides, empty `countryInfo`. |
| `README.md` | New section "Tier map (choropleth)": JSON format, behaviour, Drupal embed (`sfpTierMap`). |
| `.gitignore` | Adds `_screens/` and `_ref/`. |

`map.html` and `data/apac.json`, `americas.json`, `africa.json` are untouched (`git diff` is empty for them).

What differs from the brief's JSON proposal (all additive and optional):

- `tiers[].description` holds the doc's sub-labels ("Core / Strategic / Emerging priorities").
- `theme` sets optional hex overrides for three semantic accent tokens, so the panel can use the SFP sub-brand greens. The engine's defaults stay CGIAR.
- `ui.selectPrompt`, `ui.filterLabel`, `ui.allLabel` hold UI strings, with English defaults.
- `countryInfo[ISO3]` takes an array of strings or `{ "text", "url" }` objects. URLs go through `safeUrl()`.

## Colours

The brief asked for CGIAR brand-token placeholders. The requester then asked to use the
**SFP sub-brand palette from the email-signature screenshot** instead. Values were sampled
from the screenshot's pixels:

| Role | Hex | Source |
|---|---|---|
| Tier 1 (strongest) | `#03352a` | SFP logo block. Identical to CGIAR token `--color-brand-80` (`#033529`) to within 1 step. |
| Tier 2 | `#2c6a4f` | "SFP Communications Lead" green in the signature. Not a CGIAR ramp token. |
| Tier 3 (lightest) | `#8a8160` | Darkened from the signature's khaki `#a9a07d`. **Not the exact brand khaki**, see contrast below. |
| Panel accent (`theme.accent`) | `#2c6a4f` | Eyebrow text, links |
| Panel accent strong (`theme.accentStrong`) | `#03352a` | Pressed tier and "All tiers" rule |

The three tier hex values exist only in `data/sfp.json`. The engine has no hardcoded
colours outside the copied token layer.

### Contrast (WCAG 1.4.11, need ≥ 3:1)

Land is `--color-map-land` `#ffffff`, which is also the panel background. Water is `--color-map-water` `#e2eef1`.

| Fill | vs land / panel `#fff` | vs water `#e2eef1` | Result |
|---|---|---|---|
| Tier 1 `#03352a` | 13.58 | 11.47 | Pass |
| Tier 2 `#2c6a4f` | 6.40 | 5.40 | Pass |
| Tier 3 `#8a8160` | 3.90 | 3.29 | Pass |
| *Brand khaki `#a9a07d` (rejected)* | *2.62* | *2.21* | *Fail* |

Tier-to-tier contrast: Tier 1 vs 2 is 2.12, Tier 2 vs 3 is 1.64. WCAG does not require this,
because tier is never conveyed by colour alone (tooltip text, panel lists and the screen-reader
summary all name the tier). Lightness still runs dark, mid, light in tier order.

Faded (non-selected tier) countries drop to 20% fill opacity on purpose. Their tier is still in the tooltip and the panel.

## Reference files vs. this brief

- **`Africa Map _SFP WEbsite.pptx`**: fills all 20 countries in **one teal, `#329A89`**, with no per-tier colours. Headings use `#033529` / `#08614B`. The deck abbreviates names as "DRC" and "Vietnam". The map uses the client's full names from the brief and doc ("Democratic Republic of the Congo", "Viet Nam").
- **The deck's Morocco includes Western Sahara.** The Natural Earth GeoJSON used by all maps here keeps Western Sahara (`ESH`) separate, so the tier map fills Morocco only. See the open questions.
- **`Include in Website…docx`**: same 20 countries and tiers. It adds tier sub-labels (used as `description`) and longer intro copy. The panel keeps the brief's short subtitle because the doc's paragraph sits above the embed in Drupal. The coordinators list in the doc stays out of the map, per the brief.
- `SFP map.pptx` was not supplied, so it was not checked.

## How to test

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000/tier-map.html?data=data/sfp.json`.

## Tests run (Playwright via the Playwright MCP, Chromium)

All 40 assertions passed. Exactly as run:

- 20 country polygons drawn (`path[data-iso]`) at 1280px and at 390px. Panel counts 7 + 9 + 4 = 20.
- Default state: all 20 at full opacity, "All tiers" `aria-pressed="true"`.
- Each tier button at both widths: `aria-pressed` true on the clicked tier only, "All tiers" false. That tier's countries stay at `fill-opacity` 1 and every other priority country goes to 0.2.
- Re-clicking the active tier resets. The "All tiers" button resets.
- Hovering India gives the tooltip "India · Tier 2". Clicking it shows "India" / "Tier 2 · Strategic priorities" in the panel with `aria-pressed="true"`. Escape clears.
- Keyboard: focusing Kenya shows the tooltip "Kenya · Tier 1" and a focus outline. Enter selects it.
- No horizontal overflow at 390px.
- An `apac-map-height` message is posted.
- Console on `sfp.json` load: **0 errors, 0 warnings.**
- Validation, using an injected bad data file: an unknown ISO3 (`XXX`) and a duplicate (`KEN` in two tiers) each log one warning. A non-hex colour falls back to the accent token. A missing data file shows the error box.
- Regression: `map.html?data=data/americas.json` renders 3 pins with no new console errors. `git diff` on `map.html` and `data/*.json` (except `sfp.json`) is empty.

**Focus order** (Tab): map container, then the 20 countries in tier order (Tier 1 in data order, then Tier 2, then Tier 3), then the zoom +/− buttons, the attribution links, "All tiers", and Tier 1 → 3 buttons. This follows DOM order with the map first, the same as `map.html`.

Not done: no screen-reader run (VoiceOver/NVDA) and no Firefox or Safari run.

## Screenshots (`_screens/`, gitignored)

- `desktop-1280-all-tiers.png`, `desktop-1280-tier-1.png`, `-tier-2.png`, `-tier-3.png`
- `desktop-1280-country-selected.png`, `desktop-1280-keyboard-focus.png`
- `mobile-390-all-tiers.png`, `mobile-390-tier-1.png`, `-tier-2.png`, `-tier-3.png`
- `regression-map-americas-1280.png`

## Open questions (not decided here)

1. **Final tier colours.** Options: the SFP sub-brand palette used now, CGIAR ramp tokens (director's ask), or the client deck. The deck has a single teal, so it offers no tier colours to match.
2. **Tier 3 khaki.** The exact signature khaki `#a9a07d` fails 3:1. This build darkens it to `#8a8160`. The alternative is to keep the exact khaki and add a darker outline stroke for contrast.
3. **Boundary disclaimer wording.** `ui.disclaimer` is empty. Morocco/Western Sahara differs between the deck and the GeoJSON, and other disputed borders may not match the line CGIAR uses.
4. **Country click → coordinator info.** `countryInfo` is supported but empty, since coordinators live in Drupal for now.
5. **Panel header.** The brief puts the eyebrow, title and intro in the panel, and the Drupal page has its own "Where We Work" heading above the embed. Should the panel title be dropped to avoid saying it twice? Removing `header.title` from the JSON hides it.

## Security follow-up (automated review after the first commit)

- **Fixed: tooltip HTML injection.** Leaflet parses a string passed to `bindTooltip` as HTML. Country names and tier labels come from the JSON and the third-party GeoJSON, so the tooltip is now a `<span>` filled with `textContent`. Tested with a tier label of `<img src=x onerror=alert(1)>` and a name of `<b>Kenya</b>`: both show as plain text, no `<img>` is created and no dialog opens.
- **Fixed: `?data=` is now limited to same-origin `.json` files.** Anything else falls back to `data/sfp.json`. Tested with `?data=https://example.com/x.json`, which loads the SFP data instead.
- **Open (not changed): the GeoJSON loads from `johan/world.geo.json@master` on jsDelivr.** This is the same unpinned URL `map.html` uses. With the tooltip fix, a tampered file can no longer inject script, but it could still change the shapes. Pinning a commit hash or hosting the file in this repo would close that gap for both maps. That touches `map.html`, so it is left for a separate decision.
