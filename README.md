# Gerber 3D Viewer

A single-file, browser-based 2D and 3D viewer for PCB Gerber, Excellon drill, pick-and-place and ODB++ files,
including old EAGLE RS-274D sets with a `.WHL` aperture file. It can also compare two versions of a board and show what changed,
and export the board as a STEP solid for mechanical CAD.
Open `index.html` in a browser, drop in your fabrication files (loose or zipped),
and inspect the board as flat layers or as a 3D model with solder mask, surface finish, silkscreen, inner layers, drilled holes and components.

No installation, no build step and no server. Files are processed locally in your browser and are never uploaded.

**Try it now>** [LIVE](https://wespeakenglish.github.io/VisualGerber/)

## Quick start

1. Open `index.html` in Chrome, Edge or Firefox (a recent version with WebGL).
2. Click **Open files…** or drag files onto the page. You can drop:
   - individual Gerber, drill and pick-and-place files,
   - a whole folder,
   - one or more `.zip` archives (subfolders inside the zip are fine),
   - an ODB++ job as `.tgz`, `.tar`, `.zip` or its folder.
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
- **Castellated (half) holes**: a hole whose centre sits on a straight board edge is cut as a half-circle notch with a plated half-barrel. The Board panel counts them, and the fab check reminds you to order the castellated-hole option.
- **All common layers drawn**: top and bottom copper, solder mask, silkscreen and paste, any number of inner copper layers, outline and drills.
- **One card per file**: each file card has a show/hide checkbox, an opacity slider, a color dot and the layer-type dropdown, listed in physical order (top silkscreen down to bottom silkscreen, then outline, drills and pick-and-place). Hiding one file hides only that file, so for example the PTH and NPTH drill files can be switched separately. Click a dot to change that layer's color; Layers and Realistic styles keep separate palettes, and **Reset colors** under the list restores the defaults of the current style.
- **Components** from a pick-and-place file. In 3D they are boxes sized from the package name; in 2D they are flat outlines so pads and silkscreen stay visible (bottom-side parts are drawn fainter). Hover a part to see its designator, value, package and side.

### Nets and pins

- When the Gerbers carry **X2 netlist attributes** (KiCad and recent Altium exports can include them), hovering a pad or track shows its **net**, the **part and pin** (for example *I2C_SDA · U1 pin 5*) and the layer.
- **Click** a pad or track to highlight the whole net on every copper layer; the rest of the board dims. A label shows the number of layers and pads and the total track length. **Esc** or **×** clears it.
- The Board panel says how many nets were found, or that the files have no net names.

### Compare two versions

Click **Compare…** with a board loaded and pick another version of it: Gerbers, a zip, an ODB++ job or an old EAGLE set; the two versions do not have to come from the same tool. The compare view opens over the board:

- **Each layer** of both versions is drawn on the same grid: **grey** where they are identical, **red** where something is only in the loaded version (A, removed), **green** where it is only in the new one (B, added). A one-pixel tolerance hides edge rounding between exports.
- **Holes** are compared as a list: added, removed, **moved** (with the distance), **resized** (e.g. *1.100 mm → 1.200 mm*) or plating changed.
- **Parts** (when both versions have pick-and-place or ODB++ component data): added, removed, moved, rotated, value or footprint changed.
- **Offset between exports**: if B was exported with a different origin, the offset is found from the pads and removed, and the panel says by how much.
- **Change list**: every changed area with its size and position, largest first. Click one to zoom to it. Thin differences under 0.1 mm wide (typical of re-poured copper) are coloured on the board but not listed, so they do not bury the real changes.
- **Blink A/B** flips between the two versions automatically; **Space** flips by hand. **Hide unchanged** (or **H**) leaves only the changes; **F** fits the board; **Esc** closes.

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
- Files that are loaded but not drawn (reports, aperture lists, Altium pad masters, mechanical layers, drill drawings / drill maps, binary drill files, EAGLE aperture wheels and drill racks) are listed with a short explanation.
- Files converted from old EAGLE RS-274D data are marked *converted from RS-274D* on their card, and a notice after loading lists what was corrected. So are files that were skipped: files over 60 MB, zips inside a zip and damaged zips.
- **Your last board reopens automatically** after a page reload, with a notice that has a **Clear** button.

### Export

- **PNG** image of the current view.
- **GLB** 3D model with colors and textures, in real-world scale.
- **STL** mesh of the board, plated holes and components, in mm.
- **STEP** solid model for mechanical CAD (AP214, in mm), opened as real solids rather than a mesh:
  - The **board** is one solid built from the outline, with cutouts, slots and castellated notches as flat faces and every round hole as a true cylinder, so you can measure diameters and mate against hole walls.
  - **Components** from the pick-and-place data are named boxes (for example *R1 10k*), each a separate part in an assembly with the board. Top parts sit on the top surface; bottom parts hang below.
  - **Colors**: the board takes the solder mask color, components the same colors as in 3D.
  - **Coordinates** are the Gerber coordinates, with the board bottom at Z = 0 and the top at the board thickness.
  - The file is named after the zip or ODB++ job, or after the name the loose files share (e.g. `board.step`). A notice after saving gives the hole and part counts.

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

### Legacy EAGLE RS-274D (with `.WHL` aperture wheel)

Older EAGLE CAM jobs wrote RS-274D Gerbers, which have no aperture definitions in the file. The aperture sizes are in a separate `.WHL` wheel file, and the drill file holds only tool numbers. Drop the whole set (loose or zipped) and the viewer converts it to RS-274X and Excellon in memory:

| File | Used as |
|---|---|
| `.CMP` / `.SOL` | Top / bottom copper |
| `.LY2`, `.LY3`… | Inner copper |
| `.STC` / `.STS` | Top / bottom solder mask |
| `.PLC` / `.PLS` | Top / bottom silkscreen |
| `.CRC` / `.CRS` | Top / bottom paste |
| `.WHL` | Aperture wheel: pad and track sizes (**required**) |
| `.DRL` (tool numbers and `X…Y…` only) | Drill hits |
| Drill rack (e.g. `WOM.DRL`) or drill info (`.DRI` / `DRL.DOC`) | Hole sizes |
| `.PRT` | Hole sizes from the drill-legend texts, when no rack or info file is present |
| `.DCC` / `.DCS` | Documentation print: used to find the board outline, not drawn |

What the conversion does:

- **Apertures** are taken from the `.WHL` file (draw, round, square, rectangle, oval, octagon). Coordinates are read in EAGLE's 2.3 inch format.
- **Bottom layers** that EAGLE plotted mirrored or shifted are lined up with the top by matching pad positions.
- **Drill file**: the right rotation, mirroring and offset are found automatically by matching the holes to the copper pads. Holes on a copper pad become plated, and the rest non-plated.
- **Board outline** is the largest closed rectangle on the documentation, silkscreen or copper layer, written as an extra `…-outline.GKO` file.
- **Clean-up**: graphics outside the board (drawing titles, drill legends) and crop marks lying on the board edge are removed from copper and silkscreen.

If the `.WHL` file is missing, the old layers are listed with a note asking for it; they convert as soon as it is added. If a hole size cannot be found, that tool is drawn at 0.8 mm and the notice says so.

The conversion only affects what the viewer shows; your files are not changed. To send old EAGLE data to a fab, convert it to RS-274X with your CAD tool first.

### ODB++

Drop an ODB++ job as a `.tgz` / `.tar` / `.tar.gz`, a `.zip` or its folder. Files stored inside the job with Unix `compress` (`.Z`) are unpacked too. The job is converted in memory into RS-274X Gerbers, Excellon drill files and a pick-and-place file, which then behave like any other files (layer cards, 3D, fab check, export):

- **Layers** come from `matrix/matrix`: copper (top, bottom and inner, in stack order; negative plane layers are inverted), solder mask, silkscreen and paste, each on its side. Altium jobs that keep the silkscreen on a mechanical "legend" layer (e.g. `M1_TOP-LGD`) are recognised.
- **Board outline** from the step's `profile`, including arcs and cutouts.
- **Features**: lines, arcs, pads and filled surfaces with holes; standard symbols (round, square, rectangle, rounded and chamfered rectangle, oval, diamond, octagon, hexagon, triangle, ellipse, round and square donut, thermals) and user-defined symbols; pad rotation and mirroring; negative (clear) features; text from the job's stroke font.
- **Drills** from the drill layers, split into plated and non-plated by the layer name or the `.drill` attribute; slots are kept.
- **Nets and pins** from `eda/data`, so hovering and net highlighting work even though the job has no Gerber X2 attributes.
- **Components** from `comp_+_top` / `comp_+_bot`, with their real package size from the package outline. Unnamed footprints and one-pin "other" footprints (edge pads, fiducials, logos, mounting holes) are left out.
- The board step is picked automatically (a panel step with step-and-repeat is skipped). Layers that are not drawn (assembly, courtyard, documentation, drill drawings) are listed in one entry.

A notice after loading summarises the job: source tool, step, layer, hole and component counts and anything that needs checking.

### Excellon drill

- Metric and inch, leading and trailing zero formats, decimal coordinates
- Format hints from `;FILE_FORMAT=` comments and `METRIC,000.000`-style headers
- Tool definitions in the header or body
- Slots via `G85` and routed paths (`G00` / `M15` / `G01` / `M16`)
- Plated vs. non-plated detection from the file name (`NPTH`) or `TYPE=NON_PLATED` comments
- Drill data exported in Gerber format is also accepted. Drill *drawings* (symbol charts such as `.GDD`, `.GG1` or files named *DrillDrawing*) are recognised and not mistaken for drill data.

### Pick and place (centroid)

- KiCad `.pos`, Altium Pick Place (`.txt` / `.csv`, old and new formats), JLCPCB CPL `.csv`, EAGLE `.mnt` / `.mnb` and similar files
- Comma, semicolon, tab or space separated; units in mm, mil or inch (from the header or the values)
- Mirrored bottom-side X coordinates and a different placement origin are corrected automatically; the file row says when this happened

## Layer detection

Each file's layer is detected automatically, in this order:

1. **File content**: ODB++ jobs and old EAGLE RS-274D sets are recognised and converted first (see above), then Excellon headers (`M48`), then the Gerber X2 `FileFunction` attribute, then pick-and-place column headers.
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
| Inner copper | `In1_Cu`… | `.G1`, `.G2`… | `.ly2`, `.ly3`… |
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
- **Holes that cannot be cut**: holes that overlap each other, holes at a board corner or on a curved edge, and any beyond 4000, are drawn flat on the surface instead of cut through.
- **Duplicate holes**: a hole listed in two drill files (EasyEDA puts vias in both the PTH and the via file) is drawn once.
- **Binary drill files**: Altium's binary EIA `.DRL` cannot be read; use the ASCII NC drill file (`.TXT`) from the same export.
- **Texture resolution**: this is limited by your GPU (usually 4096 px across the longest board side). Very large boards show less fine detail. While you drag a slider or color on a large board, a quicker lower-resolution preview is shown and the sharp version follows when you stop.
- **Components are approximate**: parts are simple boxes sized from the package name, not real 3D models. Pin headers and connectors are drawn as their low plastic body.
- **STEP export** is a mechanical model: the board body and component boxes only, without copper, solder mask or silkscreen. Holes that are drawn flat instead of cut (see *Holes that cannot be cut*) are left out of the solid, and the notice after saving says how many. Plated half-holes are cut as notches but carry no plating. Arcs in the outline are written as short straight segments.
- **Nested zips**: a zip inside a zip is not opened (it is listed with a note). Extract it first. A `.tgz` inside a zip is opened.
- **ODB++**: blind and buried drill spans are drawn as through-holes, barcodes are skipped, and thermals with other than four spokes are drawn as plain rings. Components are boxes from the package outline, not 3D models.
- **Compare**: each layer is compared on a grid of up to 40 pixels per mm (25 µm), less on very large boards, so differences smaller than that are not seen. Only a pure offset between the two exports is corrected, not rotation or mirroring. Very different copper pours (e.g. a board re-poured in another tool) produce many small changes.
- **Legacy EAGLE RS-274D**: only EAGLE's default 2.3 inch coordinate format is supported, and the `.WHL` file is required. Bottom-layer and drill alignment is found by matching pads, so a set with very few through-hole pads may not line up. Check the notice after loading. Hole plating is inferred from whether the hole sits on a copper pad.

## Third-party libraries

These are loaded at runtime from public CDNs:

- [three.js](https://threejs.org/) r160, MIT License
- [JSZip](https://stuk.github.io/jszip/) 3.10.1, MIT License (dual-licensed MIT / GPLv3)

## License

Released under the MIT License. See [LICENSE](LICENSE).
