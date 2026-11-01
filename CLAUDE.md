# Glowbug Bench: working notes for Claude

The owner is Larry Higley (GitHub: INVENT-HORIZONS). The app started as the schematic tool for *The Glow Report*, his weekly vacuum-tube newsletter, and now stands on its own. The user-facing feature list is in `README.md`; this file holds the conventions and decisions behind it.

## Delivering an update (every change, every time)

1. Edit `glowbug-bench.html`. It is a single self-contained file with no build step.
2. Check it before shipping:
   - `node --check` on the extracted script.
   - Load it in Playwright with `#fresh` and confirm there are no page errors.
   - For new or changed symbols, render them large and look at them.
3. Artifact page: TBD. Do NOT publish to the Glow Schematic artifact (https://claude.ai/artifact/TXfk5HSDnztvJHS7N3ty3j); this project is a separate copy. When it gets its own page, keep the declared capabilities `db`, `downloads` and `user`. Saved projects live in the artifact database at `data/users/<uid>/<projectId>`.
4. The working copy lives on Larry's computer at `C:\Users\Owner\Documents\CLAUDE\glowbug bench\glowbug-bench.html`. Do not overwrite the Glow Schematic copy in the newsletter folder.
5. Send him the file as a download, because he uses it at work too.
6. Commit and push to `main` on https://github.com/INVENT-HORIZONS/glowbug-bench, authored as `Larry Higley <lahigley1965@gmail.com>`.
   - NEVER push to https://github.com/INVENT-HORIZONS/glowSchematic; this project was copied from it.
7. Update `README.md` when features or the part count change.

## Drawing rules

- **Colors:**
  - Paper `#FBF8F3`, ink `#17120E`.
  - Heaters and filaments are orange `#E8912D`. Nothing else uses it.
  - Phosphor green `#2F8A45` is used only for magic-eye targets (CSS class `tgt`).
- **Wires** are black by default. The palette is:
  - black `#17120E` for signal
  - red `#A3241B` for power
  - green `#2E6B34` for ground
  - blue `#1F4E8C`
  - brown `#7A4A22`
  - burnt orange `#C2571A`, kept visibly different from the heater orange

  Junction dots take the wire's color. The newsletter's own figures use black wires only.
- **Grid:** G = 10 px = 0.1 in (100 px = 1 in). Every hook sits on this grid, including after rotating or flipping. Ref des and value labels snap to 0.05 in (`snapL`).
- **Line weights in the parts panel** are fixed screen widths (`vector-effect: non-scaling-stroke`). Each tile has a minimum view window, so small parts aren't blown up.

## Symbol library conventions

- **Definitions** take the form `def(type, { cat, name, pre, value, tags, hooks: { NAME: [x, y] }, draw: () => svg })`.
  - Drawing helpers: `ln`, `pl`, `pg`, `pa`, `ci`, `stadium`, `cath`, `filV`.
  - CSS classes: `ink`, `env` (glass), `dash` (grids), `htr`, `solid` (filled arrows and dots), `tgt`.
- **The stock symbols stay hardcoded in the app.** This was decided on purpose.
  - Deferred: a "Create from stock" right-click item that copies a stock symbol into an editable custom symbol, probably with a popup editor. Custom symbols may live in an external file, probably JSON.
- **Designators** default to `pre + '?'` (V?, R?, Q?, D?). **Tubes have no default value**; part numbers go in `tags` so search finds them.
  - Deferred: a "fixed tubes" section with real part numbers.
- **Heaters inside tubes** use the triangle filament: `pl([[-10,end],[-10,base],[0,base-10],[10,base],[10,end]], 'htr')`. Standalone heater parts come in arch and triangle versions.
- **Panel groups** are listed in `CATS`, in this order: Passive, Semiconductors, Tube modern, Transformer, Heater, Power, Misc, Tube 1930s, Tube 1920s.
- **Semiconductors** are laid out in rows: diodes, then BJTs and Darlingtons, then FETs (JFETs and MOSFETs). Each row starts on a fresh line.
- **Tube, modern** has these subgroups, in order:
  1. Filament (direct), types ending `_dh`
  2. Cathode only
  3. Cathode + filament, types ending `_ih`
  4. Gas-filled, types starting `m_gas`, marked with a solid dot
  5. Specialized, types starting `m_sp_`: photomultiplier, then the single-section eye, then the two-section eye
- **Period tubes** are drawn in the style of their era. 1920s tubes are sideways with zigzag grids, and 1930s tubes are the same rotated 90°. From 1937 on, the modern symbols apply.
- **Parts with an adjustable shape** declare `vary: { key, label, min, max, def, build(n) }` and are always read through `shapeOf(p)`. Never use `LIB[type].hooks` directly for a placed part.
  - The photomultiplier, `pmtBuild(n)`, takes 2 to 14 dynodes. K and D1 stay fixed.
  - `setVary` drags wires on hooks that move and leaves wires on removed hooks where they are.

## App behavior worth knowing

- **Parts:** ref and value labels can be dragged and hidden independently. Delete on a selected label hides it.
  - Tubes can show editable `p?` pin numbers, with a dotted leader line when a number is moved.
- **Transformer winding text:** every part in the Transformer category can carry `pri` and `sec` text (volts, turns, impedance…), shown by `showPri` / `showSec`. New transformers start with `Pri` / `Sec`, both shown (`xfmrDefaults`, applied on placement, in the example and on load for parts that have neither field yet). The right-click menu edits, hides or shows them. Offsets are `pdx/pdy` and `sdx/sdy`. Label fields are listed in `LFIELD`; read visibility through `fieldShown(p, f)` and set it with `setShown`.
  - Default spot (`windingPos`): above the part, primary text right-aligned over the primary side and secondary text left-aligned over the secondary side; when the part is turned on end, to the left of each winding.
  - Delete (or Cut) on a selected winding label only hides it; the text is kept. A winding label left empty after editing is hidden again.
  - Primary hooks are the leftmost column of hooks in the symbol's own coordinates, secondary the rightmost. Keep that true for any new transformer symbol.
- **Transformer cores:** `p.core` is `air` (no lines), `ferrite` (dotted, CSS class `dotl`), `iron` (solid, the default when unset) or `var` (iron plus a diagonal adjust arrow). Chosen from the right-click menu. Every transformer `draw(c)` takes the core and draws it with `coreSVG(c, y0, y1)`; `partSVG` passes `p.core` for transformers, and thumbnails use the iron default. A new transformer symbol must draw its core through `coreSVG`. The `dotl` class is also in the export CSS.
- **The plain `transformer` type is "Transformer, isolation"**, value `Isolation`.
- **Step-up and step-down** (`transformer_up`, `transformer_down`) are drawn with 4 coil turns on the higher-voltage winding and 2 on the lower.
- **Flips:** Flip horizontal (X) and Flip vertical (Y) mirror a part about the center of its hooks, so a two-ended part reverses in place. Hooks that still land on wires keep them.
- **Tools:** Select V, Wire W, Text T, Cut C (whole item), Snip S (one wire segment).
  - Space flips the corner while drawing a wire.
  - Alt+wheel pans horizontally; Shift+wheel pans vertically.
- **Local storage keys:** `gs.current`, `gs.doc`, `gs.grid`, `gs.hooks`, `gs.panelw`, `gs.closed`, `gs.wirecolor`, `gs.broken`, `gs.cam`.
  - Loading is defensive: saved data is cleaned up first, and a failed load falls back to the example sheet.
  - `#fresh` starts on the example sheet without opening saved work.
- **Live camera (K / Camera button):** a floating webcam window over the sheet, so Larry can show his bench while he records training videos with his own production software. It is display only: nothing is recorded, and it is a DOM overlay, so it never appears in exports.
  - Drag to move, corner dot to resize, hover bar for camera picker, mirror, round/rectangle and close. Settings (position, size, mirror, shape, camera) persist in `gs.cam`.
  - The camera is never opened on page load, only from the button or K. Closing it stops the camera tracks so the camera light goes off.
  - The app always runs locally from a PC (file://). Larry may remove this feature from the production build.
  - Possible future: let users record their own training videos (screen + camera + mic via `getDisplayMedia` and `MediaRecorder`) to post. Not started; do not build until Larry asks.
- **Testing:** the main script is one IIFE. To reach its internals from Playwright, inject `window.__t = {...}` just before the final `})();`, in a scratch copy only.
