# Glowbug Bench

A browser-based schematic editor for **The Glow Report**, a weekly vacuum-tube newsletter. It draws with the newsletter's own symbol library, so schematics made here match the figures in print.

Open `glowbug-bench.html` in any modern browser. Everything is in the one file; no install or build step.

## Features

- **Symbol library (95 parts):** passives (including a two-terminal rheostat, film and variable capacitors and a tapped inductor), transformers (isolation, step-up, step-down, and a tall versatile transformer with 0 to 6 taps on each winding), heaters, power, semiconductors (diodes including photodiode, tunnel, varactor, laser and LED, BJTs, Darlingtons, JFETs and MOSFETs), misc (fuse, SPST switch, push button, SPDT and DPDT switches, a rotary switch with 2 to 12 positions, pilot lamp, speaker), modern tubes (including gas-filled rectifiers, glow regulators, thyratrons, a photomultiplier and two- and single-section electron-ray "magic eyes"), and 1920s and 1930s period tubes for the Vintage Tube spotlight.
- **Hooks on a 0.1 in grid:** every connection point lands on the grid, even after rotating or flipping. Ref des and value labels move on a finer 0.05 in grid.
- **Wiring:** click a hook to start, click to add corners, and finish on a hook or wire. Space flips the corner. Junction dots appear automatically.
- **Wire colors:** black, red, green, blue, brown and orange. New wires are black; recolor wires from the properties panel or the right-click menu.
- **Editing:**
  - Drag parts and the wires follow.
  - Drag a wire segment to move it.
  - Right-click to rotate, flip horizontal, flip vertical or delete.
  - Double-click a part, its ref des or its value to edit.
  - Delete on a selected label hides it instead of deleting anything.
- **Pin numbers on tubes:** right-click a tube and choose Show pin numbers. Each hook gets a `p?` label you can double-click to edit (Tab moves to the next pin) and drag anywhere; a moved label is tied back to its pin with a dotted line.
- **Center taps:** right-click an isolation, step-down or step-up transformer (or use Properties) to add a primary center tap, a secondary center tap, or both. Each tap is a new connection point at the middle of its winding.
- **Versatile transformer:** each winding is drawn with a number of loops (one loop is 0.1 in), so you can show a step-up or step-down ratio and put taps anywhere. Use it for center taps, ultra-linear taps, multi-tap power transformers and the like.
  - **Configure windings…** (right-click or Properties) opens a dialog with a live preview. Set the loops in each section between terminals and taps, add a tap in the middle of any section, or remove a tap. This makes asymmetric taps. Even out taps spreads them evenly again. The shorter winding is centered beside the taller one.
  - Wires stay on their taps: taps that move drag their wires along, and a removed tap leaves its wires where they were.
