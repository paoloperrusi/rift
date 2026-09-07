# RIFT — Comprehensive Test Suite & QA Protocol

This protocol covers end-to-end verification of all tools, rendering pipelines, parameters, controls, overlays, and export routines in RIFT.

---

## 📋 Test Directory Overview

- **Batch 1: Canvas, Loading, Navigation & Display Modes**
  - Image loading (demo image, file picker, drag & drop)
  - Zoom & Pan (mouse wheel, spacebar drag, middle click, zoom buttons `Fit`, `100%`, `200%`, `400%`)
  - View Comparison (Original vs Result vs Interactive Split Divider)

- **Batch 2: Toolbar & Geometry Operations**
  - Pan tool (`H`) and cursor states
  - Crop tool (`C`) with ratio locks (Free, 1:1, 4:3, 16:9), bounding box dragging, Enter/Apply, Esc/Cancel
  - Rotate & Flip buttons in Adjust pane, Canvas resizing

- **Batch 3: Sidebar Controls & Parameter Mechanics**
  - Category tabs & Search filter bar
  - Sliders: ball drag, track hit detection, dynamic colored fill bar, double-click/direct input, individual parameter reset (`↺`)
  - Randomize button (`🎲`)
  - Quick preset chips (preset application, active chip highlight, valid parameter limits)

- **Batch 4: Interactive XY Parameter Mapper**
  - Mapping X / Y axes to effect parameters via `X` / `Y` badges
  - Canvas drag manipulation in Select mode (`V`)
  - Crosshair overlay visibility: appears strictly during drag, clean disappearance upon mouse release (even when releasing off-canvas)
  - Numeric badge tracking

- **Batch 5: Effect Engines Verification**
  - Glitch & Displacement (Scanline Tear + bias slider, Pixel Sort, Chromatic Aberration, RGB Shift, Slice Jitter, Databend)
  - Color & Tone (Channel Mixer, Posterize, Solarize, Duotone, Threshold)
  - Texture & Degradation (Dither, Halftone, CRT Scanlines, Film Grain, Vignette)

- **Batch 6: Standalone Generators**
  - Generating from scratch without source image (Test Card, Cyber Grid, Plasma, Glitch Noise, Cellular)
  - Parameter tweaking on generated patterns
  - Applying effects on top of generated canvases

- **Batch 7: Masking, Blending & Adjustments**
  - Global Adjustments (Brightness, Contrast, Exposure, Warmth, Tint, Shadows, Highlights, Sharpen)
  - Masking Engine: Luminance, Gradient, Radial, and Noise masks; Invert mask
  - Blend Modes: Screen, Multiply, Overlay, Difference, Color Dodge; Opacity slider

- **Batch 8: Sequence Chaining, Script Editor, Undo/Redo & Export**
  - Sequence pipeline (`+` add to seq, reordering, `▶ run`, `↻ cycle`, `✕ clear`)
  - History system: Undo (`Ctrl+Z`), Redo (`Ctrl+Y`), visual toasts
  - Script Editor (`</>`): inspect active effect code, test execution
  - Export modal: format choice (PNG, JPEG, WebP), scale multiplier (1x, 2x, 4x), copy to clipboard (`Ctrl+S`)

---

## Batch 1: Canvas, Loading, Navigation & Display Modes
1. **Load Demo Image**: Click `✦ load demo image` in canvas center. Verify sample pattern loads crisp and centered.
2. **File Picker & Drag-and-Drop**: Drop any image onto the canvas or press `O` to open file dialog. Verify immediate image load without page reload.
3. **Zoom & Pan**:
   - Scroll mouse wheel over image: zoom in/out smoothly toward cursor.
   - Click `Fit` (fits in viewport), `100%`, `200%`.
   - Hold `Spacebar` and drag: canvas pans smoothly without selecting or dragging HTML elements. Release spacebar: pan stops.
4. **Split Comparison**:
   - Apply any effect (e.g. Scanline Tear).
   - Click `split` in the bottom-center view switcher.
   - Verify the purple vertical divider line appears.
   - Drag the divider handle `↔` left and right: verify left side shows original, right side shows effect result smoothly in real time.
   - Click `original` and `result` to verify toggle behavior.

---

## Batch 2: Toolbar & Geometry Operations
1. **Select Tool (`V`)**: Press `V` or click `↖`. Cursor should be normal crosshair/pointer, ready for XY mapping.
2. **Pan Tool (`H`)**: Press `H` or click `✋`. Drag anywhere on canvas: image pans with `grabbing` hand cursor.
3. **Crop Tool (`C`)**:
   - Press `C` or click `⬚`.
   - Drag a rectangle on the image. Dimmed overlay masks outer region.
   - Test Ratio dropdown: switch between `free`, `1:1`, `16:9`. Box should constrain properly.
   - Press `Esc` or click Cancel: crop rect disappears, canvas untouched.
   - Drag a new box and press `Enter` (or click Commit): canvas crops to exact bounds.
   - Press `Ctrl+Z`: crop is undone back to previous full image.

---

