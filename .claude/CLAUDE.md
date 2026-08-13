# Project: SDB 2027 Demo (1.35°N)

> **WORK HERE** — Project context only. Keep under 25 lines.

## Overview
Static GitHub Pages site for the 1.35°N interactive mobile demo — Rally AI's
submission to the Singapore Design Biennale 2027 International Open Call.

## Project-Specific Notes
- Stack: no build, no deps — static HTML + Claude Design `dc` runtime + vendored React 18 UMD
- Live: https://letsrally-ai.github.io/rally-sdb-2027/ (Pages: branch `main`, path `/`)
- Local preview: `python3 -m http.server 5190`
- Deploy = push to `main`. No Actions (token lacks `workflow` scope)

## Source of Truth
- `index.html` is GENERATED from the **Claude Design** project "rally-sdb-2027"
  (`SDB 135N Demo.dc.html`), which lives in Claude Design — there is no local copy.
  Change the design there and re-export; never hand-edit the template or app logic here.
- Only 4 export lines are modified: head meta, vendored React, canvas-offset removal,
  mobile-fit CSS. Everything below the `.app-shell` div must stay byte-identical.
- `support.js` and `assets/demo/` are unmodified copies. Re-copy, never edit. Sole exception:
  `studio-photo.jpg`, re-encoded 1600px→800px (505KB→142KB). It is the only photo;
  everything else is pixel art and must NEVER be resampled.

## Re-export Checklist
Re-exporting overwrites this repo's copies, so after any re-export re-apply, in order:
1. The 4 head/CSS edits above (diff against the previous `index.html` to find them)
2. Re-optimise `studio-photo.jpg` — the export ships the 505KB original and will silently
   undo the 72% saving: `magick in.jpg -resize 800x -strip -interlace Plane -quality 85 out.jpg`
3. Re-verify: no `unpkg` in the served HTML, absolute `og:image`, no horizontal scroll <440px

## Gotchas
- Open Graph URLs must be ABSOLUTE — relative ones silently give an empty share card
- `.nojekyll` is load-bearing: `assets/demo/_pins*.png` start with `_`, Jekyll drops those
- Canvas is 410×880; <440px runs edge-to-edge, landscape <500px tall scrolls the page
- Asset paths are built dynamically (`'assets/demo/av-' + n + '.png'`) — never prune
  assets by static grep, it will miss files