- **Transformer labels:** the isolation, step-up, step-down and versatile transformers place the ref des on top, the value (Isolation, Stepup, Stepdown) below, Pri left of the primary and Sec right of the secondary.
- **Hide a tube's heater:** right-click any indirectly heated tube (or untick show heater in Properties) to draw it without its heater. Only the heater connections go away; every other connection stays exactly where it was, so wires stay on. This replaces the old cathode-only tubes.
- **Folding groups:** each group under Tubes (Generic: directly heated, Generic: indirectly heated, the Stock groups, Generic: gas filled, Specialized, Heaters, Period: 1930s, Period: 1920s) and under Semiconductors (Diodes, Bipolar transistors, Field effect transistors) folds on its own when you click its header, and the parts panel remembers which ones you closed.
- **Stock tubes:** real tubes with their pinout, in the Stock groups under Tubes: Stock: rectifiers, Stock: signal tubes and Stock: All American Five.
  - **5AR4 family** (rectifier): plates on pins 4 and 6, heater on pins 2 and 8, with the cathode tied to pin 8 inside the tube. Tube types 5AR4, GZ34, CV1377, U77.
  - **5R4 family** (rectifier): plates on pins 4 and 6, filament on pins 2 and 8. Tube types 5R4G, 5R4GY, 5R4GYA, 5R4GYB, 5R4GB, 5R4WGY, 5R4WGB, CV717.
  - **5U4 family** (rectifier): plates on pins 4 and 6, filament on pins 2 and 8 (pin 8 is the B+ side). Tube types 5U4G, 5U4GA, 5U4GB, 5U4GT, 5U4GTY, 5AS4, 5AS4A, 5Z3S, U52, GZ31, VT-244, CV575, CV684.
  - **5Y3 family** (rectifier): plates on pins 4 and 6, filament on pins 2 and 8. Tube types 5Y3G, 5Y3GT, 5Y3GT/G, 5Y3WGT, 5Y3WGTA, U50, CV1856, VT-197A.
  - **6CA4 family** (9-pin rectifier): plates on pins 1 and 7, cathode on pin 3, heater on pins 4 and 5; the heater can be hidden. Tube types 6CA4, EZ81, CV5072, U709, UU12.
  - **6X4 family** (7-pin rectifier): plates on pins 1 and 6, cathode on pin 7, heater on pins 3 and 4; the heater can be hidden. Tube types 6X4, EZ90, CV493, U78, V2M70, 6063, E90Z, EZ900, 6202, 6BX4.
  - **6X5 family** (octal rectifier): plates on pins 3 and 5, cathode on pin 8, heater on pins 2 and 7; the heater can be hidden. Tube types 6X5GT, 6X5, 6X5G, 6X5GT/G, 6X5WGT, EZ35, CV573, CV574, VT-126B, U70, U147, 6Ц5С.
  - **35W4 family** (7-pin half-wave rectifier for series-heater sets): plate on pin 5, cathode on pin 7, heater on pins 3 and 4 with the panel-lamp tap on pin 6; the heater can be hidden. Tube types 35W4, HY90. It also appears in Stock: All American Five.
  - **12AX7 family:** a twin triode with pin numbers already filled in. Section A (V1A) is plate 1, grid 2, cathode 3 and carries the heater (pins 4 and 5, center tap 9); section B (V1B) is plate 6, grid 7, cathode 8. Place A, then right-click (or use Properties) and choose Add section B; drag each section wherever it belongs on the sheet. Right-click V1A (or use Properties) to hide or show its heater; the plate, grid and cathode stay put, and the heater pin numbers come back when you show it again.
  - **Tube type:** right-click either section to pick 12AX7, 12AT7, 12AU7, 12AY7, 12AV7, 12AZ7, 12BZ7, 12DW7, 5751, 7025, 6072, 6201, 6681, 5814, 6189, ECC81, ECC82 or ECC83 (same 9A pinout). Both sections change together, and renaming the ref on either renames both.
  - **6SN7 family:** an octal twin triode. Section A (V1A) is grid 1, plate 2, cathode 3 and carries the heater (pins 7 and 8); section B (V1B) is grid 4, plate 5, cathode 6. Tube types: 6SN7, 6SN7GT, 6SN7GTA, 6SN7GTB, 6SN7W, 5692, 6180, 8SN7, 12SN7GT and 25SN7GT. Sections, tube type and Hide heater work as on the 12AX7 family.
  - **6SL7 family:** the high-mu octal twin triode, on the same pins as the 6SN7 (V1A grid 1, plate 2, cathode 3 with the heater on 7 and 8; V1B grid 4, plate 5, cathode 6). Tube types 6SL7GT, 6SL7, 6SL7GTB, 6SL7W, 6SL7WGT, 6113, 5691, 12SL7GT, ECC35, VT-229, CV1984, CV1985, 6N9P, 6Н9С, 6SU7GTY.
  - **6267 family:** the low-noise 9-pin pentode (EF86). Plate on pin 6, suppressor grid 8, screen grid 1, control grid 9, cathode 3, heater 4 and 5 (the internal shield on pins 2 and 7 is not drawn); the heater can be hidden. Tube types 6267, EF86, 6BK8, 6CF8, 6F22, Z729, CV2901, CV8068, CV10098, EF86SQ, EF806S, M8195, CV4085.
