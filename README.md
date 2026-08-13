# 1.35°N — Singapore Design Biennale 2027

Interactive mobile demo submitted for the **Singapore Design Biennale 2027 International Open Call**.

**Live demo → https://letsrally-ai.github.io/rally-sdb-2027/**

Best viewed on a phone, in portrait.

---

## What this is

A playable preview of *1.35°N* — the Biennale imagined as a living map of Singapore. Visitors
pick a persona, explore districts, and unlock stories, quests and rewards as they go.

The demo walks through the full journey:

1. **Campaign card** — how the Biennale surfaces inside the Rally app
2. **Intro** — Soot, the guide, sets up the premise
3. **Choose your character** — Designer · Business Professional · Everyday Innovator · Culture Seeker
4. **The island** — a pannable pixel map; programme pins light up for your persona and dim for others
5. **Quests** — visit, do, and collect across the programme

Everything is a self-contained front-end prototype. There is no backend, no account, and no data
is collected from anyone who opens it.

## Running it locally

No build step and no dependencies — it is static files.

```bash
python3 -m http.server 5190
# then open http://localhost:5190/
```

## Structure

```
index.html      the app — template + logic, authored in Claude Design
support.js      Claude Design (dc) runtime; renders <x-dc>/<sc-if>/<sc-for>
vendor/         React 18.3.1 + ReactDOM UMD builds, vendored locally
assets/demo/    pixel art, sprites and photography
.nojekyll       serve _-prefixed paths verbatim on GitHub Pages
```

## Provenance

`index.html` is generated from **Claude Design** (source project: `rally-sdb-2027`,
file `SDB 135N Demo.dc.html`). The design is the source of truth — regenerate rather than
hand-edit the template or the app logic.

Only four lines of the export were touched, all of them for web publication. Everything else —
522 template lines and 487 lines of app logic — is byte-identical:

| Change | Why |
| --- | --- |
| `<head>` metadata | Page title, description, favicon, theme colour, Open Graph tags, `viewport-fit=cover`. Open Graph URLs are absolute — relative ones produce an empty share card. |
| React loaded from `vendor/` | The runtime otherwise pulls React from unpkg at load. A CDN outage would leave a blank page on a submission URL. `loadReactUmd()` short-circuits when `window.React` already exists, so this needed no change to `support.js`. Both files were checked against the SRI hashes pinned in `support.js`. |
| Removed `position:absolute; left:12px; top:-3px` | Claude Design canvas framing. On a phone it pushed the app 12px off-screen, clipping the right edge (the SKIP button) and forcing a horizontal scrollbar. |
| Mobile-fit CSS block | Below 440px the phone-frame chrome (rounded corners, drop shadow) is dropped and the app runs edge-to-edge at `100dvh`. Above 440px the framed phone stays centred. Landscape phones get the full 880px canvas with page scroll instead of clipped CTAs. |

`support.js` and 45 of the 46 assets are unmodified copies.

The one exception is `assets/demo/studio-photo.jpg`, the only photograph here and by far the
largest file. It shipped at 1600×1068 / 505KB but is displayed in a modal capped at 330px wide,
so it was ~5× oversized even for a 3× screen. Re-encoded to 800×534 at q85 — **142KB, 72%
smaller** — which is still 2.4× the display width. The same file was written back to the Claude
Design source, so a re-export keeps it rather than reverting to the 505KB original.

This is safe here only because it is a photograph. The rest of `assets/demo/` is pixel art and
must never be resampled — rescaling it would destroy the hard pixel edges the whole look depends on.

## Verified on

Chromium at 360×640, 390×844, 430×932, 844×390 (landscape) and 1440×900 — full walkthrough
(campaign card → intro → character select → island map), zero console errors.

Portrait is the intended orientation. Landscape phones are shorter than the 880px design canvas,
so the page scrolls to reach the lower half rather than clipping it.

## Credits

Built by [Rally AI](https://letsrally.ai). Pixel art and campaign design in-house.