## Batch 3: Sidebar Controls & Parameter Mechanics
1. **Category Tabs & Search**:
   - Type in the search box (e.g., `rgb`, `tear`, `noise`): verify filtered list updates instantly. Clear search.
   - Click category tags (`glitch`, `color`, `distort`, etc.): verify list filters by category.
2. **Slider Mechanics**:
   - Select `scanline_tear`.
   - Click directly on the slider track: ball jumps to click position immediately.
   - Drag slider ball: ball moves smoothly and the colored fill bar precisely matches ball center.
   - Click `↺` reset icon next to a slider: resets to default value.
3. **Randomize Button (`🎲`)**:
   - Click `🎲` random button in effect header: all sliders randomize within valid ranges and image preview updates immediately.
4. **Quick Preset Chips**:
   - Click any quick preset chip (e.g., `subtle`, `heavy`, `analog`):
   - Values should change, active chip receives purple glowing border, and canvas preview reflects the preset.

---

## Batch 4: Interactive XY Parameter Mapper
1. **Axis Assignment**:
   - On `scanline_tear`, click the small `X` icon on `Tear Amplitude` (turns purple).
   - Click the small `Y` icon on `Tear Frequency` (turns amber).
2. **Canvas Dragging**:
   - Switch to Select tool (`V`).
   - Click and hold mouse down on canvas, then drag:
     - Crosshair lines appear at cursor position.
     - Tooltip badge shows live `X→amplitude` and `Y→frequency` values.
     - Sliders in sidebar update in real time.
     - Canvas glitch effect updates live.
3. **Mouse Release**:
   - Release mouse: crosshairs and value badges disappear immediately.
   - Drag cursor outside canvas window and release: crosshairs still vanish cleanly, no ghost dragging or stuck state.
4. **Toggle Off**:
   - Click `X` and `Y` icons again in sidebar to turn them off.

---

## Batch 5: Effect Engines Verification
1. **Scanline Tear**:
   - Adjust `Tear Bias` slider from -1.0 to +1.0: verify directional bias shifts scanline displacement (left vs right).
2. **Pixel Sort**:
   - Adjust threshold and sort direction: verify sorted pixel streaks appear along luminance/contrast boundaries.
3. **Chromatic Aberration & RGB Shift**:
   - Increase offset/distance: verify separate red/cyan/blue channel fringing.
4. **Duotone / Solarize / Invert**:
   - Check color effects apply properly with correct palette and threshold curves.

---

## Batch 6: Standalone Generators
1. **Open Without Image**: Refresh page or press clear.
2. **Test Card (`test_card`)**: Click `test_card` in Generators: SMPTE color bars and resolution grid appear immediately.
3. **Cyber Grid (`cyber_grid`)**: Tweak grid size, perspective, glow: smooth procedural grid rendering.
4. **Plasma (`plasma`)**: Animate or tweak frequencies: smooth mathematical color waves.
5. **Chain Effect onto Generator**:
   - With a generator loaded, switch to an effect (e.g. `dither` or `scanline_tear`): verify effect applies directly on procedural output.

---

## Batch 7: Masking, Blending & Adjustments
1. **Global Adjustments**:
   - Switch to `Adjust` tab in sidebar.
   - Adjust Brightness, Contrast, Saturation, Warmth: canvas updates in real time.
   - Click `Reset Adjustments`: values return to defaults.
2. **Masking Engine**:
   - Switch to `Mask` tab. Check `Enable Mask`.
   - Select `Luminance` mask: effects only apply to bright/dark areas.
   - Check `Invert Mask`: effect targets opposite luminance range.
   - Select `Radial` or `Gradient` mask: verify spatial falloff.
3. **Blend Modes**:
   - Set blend mode to `Screen`, `Multiply`, `Difference`: verify blend math.
   - Adjust `Mask Opacity`: transitions smoothly between unaffected and fully effected pixels.

---

## Batch 8: Sequence Chaining, Script Editor, Undo/Redo & Export
1. **Sequence Pipeline**:
   - Pick an effect and click `+` in bottom sequence bar.
   - Pick another effect and click `+`.
   - Click `▶ run`: sequence processes sequentially.
   - Click `↻ cycle`: auto-cycles periodically. Click again to stop.
   - Click `✕ clear`: clears sequence queue.
2. **History (Undo / Redo)**:
   - Click `Apply` on an effect.
   - Press `Ctrl+Z` (Undo): reverts to previous state.
   - Press `Ctrl+Y` (Redo): re-applies effect.
3. **Script Editor (`</>`)**:
   - Click `</>` in toolbar: modal opens showing GLSL-like / JS pixel shader code for current effect.
   - Change a coefficient and click `Deploy / Test`: canvas updates with custom code.
4. **Export (`Ctrl+S`)**:
   - Click `Export` button or press `Ctrl+S`.
   - Test `PNG`, `JPEG`, `WebP` format switches.
   - Test `1x`, `2x` resolution multiplier.
   - Click `Download Image`: browser downloads file with proper extension.
   - Click `Copy to Clipboard`: toast confirms image copied to clipboard.
