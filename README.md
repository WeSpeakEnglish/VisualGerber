# Gerber 3D Viewer

A single-file, browser-based 2D and 3D viewer for PCB Gerber, Excellon drill and pick-and-place files.
Open `index.html` in a browser, drop in your fabrication files (loose or zipped),
and inspect the board as flat layers or as a 3D model with solder mask, surface finish, silkscreen, inner layers, drilled holes and components.

No installation, no build step and no server. Files are processed locally in your browser and are never uploaded.

**Try it now>** [LIVE](https://wespeakenglish.github.io/VisualGerber/)

## Quick start

1. Open `index.html` in Chrome, Edge or Firefox (a recent version with WebGL).
2. Click **Open files…** or drag files onto the page. You can drop:
   - individual Gerber, drill and pick-and-place files,
   - a whole folder,
   - one or more `.zip` archives (subfolders inside the zip are fine).
3. The board opens in **2D**. Switch to **3D** with the toolbar switch (or press **2** / **3**).

> The page loads three.js and JSZip from public CDNs, so an internet connection is needed when it opens.
> After that, all processing happens offline in the browser.

## Features

### Views

- **2D | 3D switch** at the start of the toolbar. 2D is a flat top or bottom view you pan and zoom; 3D lets you rotate the board. The last choice is remembered.
- **Style switch** in the toolbar: **Layers** (each layer in its own color, like a classic Gerber viewer) or **Realistic** (looks like a manufactured board). 2D and 3D keep their own style; by default 2D uses Layers and 3D uses Realistic.
- **View buttons**: Top and Bottom in both modes; Iso (angled view) and Flip in 3D. The current view is underlined.
- **X-ray**: makes the board material see-through while copper, mask and silkscreen stay solid, so the other side, inner layers and plated holes show through.
- **Exploded view**: pulls copper, mask, silkscreen, paste and inner layers apart into a stack.
- **Cross-section**: cuts the board along X or Y at a position you choose.

### Board and layers

- **Real board shape** from the outline layer, including internal cutouts, slots and notches. Plated and non-plated holes are cut through the board, and plated holes get metal barrels.
- **All common layers drawn**: top and bottom copper, solder mask, silkscreen and paste, any number of inner copper layers, outline and drills.
- **One card per file**: each file card has a show/hide checkbox, an opacity slider, a color dot and the layer-type dropdown, listed in physical order (top silkscreen down to bottom silkscreen, then outline, drills and pick-and-place). Hiding one file hides only that file, so for example the PTH and NPTH drill files can be switched separately. Click a dot to change that layer's color; Layers and Realistic styles keep separate palettes, and **Reset colors** under the list restores the defaults of the current style.
- **Components** from a pick-and-place file. In 3D they are boxes sized from the package name; in 2D they are flat outlines so pads and silkscreen stay visible (bottom-side parts are drawn fainter). Hover a part to see its designator, value, package and side.

### Nets and pins

- When the Gerbers carry **X2 netlist attributes** (KiCad and recent Altium exports can include them), hovering a pad or track shows its **net**, the **part and pin** (for example *I2C_SDA · U1 pin 5*) and the layer.
- **Click** a pad or track to highlight the whole net on every copper layer; the rest of the board dims. A label shows the number of layers and pads and the total track length. **Esc** or **×** clears it.
- The Board panel says how many nets were found, or that the files have no net names.

### Fab check

- Checks the board against the published standard limits of **JLCPCB** or **PCBWay** (choose in the panel): board size, narrowest track, smallest plated and non-plated holes, largest hole, via and pad annular rings, hole-to-hole spacing, hole and copper distance to the board edge, and the thinnest silkscreen line.
- Each line shows the measured value, the limit and ✓ / ! (allowed but below the recommended value) / ✗.
- Values are measured from the Gerbers. Spacing between copper features is **not** checked, and limits change over time — confirm on the manufacturer's capabilities page before ordering.

### Appearance

- Solder mask color (green, red, blue, black, white, purple, yellow), surface finish (ENIG, HASL, bare copper), silkscreen color and board thickness (presets from 0.4 to 3.2 mm, or any custom value from 0.1 to 10 mm). Per-side colors can be set with the dots on the file cards.
- **Board material** row: show or hide the board material (in Layers style, the board background), change its color, and set its opacity (the same setting the X-ray button uses).
- Viewport background: follow the light/dark theme, pick a preset or any custom color.
- Light, dark or automatic theme (button in the header).

### Measuring

- Press **M** or click **Measure**, then click two points. Shows distance, ΔX / ΔY, angle and both coordinates.
- **Units**: switch between **mm**, **mil** and **inch** in the Board panel; measurements, coordinates, board size, net lengths and the fab check all follow.
- Points snap to pad, hole and component centres; hold **Alt** to place freely. **Esc** clears.
- The cursor position in board coordinates is shown in the bottom-right corner.

### Files

- Each file can be removed with its **×** button; **Remove ignored** drops all files that are not drawn; **Clear all** removes everything. Every removal can be undone for a few seconds.
- Files that are loaded but not drawn (reports, aperture lists, Altium pad masters, mechanical layers, binary drill files) are listed with a short explanation. So are files that were skipped: files over 60 MB, zips inside a zip and damaged zips.
- **Your last board reopens automatically** after a page reload, with a notice that has a **Clear** button.

### Export

- **PNG** image of the current view.
- **GLB** 3D model with colors and textures, in real-world scale.
- **STL** mesh of the board, plated holes and components, in mm.

### Remembered settings

Colors, opacities, style, 2D/3D choice, units, fab-check manufacturer, thickness, background and theme are saved in the browser and restored next time. The loaded files are kept in the browser's own storage (IndexedDB) so the last board reopens after a reload; they never leave your computer, and **Clear all** removes them. Very large sets (over about 150 MB) are not kept.

## Supported input

### Gerber (RS-274X / X2)

- Coordinate format (`FS`), units (`MO`), including legacy `G70`/`G71`
- Standard apertures: circle, rectangle, obround, polygon (with holes)
- Aperture macros: circle, vector line, center line, lower-left line, outline, polygon, moiré, thermal; variables and arithmetic expressions
- Linear and circular interpolation (`G01`, `G02`, `G03`), single and multi-quadrant (`G74`, `G75`)
- Regions (`G36`/`G37`), dark and clear polarity (`LPD`/`LPC`)
- Step-and-repeat (`SR`)
- Block apertures (`AB`), including nested blocks and blocks flashed with clear polarity
- Aperture transformations: mirroring (`LM`), rotation (`LR`) and scaling (`LS`), applied to flashes, draws and block apertures
- X2 file attributes (`%TF.FileFunction`) for automatic layer detection, including inner layer order
- X2 object attributes (`%TO.N` net, `%TO.P` pad, `%TO.C` component, `%TD`) for net and pin information

### Excellon drill

- Metric and inch, leading and trailing zero formats, decimal coordinates
- Format hints from `;FILE_FORMAT=` comments and `METRIC,000.000`-style headers
- Tool definitions in the header or body
- Slots via `G85` and routed paths (`G00` / `M15` / `G01` / `M16`)
- Plated vs. non-plated detection from the file name (`NPTH`) or `TYPE=NON_PLATED` comments
- Drill data exported in Gerber format is also accepted

### Pick and place (centroid)

- KiCad `.pos`, Altium Pick Place (`.txt` / `.csv`, old and new formats), JLCPCB CPL `.csv`, EAGLE `.mnt` / `.mnb` and similar files
- Comma, semicolon, tab or space separated; units in mm, mil or inch (from the header or the values)
- Mirrored bottom-side X coordinates and a different placement origin are corrected automatically; the file row says when this happened

## Layer detection

Each file's layer is detected automatically, in this order:

1. **File content**: Excellon headers (`M48`), then the Gerber X2 `FileFunction` attribute, then pick-and-place column headers.
2. **File name**, using common CAD conventions:

| Layer | KiCad | Altium / Protel | EAGLE |
|---|---|---|---|
| Top copper | `F_Cu` | `.GTL` | `.cmp` |
| Bottom copper | `B_Cu` | `.GBL` | `.sol` |
| Top mask | `F_Mask` | `.GTS` | `.stc` |
| Bottom mask | `B_Mask` | `.GBS` | `.sts` |
| Top silkscreen | `F_Silkscreen` / `F_SilkS` | `.GTO` | `.plc` |
| Bottom silkscreen | `B_Silkscreen` / `B_SilkS` | `.GBO` | `.pls` |
| Top / bottom paste | `F_Paste` / `B_Paste` | `.GTP` / `.GBP` | `.crc` / `.crs` |
| Outline | `Edge_Cuts` | `.GKO`, `.GM1` | `.dim` |
| Inner copper | `In1_Cu`… | `.G1`, `.G2`… | — |
| Drill | `.drl` | `.TXT` (ASCII) | `.drd`, `.xln` |
| Pick and place | `-pos.csv`, `.pos` | `Pick Place….txt` / `.csv` | `.mnt`, `.mnb` |

**Board outline**: when several outline-type files are present (for example Altium `.GKO` plus a `.GM1` that holds the drawing frame), the outline is the smallest closed shape that encloses the pads and holes. Larger shapes such as drawing frames and title blocks are ignored, and the Board panel shows which file the outline came from.

If a layer is detected incorrectly, change it with the dropdown next to the file name. Set a file to **Ignore** to exclude it.

Non-Gerber files inside a zip (PDF, images, `.gbrjob`, reports, etc.) are skipped automatically.

## Controls

| Action | Mouse | Touch |
|---|---|---|
| Rotate (3D) | Left-drag | One-finger drag |
| Pan | Right-drag (3D), left-drag (2D) | Two-finger drag (3D), one-finger drag (2D) |
| Zoom | Scroll wheel (zooms toward the cursor) | Pinch |

| Key | Action |
|---|---|
| **2** / **3** | Switch to 2D / 3D |
| **T** / **B** | Look at the top / bottom |
| **I** | Angled 3D view |
| **F** | Flip the board (3D) |
| **X** | X-ray on/off |
| **M** | Measure on/off |
| **Alt** (while measuring) | Place a point without snapping |
| **Esc** | Clear the measurement and net highlight |
| **?** | Show the shortcut list |

Click a pad or track to highlight its net (files with net names).

Toolbar: **2D | 3D**, **Style** (Layers / Realistic), **Iso**, **Top**, **Bottom**, **Flip**, **X-ray**, **Measure**, **PNG**, **?** (shortcuts).

## Limitations

- **Missing outline**: without an outline layer, the board is framed around the pads and holes (or, with no pads, the bounding box of all layers) plus a 1 mm margin.
- **Holes that cannot be cut**: holes that overlap each other or the board edge, and any beyond 4000, are drawn flat on the surface instead of cut through.
- **Binary drill files**: Altium's binary EIA `.DRL` cannot be read; use the ASCII NC drill file (`.TXT`) from the same export.
- **Texture resolution**: this is limited by your GPU (usually 4096 px across the longest board side). Very large boards show less fine detail. While you drag a slider or color on a large board, a quicker lower-resolution preview is shown and the sharp version follows when you stop.
- **Components are approximate**: parts are simple boxes sized from the package name, not real 3D models. Pin headers and connectors are drawn as their low plastic body.
- **Nested zips**: a zip inside a zip is not opened (it is listed with a note). Extract it first.

## Third-party libraries

These are loaded at runtime from public CDNs:

- [three.js](https://threejs.org/) r160, MIT License
- [JSZip](https://stuk.github.io/jszip/) 3.10.1, MIT License (dual-licensed MIT / GPLv3)

## License

Released under the MIT License. See [LICENSE](LICENSE).
