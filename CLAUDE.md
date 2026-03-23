# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Stack

Pure static HTML — no build system, no package manager. Dependencies loaded from CDN:
- **Three.js r128** — 3D WebGL rendering
- **Tailwind CSS** — utility styling (CDN)
- **Google Fonts** — Inter, JetBrains Mono

## Development

Open `index.html` directly in a browser, or run a local server:
```
python3 -m http.server
```

## Deployment

GitHub Pages — push to `main` on `aether-space.github.io`. No CI/CD pipeline.

## Architecture

Single-file app (`index.html`). All CSS lives in an inline `<style>` block; all JS lives in an inline `<script>` block at the bottom of `<body>`.

**Three.js scene globals:**
- `scene`, `camera`, `renderer` — standard Three.js setup
- `earth`, `moon`, `mars` — `THREE.Mesh` objects with texture maps loaded from mrdoob/three.js GitHub raw URLs
- `satSystem[]` — satellite shell groups, each with a `{ mesh, speed }` shape; rotation applied each frame in `animate()`
- `rocketSystem[]` — transit objects, each with `{ mesh, curve, t, speed }`; position updated along a `QuadraticBezierCurve3` each frame

**`animate()` loop** handles: planet rotation, satellite shell rotation, rocket traversal along curves, and mouse-driven camera parallax (`mouseX`/`mouseY` globals updated via `mousemove`).

Planet textures are fetched from external CDN URLs at runtime — they require an internet connection.
