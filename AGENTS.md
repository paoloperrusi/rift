# RIFT — Universal Project Guide & Environment Sync (`AGENTS.md`)

> **Multi-Purpose Environment Context**: This file is the single source of truth for **all environments** (Antigravity CLI, Antigravity Desktop 2.0, Antigravity IDE, external agents, and human contributors). Any session or agent opening this repository can read this document to instantly catch up on project architecture, feature capabilities, workflows, and recent changes.
>
> **Mandatory Rule for All Environments:** Whenever changes, fixes, or enhancements are made to the codebase, **document them in the [Project Changelog](#project-changelog--catch-up-log) section of this file**.

---

## 1. Project Overview & Architecture

**RIFT** is a high-performance, in-browser creative tool for generative glitch art, digital distortion, procedural image synthesis, and photographic destruction.

* **Live Demo:** [https://paoloperrusi.github.io/rift/](https://paoloperrusi.github.io/rift/)
* **Zero Dependencies:** Pure vanilla HTML5, CSS3, and JavaScript within a single standalone [index.html](file:///C:/proj/rift/index.html).
* **100% Client-Side:** All image processing, procedural generation, and script execution happen locally in the browser via native 2D Canvas contexts (`ImageData` pixel buffers). No backend or build step is required.
* **Local Run:** Simply open `index.html` in any modern web browser or serve via `npx serve .` / `python -m http.server`.

---

## 2. Architecture & Key Systems

The entire application state and rendering engine live in [index.html](file:///C:/proj/rift/index.html):

### Global State (`ST` & `ADJ`)
* `ST`: Houses active source images, working canvas buffers, zoom/pan transforms, active effect selections, parameter values (`activeParams`), sequence chain items, undo/redo stacks, and mask layers.
* `ADJ`: Holds tone, color, geometry, and lens adjustment slider states.

### Canvas Processing Pipeline
1. **Source & Buffer:** `ST.img` stores original loaded media. `ST.workingCanvas` holds committed edits.
2. **Preview & Commit Workflow:**
   * **Apply (`previewEff`):** Computes pixel manipulations non-destructively for visual feedback.
   * **Commit:** Bakes current effect output into `ST.workingCanvas` and pushes to the Undo stack (`ST.history`).
   * **Reset:** Discards uncommitted adjustments and restores canvas back to the last committed state.
3. **Split Comparison View:** Interactive dual-view rendering with a draggable vertical separator line, using canvas clipping (`ctx.clip()`) to compare original source against transformed output in real-time.
4. **Slider Tracking Subsystem:** `.slider-wrap` custom UI elements with hidden overlay range inputs and synced thumb discs / fill bars. Supports bidirectional fill for negative-to-positive ranges (e.g., center zero mark).

---

## 3. Feature Inventory & UI Guide

### Titlebar
* **Open / Export:** Load images, generate demo test patterns, and export PNG at 1x, 2x, or 4x resolution.
* **History:** Undo (`Ctrl+Z`) and Redo (`Ctrl+Y`) for committed transforms, crops, and tone adjustments.
* **View Modes:** Fit to screen (`Z`), 100% actual pixels (`1`), and zoom slider.
* **Apply / Commit / Reset:** Render preview, bake into image buffer, or revert.

### Left Sidebar
* **Live Search Filter:** Instant fuzzy search across 50+ effects and procedural generators.
* **Effect Categories:**
  * **Glitch:** Scanline Tear, Channel Shift, Block Shuffle, Pixel Scatter, JPEG Artifact, Data Reinterpret, Word Corrupt, Row Duplicate.
  * **Distort:** Wave Distortion, Twirl/Swirl, Polar Inversion, Pinch/Bulge, Ripple, Shear/Skew.
  * **Corrupt:** Bitplane Isolate, Bitwise XOR, Noise Injection, Echo/Ghost, Color Bleed.
  * **Mirror:** Mirror H, Mirror V, Kaleidoscope, Tile Mirror, Droste Spiral, Radial Tile.
  * **Pixel:** Luminance Sort, Hue Sort, Pixelate, Halftone, Bayer Dither.
  * **Color:** Hue Rotation, Palette Crush, False Color, Channel Swap, Invert, Threshold.
  * **Art:** Floyd-Steinberg Dither, Emboss, Oil Paint, Voronoi Shatter, Stipple, ASCII Art.
  * **Geometry:** Möbius Transform, Log-Polar Spiral, Affine Tile, Conformal zⁿ, Perspective Warp, Wallpaper Groups.
  * **3D:** Sphere Wrap, Bump Map, Parallax Depth, Glass Refraction, Depth of Field, Cube Face Map.
  * **Fractal:** Mandelbrot Lens, Julia Warp, Newton Warp, IFS Warp, Hyperbolic Tile, Reaction Diffusion.
  * **Generator:** Standalone procedural pattern generators (Perlin Noise, Voronoi Field, Gray-Scott, Plasma, Mandelbrot, Julia, Newton, L-System, Flow Field, Truchet, Wave Interference, Spirograph, Lissajous, Attractor).
* **Presets:** One-click aesthetic recipes (Vaporwave, Deep Glitch, CRT Damage, Crystallize, Fractal Dream) and custom user preset saving.

### Center Viewport
* **View Modes:** Original, Result, and Split comparison slider.
* **Interactive XY Mapper:** Click the `X` or `Y` icon beside any parameter slider to map canvas drag coordinates directly to that parameter in real-time.
* **Crop Tool:** Drag crop box with aspect ratio constraints (1:1, 4:3, 16:9, 3:2, A4) or freeform.

### Sequence Chain (Bottom Strip)
* Chain multiple effects sequentially to create complex generative pipelines.
* Controls: Add effect (`+`), Run chain (`▶ run`), and cycle live animation loop (`↻ cycle`).

### Right Panel Tabs
* **Params:** Live sliders with direct numeric click-to-edit, randomize, and reset buttons.
* **Adjust:** Color & Tone (Brightness, Contrast, Saturation, Gamma, Exposure, Temp, Tint), Geometry (Rotate, Flip), and Lens & Detail (Vignette, Grain, Chromatic Aberration, Sharpness).
* **Mask:** Selective effect application. Brush paint/erase, automatic luminance thresholds (Shadows, Midtones, Highlights), Sobel edge detection, and mask inversion.
* **Script:** Built-in JavaScript pixel shader editor for custom algorithms with live parameter definitions (`param()`), helper functions (`ctx.sample()`, `ctx.noise()`, `ctx.luma()`), and `.rift` file import/export.

---

## 4. Keyboard Shortcuts

| Key | Action |
| :--- | :--- |
| `Space` + Drag | Pan canvas viewport |
| `Mouse Wheel` | Zoom in / out centered at cursor |
| `Z` | Zoom to fit viewport |
| `1` | Zoom 100% (1:1 actual pixels) |
| `Ctrl+Z` / `Cmd+Z` | Undo last commit / crop / adjustment |
| `Ctrl+Y` / `Cmd+Y` | Redo |
| `O` | Open image file dialog |
| `S` | Export PNG dialog |
| `Enter` (in Crop) | Commit crop |
| `Escape` | Cancel crop / dismiss active modal |

---

## 5. Development & Environment Guidelines

1. **Maintain Single-File Integrity:** Keep all HTML, styles, and logic centralized in `index.html` unless architectural changes require modularization.
2. **Zero External Runtime Dependencies:** Do not introduce npm packages, runtime CDNs, or bundlers without explicit instruction. Everything should remain runnable by simply double-clicking `index.html`.
3. **Canvas Performance:**
   * Work with 1D `Uint32Array` or `Uint8ClampedArray` directly when iterating over pixels.
   * Avoid memory allocation inside hot pixel loops; reuse buffers where possible.
   * Clamp coordinates and verify array bounds to prevent out-of-bounds exceptions.
4. **CSS Token Usage:** Use the defined CSS variables (`--bg1` through `--bg4`, `--acc`, `--acc2`, `--t1` through `--t3`, `--ln`) for styling new controls.

---

## 6. Project Changelog & Catch-Up Log

> **Environment Note:** When making changes in any environment, add a new entry to the top of this list describing the changes, affected areas, and any follow-up notes.

### [2026-09-07] — Multi-Environment Sync Transition
* **Author / Environment:** Antigravity Agent (`agy` CLI)
* **Changes:**
  * Renamed and repurposed `GUIDE.txt` to `AGENTS.md`.
  * Established universal multi-environment sync specification across Antigravity CLI, Antigravity Desktop 2.0, IDE, and contributor tools.
  * Formatted documentation with GitHub Flavored Markdown and complete architectural index.

### [2026-09-07] — Slider Hit-Box & Bidirectional Tracking Overhaul (Commit `d78ba19`)
* **Author / Environment:** Paolo Perrusi
* **Changes:**
  * Replaced default range slider rendering with `.slider-wrap` container.
  * Fixed thumb-disc hit-box alignment and visual tracking across parameter and adjustment sliders.
  * Added bidirectional zero-mark tracking for negative-to-positive ranges (red fill for negative values, cyan fill for positive values).

### [2026-09-07] — Interactive Features & History Expansion (Commit `cd10f0f`)
* **Author / Environment:** Paolo Perrusi
* **Changes:**
  * Added interactive before/after split screen slider mode on the canvas.
  * Implemented live mask blending and luminance isolation (Highlights/Midtones/Shadows).
  * Added Undo/Redo history stack for non-destructive workflow.
  * Introduced fuzzy sidebar search filter across 50+ effects and generators.
  * Added synthetic procedural test pattern generator for instant demoing without loading an image.
  * Created initial user guide.

### [2026-09-07] — Initial Release (Commit `35b2151`)
* **Author / Environment:** Paolo Perrusi
* **Changes:**
  * Initial commit of the RIFT Image Fracture Tool.
  * 50+ generative glitch, distortion, fractal, and color effects.
  * Sequencer chain, custom JS script shader editor, and canvas export.