- **LED color:** right-click an LED (or use Properties) to color its body and light arrows red (the default), orange, yellow, green, blue, white or black; the leads stay black. A value that names a color follows the change.
- **Transformer outline box:** right-click any transformer and turn on Gray outline box (or tick it in Properties) to draw a light gray rectangle around it. Its labels move out to clear the box, and it appears in exports.
- **Inductor cores:** the Inductor and the center-tap Winding have a Core choice in their right-click menu, like the transformers: air (the default, no lines), ferrite (dashed), iron (solid) or variable (iron with an adjusting arrow). The old iron-core Choke is now an Inductor set to iron; saved drawings convert on their own.
- **Transformer cores:** right-click any transformer to pick its core: air (no lines), ferrite (dashed lines), iron (solid lines, the default) or variable (iron with an adjusting arrow).
- **Winding text on transformers:** every transformer starts with "Pri" and "Sec" over its windings. Double-click either to change it to voltages, turns, impedance or anything else. Each has its own Show checkbox in Properties and in the double-click dialog, and the right-click menu can edit, hide or show them. Drag them anywhere; Delete on a selected one just hides it, and its text is kept.
- **Rotary switch positions:** right-click a rotary switch (or use the properties panel) to set 2 to 12 positions (5 by default). The contacts fan out on an arc around the wiper; wires on contacts that move follow them.
- **Photomultiplier dynodes:** right-click a photomultiplier (or use the properties panel) to set 2 to 14 dynodes. The photocathode and D1 stay put; the anode's wires follow it.
- **Toolbox:** Select (V), Wire (W), Text (T), Cut (C, removes a whole part or wire) and Snip (S, removes one wire segment).
- **Sheets:** ANSI A (Letter), B (Tabloid), C and D, portrait or landscape, with mixed sizes in one project. Each sheet can have:
  - A zoned border.
  - A revision chart.
  - A title block with logo, company, project, sheet title, file name, part number, size, revision, sheet *n* of *m*, and drawn, checked and approved by.
- **Movable blocks:** the title block and revision chart drag anywhere and stick to any corner.
- **Project tree:** schematics are listed by file name; double-click to rename.
- **Bench instruments:** voltmeter, ammeter, oscilloscope and signal generator buttons sit after Camera on the toolbar. Each opens a menu of instruments: voltmeters V1–V7, ammeters I1–I3, scope channels S1–S3, and generator channels AWG1 and AWG2. Picking one adds its white, hovering card to the current sheet, where you can drag it anywhere; each instrument can be in use only once (it is greyed out in the menu until you delete its card) and appears only on the sheet it was added to. The cards are empty for now and are saved with the project.
- **Circle (O) and Rectangle (B) tools:** click the center and then a point on the edge for a circle, or two opposite corners for a rectangle. Like lines they are drawing only, never wires, and can be selected, moved, recolored, copied and deleted. While the Line, Circle or Rectangle tool is on, the Properties panel sets the pen for new shapes: color, pen size (hairline to heavy) and line style (solid, dashed, long dash, dotted, dash dot). Select a shape to change its pen afterwards.
- **Link tool (J):** gang adjustable controls (potentiometers, rheostats, variable capacitors, and inductors or transformers with a variable core). It works exactly like the wire tool: start on a control's arrow tip, click to add corners, end on another control's arrow tip; Space flips the corner. To gang three or more controls, start or end a link on an existing link, just as a wire can end on a wire. The thin dashed orange link follows the controls, its segments drag like wire segments, and deleting either control or any piece of the link deletes the whole link.
- **Line tool (L):** click a start and an end to draw a straight line at any angle, for arrows, outlines and notes. Lines are drawing only, never wires. Select, move, recolor, copy or delete them like anything else.
- **Active Plot:** a toolbar button after the instruments. Click it and then click the sheet, or drag it onto the sheet, to drop an empty four-quadrant X–Y plot. Like a part, it can be moved, duplicated, copied and deleted, and it resizes from its corner handle. Waveform data comes later.
- **Potentiometer:** a dial button after Active Plot, for voltages that can be varied virtually (coming soon).
- **Slider:** a horizontal slider button next to the Potentiometer, for the same job with a different look (coming soon).
- **LED** and **Incandescent bulb:** two buttons after the Slider (coming soon).
- **Browser check:** Glowbug Bench is built for Google Chrome and Microsoft Edge. Opening it in any other browser shows a popup saying so; you can still carry on after clicking OK.
- **Settings:** the gear at the far right of the toolbar opens the Settings dialog (empty for now).
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
