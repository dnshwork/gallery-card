# Patch notes (fork v1.2.1)

Base: lukelalo/gallery-card v1.2.0 (archived upstream).

## Problem
The Zoom link (`<a target="_blank">`) stopped working in the Home Assistant
Android companion app (2026.7.x): WebView swallows new-window requests,
so tapping Zoom opened a blank/black page. Desktop and mobile browsers
were unaffected.

## Fix
- Zoom opens an in-page fullscreen overlay on `document.body` (no navigation,
  no new windows), so it works in the companion app and in any browser.
- Image viewer: pinch-to-zoom up to 6x, one-finger pan when zoomed,
  double-tap to toggle zoom, + / - buttons, mouse wheel + drag on desktop.
- Videos open in the same overlay with native controls.
- Zoom button moved from the caption to the card header (top-left,
  symmetric to Reload) as `ha-progress-button.btn-zoom`.
- Overlay uses native `ha-progress-button` for + / - / Close.

## Files
- `gallery-card.js` attached to the v1.2.1 release is the patched build
  (based on the v1.2.0 asset). HACS auto-generates the `.gz` variant
  on download; do not commit it.

## v1.2.2 — embedded placeholder
- The card used to reference `/local/community/gallery-card/placeholder.jpg`
  for lazy thumbnails, but HACS dashboard installs only ship the `.js` file,
  so the placeholder was missing (404) and absent snapshots rendered as
  broken-image icons (no `onerror` handling anywhere).
- The placeholder is now embedded in `gallery-card.js` as a data-URI
  (`window.__gph`, ~5 KB PNG) and wired everywhere: lazy thumbnails initial
  `src`, `@error` fallback on thumbnails / main viewer, `poster` on menu +
  main videos, popup and fullscreen viewer fallback.
- External `placeholder.jpg` is no longer required and can be removed
  from `www/community/gallery-card/`.
