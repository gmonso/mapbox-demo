# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file Mapbox GL JS demo (`index.html`) — no build step, no npm, no bundler. Open the file directly in a browser or serve it with any static server.

```
# Servir localment (qualsevol d'aquests)
npx serve .
python -m http.server 8080
```

## Architecture

Everything lives in `index.html`:

- **Mapbox GL JS v3.6.0** via CDN (no instal·lació)
- **Markers**: two custom `<div>` elements (origin → green, destination → red) + a hidden moving vehicle marker
- **Route**: fetched from the Mapbox Directions API (`/directions/v5/mapbox/driving`) and drawn as a `line` layer
- **Animation**: `requestAnimationFrame` loop that interpolates the vehicle marker along `routeCoords[]` over `ANIM_MS` milliseconds
- **Popup**: Mapbox `mapboxgl.Popup` with custom HTML card injected via `setHTML()`. The card header image uses the Mapbox Static Images API.

## Key config

| Variable | Location | Purpose |
|---|---|---|
| `mapboxgl.accessToken` | top of `<script>` | Mapbox public token — replace with a project-specific token |
| `ORIGIN` / `DESTINATION` | top of `<script>` | `[lng, lat]` coordinates |
| `ANIM_MS` | top of animation section | Duration of the simulated trip in ms |

## Customising the location card

The `buildCard()` function returns raw HTML injected into the popup. To use content from your own web:
- Replace the `<img>` `src` with any URL (your CMS image, a CDN asset, etc.)
- Replace the static text fields with data fetched from your API before calling `showPopup()`
- The popup accepts arbitrary HTML/CSS — styles are scoped inside `.location-card`
