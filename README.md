# Glowbug Bench

A browser-based schematic editor for **The Glow Report**, a weekly vacuum-tube newsletter. It draws with the newsletter's own symbol library, so schematics made here match the figures in print.

Open `glowbug-bench.html` in any modern browser. Everything is in the one file; no install or build step.

## Features

- **Symbol library (82 parts):** passives, transformers (isolation, step-up, step-down, and a tall versatile transformer with 0 to 6 taps on each winding), heaters, power, semiconductors (diodes including an LED, BJTs, Darlingtons, JFETs and MOSFETs), misc, modern tubes (including gas-filled rectifiers, glow regulators, thyratrons, a photomultiplier and two- and single-section electron-ray "magic eyes"), and 1920s and 1930s period tubes for the Vintage Tube spotlight.
- **Hooks on a 0.1 in grid:** every connection point lands on the grid, even after rotating or flipping. Ref des and value labels move on a finer 0.05 in grid.
- **Wiring:** click a hook to start, click to add corners, and finish on a hook or wire. Space flips the corner. Junction dots appear automatically.
- **Wire colors:** black, red, green, blue, brown and orange. Pick the color for new wires from the swatch under the Wire tool; recolor existing wires from the properties panel or the right-click menu.
- **Editing:**
  - Drag parts and the wires follow.
  - Drag a wire segment to move it.
  - Right-click to rotate, flip horizontal, flip vertical or delete.
  - Double-click a part, its ref des or its value to edit.
  - Delete on a selected label hides it instead of deleting anything.
- **Pin numbers on tubes:** right-click a tube and choose Show pin numbers. Each hook gets a `p?` label you can double-click to edit (Tab moves to the next pin) and drag anywhere; a moved label is tied back to its pin with a dotted line.
- **Versatile transformer:** each winding is drawn with a number of loops (one loop is 0.1 in), so you can show a step-up or step-down ratio and put taps anywhere. Use it for center taps, ultra-linear taps, multi-tap power transformers and the like.
  - **Configure windings…** (right-click or Properties) opens a dialog with a live preview. Set the loops in each section between terminals and taps, add a tap in the middle of any section, or remove a tap. This makes asymmetric taps. Even out taps spreads them evenly again. The shorter winding is centered beside the taller one.
  - Wires stay on their taps: taps that move drag their wires along, and a removed tap leaves its wires where they were.
- **Transformer labels:** the isolation, step-up, step-down and versatile transformers place the ref des on top, the value (Isolation, Stepup, Stepdown) below, Pri left of the primary and Sec right of the secondary.
- **Stock tubes:** real tubes with their pinout, in the Stock group under Tube, modern.
  - **12AX7 family:** a twin triode with pin numbers already filled in. Section A (V1A) is plate 1, grid 2, cathode 3 and carries the heater (pins 4 and 5, center tap 9); section B (V1B) is plate 6, grid 7, cathode 8. Place A, then right-click (or use Properties) and choose Add section B; drag each section wherever it belongs on the sheet. Right-click V1A (or use Properties) to hide or show its heater; the plate, grid and cathode stay put, and the heater pin numbers come back when you show it again.
  - **Tube type:** right-click either section to pick 12AX7, 12AT7, 12AU7, 12AY7, 12AV7, 12AZ7, 12BZ7, 12DW7, 5751, 7025, 6072, 6201, 6681, 5814, 6189, ECC81, ECC82 or ECC83 (same 9A pinout). Both sections change together, and renaming the ref on either renames both.
  - **6SN7 family:** an octal twin triode. Section A (V1A) is grid 1, plate 2, cathode 3 and carries the heater (pins 7 and 8); section B (V1B) is grid 4, plate 5, cathode 6. Tube types: 6SN7, 6SN7GT, 6SN7GTA, 6SN7GTB, 6SN7W, 5692, 6180, 8SN7, 12SN7GT, 25SN7GT, 6SL7, 6SL7GT, 5691 and 12SL7GT. Sections, tube type and Hide heater work as on the 12AX7 family.
- **LED color:** right-click an LED (or use Properties) to color its body and light arrows red (the default), orange, yellow, green, blue, white or black; the leads stay black. A value that names a color follows the change.
- **Transformer outline box:** right-click any transformer and turn on Gray outline box (or tick it in Properties) to draw a light gray rectangle around it. Its labels move out to clear the box, and it appears in exports.
- **Transformer cores:** right-click any transformer to pick its core: air (no lines), ferrite (dotted lines), iron (solid lines, the default) or variable (iron with an adjusting arrow).
- **Winding text on transformers:** every transformer starts with "Pri" and "Sec" over its windings. Double-click either to change it to voltages, turns, impedance or anything else. Each has its own Show checkbox in Properties and in the double-click dialog, and the right-click menu can edit, hide or show them. Drag them anywhere; Delete on a selected one just hides it, and its text is kept.
- **Photomultiplier dynodes:** right-click a photomultiplier (or use the properties panel) to set 2 to 14 dynodes. The photocathode and D1 stay put; the anode's wires follow it.
- **Toolbox:** Select (V), Wire (W), Text (T), Cut (C, removes a whole part or wire) and Snip (S, removes one wire segment).
- **Sheets:** ANSI A (Letter), B (Tabloid), C and D, portrait or landscape, with mixed sizes in one project. Each sheet can have:
  - A zoned border.
  - A revision chart.
  - A title block with logo, company, project, sheet title, file name, part number, size, revision, sheet *n* of *m*, and drawn, checked and approved by.
- **Movable blocks:** the title block and revision chart drag anywhere and stick to any corner.
- **Project tree:** schematics are listed by file name; double-click to rename.
- **Live camera:** press K or the Camera button to float a live webcam view over the sheet, for showing your bench while you teach. Drag it, resize it from the corner dot, and use its hover bar to pick a camera, mirror the picture, switch between round and rectangle, or close it. It is never recorded and never appears in exports.
- **Export:** whole sheet or drawing only, as SVG or PNG, plus a project file (.json) you can import later.

## Colors

The sheet uses the newsletter's palette: paper `#FBF8F3`, ink `#17120E`, and glow `#E8912D` for heaters and filaments only, and phosphor green `#2F8A45` for the magic-eye targets. Wires are ink unless you color them.

## Keys

| Key | Action |
| --- | --- |
| V / W / T / C / S | Select, Wire, Text, Cut, Snip |
| K | Show or hide the live camera |
| R / X / Y | Rotate, flip horizontal, flip vertical |
| Del | Delete the selection (hides a selected label) |
| Ctrl+Z / Ctrl+Y | Undo, redo |
| Ctrl+C / Ctrl+V / Ctrl+D | Copy, paste, duplicate |
| F / Shift+F | Fit sheet, fit drawing |
| Wheel / Shift+wheel / Alt+wheel | Zoom, pan up/down, pan left/right |
| Space (while wiring) | Flip the corner |

Add `#fresh` to the end of the address to start on the example sheet without opening saved work.
