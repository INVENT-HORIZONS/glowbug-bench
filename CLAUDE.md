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
  - The Stock subgroup (Tube, modern) holds real tubes with pinouts; Larry is adding families one at a time, starting with the 12AX7 family.
- **Heaters inside tubes** use the triangle filament: `pl([[-10,end],[-10,base],[0,base-10],[10,base],[10,end]], 'htr')`. Standalone heater parts come in arch and triangle versions.
- **Panel groups** are listed in `CATS`, in this order: Passive, Semiconductors, Tube modern, Transformer, Power, Misc, Tube 1930s, Tube 1920s. The heater parts keep `cat: 'Heater'` but have no section of their own: Larry moved them into Tube, modern as the Heaters subgroup.
- **Semiconductors** are in three folding subgroups, in order: Diodes, Bipolar transistors (NPN, PNP and the Darlingtons) and Field effect transistors (JFETs and MOSFETs). They use the same `sub()` / `grp()` helpers as Tube, modern, remembered in `gs.closed` as `Semiconductors|<header>`.
  - Diodes, in order: diode, Schottky, Zener, LED (`diode_led`, value `Red`).
  - LED color: `p.led` (key in `LED_COLORS`; unset reads as `red`) colors only the body (triangle and bar) and the two light-out arrows, never the leads. `ledDraw(c)` writes the color as inline styles so exports keep it; the def's `drawArg` hands `p.led` to `draw`. Default red is the power-wire red `#A3241B`, chosen by Larry to fit the color scheme. Set from the right-click menu or Properties via `setLedColor`, which also renames a value that names a color (not for Black).
