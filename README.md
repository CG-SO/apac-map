# Reusable interactive map

One map engine (`map.html`), many datasets. To publish a new map you write **one JSON file** —
no database, no build step, no code changes.

**Live base URL:** `https://cg-so.github.io/apac-map/`

---

## How to make a new map (3 steps)

**1. Copy a data file**

Duplicate `data/apac.json` → `data/myregion.json` and edit the text, coordinates and links.

**2. Commit and push**

```bash
git add data/myregion.json
git commit -m "Add myregion map data"
git push
```

Wait ~1 minute for GitHub Pages to publish. *(Push one commit at a time — several
back-to-back pushes queue competing deploys and fail with "try again later".)*

**3. Embed it**

Point the iframe at your new file with `?data=`:

```html
<iframe id="apacMap"
        src="https://cg-so.github.io/apac-map/map.html?data=data/myregion.json"
        title="Interactive map" loading="lazy" scrolling="no"
        style="width:100%; min-height:520px; border:0; border-radius:.8rem; display:block;">
</iframe>
<script>
  window.addEventListener('message', function (e) {
    if (e.data && e.data.type === 'apac-map-height' && typeof e.data.height === 'number') {
      var f = document.getElementById('apacMap');
      if (f) f.style.height = e.data.height + 'px';
    }
  });
</script>
```

The `<script>` makes the iframe auto-fit its content height. If your CMS strips inline
scripts, drop it and set a fixed `height:640px` instead — the map is still width-responsive.

---

## Data file format

Only `pins` is required. Everything else has a sensible default.

```jsonc
{
  "header": {                        // optional — omit the whole block to hide the title area
    "eyebrow":  "CGIAR · Africa",    // small uppercase line above the title
    "title":    "CGIAR in Africa",   // also becomes the browser tab title
    "subtitle": "Short intro line."
  },

  "map": {
    "regionLabel": "Africa",         // white pill shown on the map (omit to hide)
    "center": [2, 20],               // [latitude, longitude]
    "zoom": 3,
    "fitToPins": true                // true = auto-zoom to fit all pins (default)
  },

  "ui": {
    "hint": "Hover or click a pin…", // small grey line under the buttons
    "defaultPinStyle": "numbers",    // or "logos" to start with badges showing
    "showPinStyleToggle": true       // false hides the 123 / Logos switch
  },

  "theme": {                         // optional colour overrides
    "brand":    "#0a6b3b",           // pins, chips, secondary button
    "cta-bg":   "#123528",           // primary button background
    "cta-text": "#6ee7b7",           // primary button text
    "ocean":    "#a8d3dd",
    "land":     "#f2efe6"
  },

  "pins": [                          // REQUIRED — 3–5 works best
    {
      "coords": [-1.29, 36.82],      // REQUIRED — [latitude, longitude]
      "badge":  "MAIZE",             // short text on the pin in "Logos" mode
      "logo":   "https://…/logo.png",// image instead of badge text (optional)
      "kicker": "Regional initiative",
      "title":  "Drought-tolerant maize",
      "desc":   "Max ~150 characters keeps the panel tidy.",
      "chip":   "Program",           // small green tag
      "loc":    "Kenya · East Africa",
      "primary":   { "label": "View related Program", "url": "https://…" },
      "secondary": { "label": "Explore the region",   "url": "https://…" }  // omit to show one button
    }
  ]
}
```

### Notes

- **Text is plain text.** Write `Crop & livestock` — no HTML, no `&amp;`.
- **Coordinates** are `[latitude, longitude]`. Get them by right-clicking a spot in
  Google Maps and copying the two numbers.
- **Links** must be `http://` or `https://`. Other schemes are ignored.
- **Buttons** appear only when both `label` and `url` are set, so a pin can have two,
  one, or none.
- **Pin toggle** (123 / Logos) appears automatically when at least one pin has a
  `badge` or `logo`.

---

## Examples

| Map | URL |
|---|---|
| APAC | `map.html?data=data/apac.json` |
| Africa (demo) | `map.html?data=data/example-africa.json` |

---

## Files

| File | Purpose |
|---|---|
| `map.html` | The map engine. Generic — contains no content. |
| `data/*.json` | One file per map. **This is what you edit.** |
| `index.html` | Original standalone demo (map + portfolio tabs). |
| `map-only.html` | Original single-purpose APAC map, kept for existing embeds. |

Testing locally needs a small web server (browsers block `fetch` on `file://`):

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000/map.html?data=data/apac.json`.
