# Anvil & Altar

Daily devotionals for men who want a faith that works on Monday morning.

A single-file web app — no build step, no dependencies, no server. Open `index.html` and it runs.

## Current library

91 days of devotionals:

| Range | Days | Calendar |
|---|---|---|
| January 1–5 | 5 | Day 1 – Day 5 |
| September 6–30 | 25 | Day 249 – Day 273 |
| October 1–31 | 31 | Day 274 – Day 304 |
| November 1–30 | 30 | Day 305 – Day 334 |

Each day contains a Bible passage (NIV primary, The Message secondary), a devotional body ("The Grind"), a practical action step, and a closing prayer. All scripture quotations have been verified against BibleGateway.

## Features

- Daily devotional with five content sections
- Prev / Next navigation, Today button, and a 12-month jump calendar
- Full-text search across every devotional
- Star / bookmark days, with a starred panel
- "Mark it done" tracking per day
- Private reflection notes with live word count
- Light / dark / system theme, remembered per device
- Progress bar against the 365-day year
- Keyboard navigation (arrow keys, Escape)
- Share, with a copy-to-clipboard fallback
- Respects `prefers-reduced-motion`

All personal data — reflections, stars, completion, theme — is stored in the browser's `localStorage`. It never leaves the device and is never shared between visitors.

## Hosting it on GitHub Pages

1. Create a new repository on GitHub.
2. Upload `index.html` and this `README.md` to it.
3. Go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, pick `main` and `/ (root)`, then Save.
5. Wait about a minute. The site appears at `https://<your-username>.github.io/<repo-name>/`.

Because the file is named `index.html`, GitHub Pages serves it automatically at the site root.

## Adding more months

Devotionals live in the `DEVOTIONALS` array in the second `<script>` block. Each entry is:

```js
{
  title: "",
  ref: "",       // e.g. "Proverbs 27:17"
  niv: "",
  msg: "",
  grind: ["", "", ""],   // 3–4 paragraphs
  action: "",
  prayer: ""
}
```

Dates are **not** stored on the objects. They are computed from `DAY_MAP`, which maps each array index to a day of the year (0 = January 1). To add a month, append the devotional objects to `DEVOTIONALS` **and** append the matching calendar days to `DAY_MAP`. The two arrays must stay the same length and in the same order.

Day-of-year offsets (non-leap): Jan 0, Feb 31, Mar 59, Apr 90, May 120, Jun 151, Jul 181, Aug 212, Sep 243, Oct 273, Nov 304, Dec 334.

## Editing safely

The whole app is one HTML file with two `<script>` blocks — the devotional data, then the app logic. `render()` is called at the very end of the main IIFE, so **anything that throws before that point leaves the page blank**. When editing, keep new logic after `render()` and wrapped in `try/catch`.

Avoid these APIs — they are blocked in some sandboxed hosting contexts and will kill the script: `canvas.toDataURL()`, `URL.createObjectURL()`, `new Blob()`, and service worker registration. On a real domain like GitHub Pages these work normally.