- **Tube, modern** subgroups each fold on their own (a `<details class="subgrp">` per subgroup, built by `sub()` / `grp()` in the panel code, shared with Semiconductors). Closed ones are remembered in `gs.closed` beside the main groups, keyed `Tube, modern|<header>`; a search opens them all. They are, in order:
  1. Directly heated (header; formerly "Filament (direct)"), types ending `_dh`
  2. Indirectly heated (header; formerly "Cathode + filament"), types ending `_ih`. Every one can hide its heater (`p.noHeater`): after the defs, each gets `L.bare`, its own drawing with the `htr` elements stripped and the `H1`/`H2` hooks dropped, every other hook unchanged. Larry chose this over separate cathode-only symbols.
     - The cathode-only tubes (`m_diode`, `m_rect_k`, `m_triode`, `m_twin`, `m_tetrode`, `m_pentode`, `m_beam`) are retired: `legacy: true`, hidden from the panel and search, still defined so older drawings open. The twin triode had no heater version; the stock 12AX7 and 6SN7 families cover twin triodes.
  3. Stock, types starting `m_st_`: real tubes with their pinout (Larry's stock vacuum tube symbols), in three groups between Indirectly heated and Gas-filled: **Stock: rectifiers** (defs with `stock: 'rect'`), **Stock: signal tubes** (defs with no `stock` key) and **Stock: All American Five** (defs with `stock: 'aa5'`, or `alsoIn: ['aa5']`, for the tubes of the classic five-tube series-heater radio; so far the 35W4). An empty group shows its header with a "No stock … yet." note; during a search it hides when nothing matches. Stock tubes keep their tube type as the default value (other tubes start blank).
     - Stock rectifiers (`stock: 'rect'`) are listed in order by name, sorted with numbers read as numbers (`localeCompare` numeric), so 6X5 comes before 35W4 (Larry asked for alphabetical order). All are single-section parts (no `units`, so the ref has no letter); `pins` on the def are copied to the part by `stockDefaults` with `showPins` on. Pinouts are from the r-type.org base tables; family lists mostly from radiomuseum.org's identical / other bulb / other quality groups.
       - 5AR4 family (`m_st_5ar4`, `FAM_5AR4`): pin 1 internal, 2 heater, 4 anode 2, 6 anode 1, 8 heater + cathode. Drawn as the cathode tied to the heater leg at pin 8 with a junction dot, no cathode pin, so it cannot hide its heater.
       - 5R4 family (`m_st_5r4`, `FAM_5R4`), 5U4 family (`m_st_5u4`, `FAM_5U4`) and 5Y3 family (`m_st_5y3`, `FAM_5Y3`): directly heated, pin 2 filament, 4 anode 2, 6 anode 1, 8 filament (B+ side). The filament is the cathode, so no Hide heater. The 5Y4GT has different pins and is not in the 5Y3 family.
       - 6CA4 family (`m_st_6ca4`, `FAM_6CA4`, B9A; EZ81 table): pin 1 anode 2, 3 cathode, 4 and 5 heater, 7 anode 1; pins 2, 6, 8, 9 internal connections, not drawn.
       - 6X4 family (`m_st_6x4`, `FAM_6X4`, B7G; EZ90 table): pin 1 anode 2, 3 and 4 heater, 6 anode 1, 7 cathode. 6X5 family (`m_st_6x5`, `FAM_6X5`, octal; EZ35 table): pin 1 metal shell, not drawn, 2 and 7 heater, 3 anode 2, 5 anode 1, 8 cathode.
       - The 6CA4, 6X4 and 6X5 are built with `stockRectIH(type, name, value, family, tags, pins)`, which gives the shared drawing and a `bare` shape so they can hide their heaters (`heaterBare` returns `L.bare` for single-section parts). Use it for any further indirectly heated full-wave rectifier with its own cathode pin.
       - 35W4 family (`m_st_35w4`, `FAM_35W4`, B7G half-wave): pin 3 and 4 heater, 5 anode, 6 heater tap, 7 cathode, 1 and 2 nc. The heater tap is a line down from the heater's apex to its own hook `HT`; hiding the heater removes H1, HT and H2. It is an All American Five tube, so it also shows in Stock: All American Five (`alsoIn: ['aa5']` on a def lists a part in a second stock group as well as its own).
     - The Tube type chips show for any part with a `family`; the Sections items only for parts with `units`.
     - A stock part can have sections: the def carries `units: { A: { hooks, draw, pins }, B: … }`. Each section is its own placed part with `p.unit`, linked to its siblings by `p.grp`. The ref displays as ref + unit (`refText`, e.g. V1A, V1B); `p.ref` holds just V1. `syncGroup(p)` copies ref and value to the siblings and is called from every ref/value edit path. `addSection(p, u)` places a missing section 120 px to the right. Paste gives pasted sections new groups.
     - `stockDefaults` (placement and load) sets unit, group and the section's pins (`showPins` on). `shapeOf` returns the section's shape.
     - Families: `family: [...]` on the def lists tube types with the same pinout; picked from the "Tube type" chips in the right-click menu or the Properties select (`setTubeType`, applies to every section).
     - 12AX7 family (`m_st_12ax7`, `FAM_12AX7`): Larry chose V1A / V1B labels, A = pins 1, 2, 3 (plate, grid, cathode), B = 6, 7, 8, and the heater on section A (H1 pin 4, H2 pin 5, HCT centre tap pin 9). Section B has no heater.
     - 6SN7 family (`m_st_6sn7`, `FAM_6SN7`, 8BD octal): pinout checked against the 6SN7EH datasheet (pins 1-2-3 grid, plate, cathode of unit 2; 4-5-6 unit 1; heater 7 and 8, no centre tap). Same convention as the 12AX7: A = pins 1, 2, 3 with the heater, B = 4, 5, 6.
     - 6SL7 family (`m_st_6sl7`, `FAM_6SL7`): its own part at Larry's request, sharing the 6SN7's sections (`units: ST6SN7`; r-type.org 6SL7GT table confirms the same 8BD pins). The 6SL7 types were moved out of the 6SN7 family, so each type is in one family only.
     - 6267 family (`m_st_6267`, `FAM_6267`, single section): the EF86 pentode on B9A (r-type.org EF86 table: 1 g2, 2 and 7 internal shield, not drawn, 3 k, 4 and 5 h, 6 a, 8 g3, 9 g1). Defined after the `_ih` loop so it can reuse `m_pentode_ih`'s hooks, draw and `bare`. Its screen-grid pin label uses `below: true`, a pin option that puts a side label under its lead instead of above, for closely spaced leads.
     - Section A's heater can be hidden the same way (`p.noHeater`, `setHeater`, `heaterBare`, right-click "Hide heater" or the Properties checkbox): the section's `bare` shape keeps P, G and K exactly where they are, so only the heater hooks come and go and a wire left on a heater pin can never land on the cathode.
  4. Gas-filled, types starting `m_gas`, marked with a solid dot
  5. Specialized, types starting `m_sp_`: photomultiplier, then the single-section eye, then the two-section eye
  6. Heaters (at the bottom, at Larry's request): the standalone heater parts (`cat: 'Heater'`: heater, center tap, triangle, center tap triangle), pulled into the Tube, modern list by the panel's item filter.
- **Period tubes** are drawn in the style of their era. 1920s tubes are sideways with zigzag grids, and 1930s tubes are the same rotated 90°. From 1937 on, the modern symbols apply.
- **Parts with an adjustable shape** declare `vary: { key, label, min, max, def, build(n) }`, or a list of settings `vary: [{ key, label, min, max, def }, …]` plus `build({ key: n, … })` on the def. Read settings with `varyList(L)` / `varyVal(p, v)`, change them with `setVary(p, n, key)`, and always read the shape through `shapeOf(p)`. Never use `LIB[type].hooks` directly for a placed part.
  - The photomultiplier, `pmtBuild(n)`, takes 2 to 14 dynodes. K and D1 stay fixed.
  - `setVary` drags wires on hooks that move and leaves wires on removed hooks where they are.

## App behavior worth knowing

- **Parts:** ref and value labels can be dragged and hidden independently. Delete on a selected label hides it.
  - Tubes can show editable `p?` pin numbers, with a dotted leader line when a number is moved.
- **Transformer winding text:** every part in the Transformer category can carry `pri` and `sec` text (volts, turns, impedance…), shown by `showPri` / `showSec`. New transformers start with `Pri` / `Sec`, both shown (`xfmrDefaults`, applied on placement, in the example and on load for parts that have neither field yet). The right-click menu edits, hides or shows them. Offsets are `pdx/pdy` and `sdx/sdy`. Label fields are listed in `LFIELD`; read visibility through `fieldShown(p, f)` and set it with `setShown`.
  - Default spot (`windingPos`): above the part, primary text right-aligned over the primary side and secondary text left-aligned over the secondary side; when the part is turned on end, to the left of each winding.
  - Delete (or Cut) on a selected winding label only hides it; the text is kept. A winding label left empty after editing is hidden again.
  - Primary hooks are the leftmost column of hooks in the symbol's own coordinates, secondary the rightmost. Keep that true for any new transformer symbol.
- **Transformer outline box:** `p.box` draws a light gray (`#B4B4B4`, CSS class `xbox`, also in the export CSS) rectangle `XBOX_PAD` (8 px) outside the symbol. Toggled from the right-click menu or the Properties checkbox. `localBB(p)` includes the box, so `partBB`, label placement, selection, Fit drawing and exports all clear it.
- **Transformer cores:** `p.core` is `air` (no lines), `ferrite` (dotted, CSS class `dotl`), `iron` (solid, the default when unset) or `var` (iron plus a diagonal adjust arrow). Chosen from the right-click menu. Every transformer `draw(c)` takes the core and draws it with `coreSVG(c, y0, y1)`; `partSVG` passes `p.core` for transformers, and thumbnails use the iron default. A new transformer symbol must draw its core through `coreSVG`. The `dotl` class is also in the export CSS.
- **The plain `transformer` type is "Transformer, isolation"**, value `Isolation`.
  - It, the step-up / step-down and the versatile transformers declare `layout: 'xfmr'`, so `xfmrLabelPos` places their labels: ref des centered on top, value centered below, Pri left of the primary winding, Sec right of the secondary (Larry's chosen default look). Turned on end: ref des and value to the right, Pri and Sec above and below their windings. Pri and Sec sit in the widest gap between that winding's hooks, so they stay clear of tap wires.
- **Versatile transformer** (`transformer_vers`, value `Versatile`, flag `windings: true`): the isolation look, drawn by loops. `p.pw` and `p.sw` are lists of loops per section (terminal or tap to the next), one loop = 10 px, so every tap is on the grid. Read them with `windingsOf(p)`, which also converts parts saved with the older `pt` / `st` tap counts (16 loops, even taps). Default `[16]` and `[16]`. Limits: `VERS_TAPS` 6 taps and `VERS_MAX` 40 loops per winding.
  - Shape: `versBuild(pw, sw)`. The taller winding sets the height (hooks from y 0 to loops·10 + 20); the shorter is centered beside it, on the grid. Taps are hooks `PT1…` / `ST1…`, numbered top down. `shapeOf` caches shapes by the two lists.
  - The only way to change windings is `windingDialog(p)` ("Configure windings…" in the right-click menu and Properties). Larry chose this over quick tap/loop chips in the menu, which he removed as clutter; don't add them back.
  - `windingDialog(p)`: per-section loop counts, "+ Tap here" splits a section, "Remove tap" joins two, "Even out taps", live preview. It tracks each tap's identity so Apply renames hooks correctly and wires stay on the same physical tap.
  - Any shape change that should keep wires attached goes through `reshape(p, mutate, rename, msg)`.
- **Retired transformers:** secondary CT, ultra-linear and primary CT (`transformer_sct`, `transformer_ul`, `transformer_pct`) carry `legacy: true`. They are hidden from the parts panel and search but stay defined so older drawings still open. Larry replaced them with the versatile transformer.
- **Step-up and step-down** (`transformer_up`, `transformer_down`) are drawn with 4 coil turns on the higher-voltage winding and 2 on the lower. Values `Stepup` and `Stepdown`.
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
  - Resolution: `camStart` asks for an ideal 1280 × 720, so 1080p and 4K cameras run at 720p. Larry finds 720p not really usable for his videos. Deferred until he has a better camera: ask for 1920 × 1080, or add a resolution choice (720p / 1080p / camera's best) to the camera window's hover bar.
  - Possible future: let users record their own training videos (screen + camera + mic via `getDisplayMedia` and `MediaRecorder`) to post. Not started; do not build until Larry asks.
- **Testing:** the main script is one IIFE. To reach its internals from Playwright, inject `window.__t = {...}` just before the final `})();`, in a scratch copy only.
