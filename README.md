# Shot Logger — standalone golf tracking PWA

3-tap shot logging (yardage · club · lie · result) with automatic GPS shot marking,
green geotagging, club gapping, lie splits, driver slice-watch, strokes-gained-lite,
and CSV export. Built for use alongside the Golf Canada app at Sunningdale G&CC.

No accounts, no server, no subscription. All data lives in your phone's browser
storage (localStorage) and exports to CSV anytime.

---

## Deploy to GitHub Pages (one-time, ~5 minutes)

### Option A — Git Bash (your normal workflow)

```bash
cd golf-shot-logger
git init
git add .
git commit -m "Shot Logger v1"
# create an empty repo named shot-logger on github.com first, then:
git remote add origin https://github.com/YOUR-USERNAME/shot-logger.git
git branch -M main
git push -u origin main
```

Then on github.com: repo → **Settings → Pages** → Source: *Deploy from a branch* →
Branch: **main**, folder **/ (root)** → Save.

After ~1 minute your app is live at:
`https://YOUR-USERNAME.github.io/shot-logger/`

### Option B — no terminal

github.com → New repository → name it `shot-logger` → create.
"uploading an existing file" link → drag all 5 files in → Commit.
Then Settings → Pages as above.

> GitHub Pages serves over HTTPS, which is required for GPS to work. Any static
> host works (Cloudflare Pages, Netlify) — Pages is just the path of least
> resistance from Git Bash.

---

## Install on your iPhone

1. Open the URL in **Safari** (must be Safari for Add to Home Screen on iOS).
2. Share button → **Add to Home Screen** → Add.
3. Open it from the home screen icon — it runs full-screen like a native app.
4. First time you log a shot or tap ⟳, iOS asks for location permission →
   **Allow While Using App**.

Works offline after first load (service worker caches the app shell). Your data
never leaves the phone.

---

## First-round setup (~10 min total, once)

1. **COURSES tab** — fix the pars. East holes 2–12 are preloaded; tap any hole's
   par to cycle 3→4→5. Fill in hole 1 and 13–18 for East, and all of West from
   the scorecard. Verify the East/West assignment is right and rename if needed.
2. **Tag greens as you play** — while standing on each green, tap **⛳ SET GREEN**
   once. 18 taps per course and every future round shows live distance-to-green.
   The Courses tab shows tagging progress (⛳ count) per course.

## Per-shot workflow (on the course)

1. Rangefinder the shot, punch the number on the keypad.
2. Tap club. (Lie auto-defaults: Tee on shot 1, Fairway after — only tap it when
   you're in trouble.)
3. Tap the result — **that saves the shot**. UNDO appears for 3 seconds.
4. GPS position attaches automatically in the background (only kept if accuracy
   is better than 40 m; never blocks logging).
5. Optional: tap PUTTS count after holing out — unlocks putting stats and
   completes the strokes-gained chain.

## What the stats give you

- **CLUB BOOK** — AVG (attempt distance), PURE (rangefinder distance on
  on-target shots = true gapping), GPS (measured carry from shot-to-shot
  coordinates), on-target %, top miss per club.
- **BY LIE** — execution rate from tee/fairway/rough/sand/trees; quantifies what
  missing fairways costs on the next shot.
- **DRIVER · SLICE WATCH** — miss-direction distribution for tee balls.
- **SG-LITE** — strokes gained vs a scratch baseline (Broadie-style tables,
  linear interpolation), split Tee / Approach / Short / Sand / Putting.
  End-states on hole-out shots are modelled, so treat category *relatives* as
  the signal, not the absolute values.
- **Export CSV** — full shot-level dump (coords, carries, SG per shot) for
  Python/Excel analysis.

## Files

| file | purpose |
|---|---|
| `index.html` | the entire app (vanilla JS, no build step) |
| `manifest.webmanifest` | PWA install metadata |
| `sw.js` | offline caching |
| `icon-192.png` / `icon-512.png` | home-screen icons |

## Tweaks you'll probably want eventually

All in `index.html`, clearly marked constants at the top of the script:
`DEFAULT_CLUBS` (bag changes), `RESULTS` / `LIES` (taxonomy), `EAST_PARS`, and
the `BASE` strokes-gained tables. Edit, commit, push — the service worker picks
up new versions on next online load (bump `CACHE` version in `sw.js` when you
change files).
