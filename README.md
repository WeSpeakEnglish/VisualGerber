# Gerber 3D Viewer

A single-file, browser-based 3D viewer for PCB Gerber and Excellon drill files.
Open `gerber-3d-viewer.html` in a browser, drop in your fabrication files (loose or zipped),
and inspect the board in 3D with solder mask, surface finish, silkscreen, outline and drilled holes.

No installation, no build step and no server. Files are processed locally in your browser and are never uploaded.

## Quick start

1. Open `gerber-3d-viewer.html` in Chrome, Edge or Firefox (a recent version with WebGL).
2. Click **Open files…** or drag files onto the page. You can drop:
   - individual Gerber and drill files,
   - a whole folder,
   - one or more `.zip` archives (subfolders inside the zip are fine).
3. Orbit, pan and zoom the board. Use the toolbar for preset views.

> The page loads three.js and JSZip from public CDNs, so an internet connection is needed when it opens.
> After that, all processing happens offline in the browser.

## Features

- **Real 3D board**: the shape comes from the outline layer, including internal cutouts and notches. Plated and non-plated holes are cut through the board, and plated holes get metal barrels.
- **Layers rendered**: top and bottom copper, solder mask, silkscreen and outline, plus drill data. Paste and inner copper layers are recognised and listed but not drawn.
- **Appearance**: solder mask color (green, red, blue, black, white, purple, yellow), surface finish (ENIG, HASL, bare copper), silkscreen color and board thickness (0.6 to 2.4 mm).
- **Layer toggles**: show or hide mask, silkscreen, copper and holes.
- **Views**: isometric, top, bottom, flip board, and export the current view as PNG.
- **Board info**: dimensions, outline status, hole count and thickness.

## Supported input

### Gerber (RS-274X / X2)

- Coordinate format (`FS`), units (`MO`), including legacy `G70`/`G71`
- Standard apertures: circle, rectangle, obround, polygon (with holes)
- Aperture macros: circle, vector line, center line, lower-left line, outline, polygon, moiré, thermal; variables and arithmetic expressions
- Linear and circular interpolation (`G01`, `G02`, `G03`), single and multi-quadrant (`G74`, `G75`)
- Regions (`G36`/`G37`), dark and clear polarity (`LPD`/`LPC`)
- Step-and-repeat (`SR`)
- X2 file attributes (`%TF.FileFunction`) for automatic layer detection

Not supported: block apertures (`AB`), and mirroring, rotation or scaling of objects (`LM`, `LR`, `LS`).

### Excellon drill

- Metric and inch, leading and trailing zero formats, decimal coordinates
- Format hints from `;FILE_FORMAT=` comments and `METRIC,000.000`-style headers
- Tool definitions in the header or body
- Slots via `G85` and routed paths (`G00` / `M15` / `G01` / `M16`)
- Plated vs. non-plated detection from the file name (`NPTH`) or `TYPE=NON_PLATED` comments
- Drill data exported in Gerber format is also accepted

## Layer detection

Each file's layer is detected automatically, in this order:

1. **File content**: Excellon headers (`M48`), then the Gerber X2 `FileFunction` attribute.
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
| Drill | `.drl` | `.TXT`, `.DRL` | `.drd`, `.xln` |

If a layer is detected incorrectly, change it with the dropdown next to the file name. Set a file to **Ignore** to exclude it.

Non-Gerber files inside a zip (PDF, images, `.gbrjob`, reports, etc.) are skipped automatically.

## Controls

| Action | Mouse | Touch |
|---|---|---|
| Orbit | Left-drag | One-finger drag |
| Pan | Right-drag or Shift + drag | Two-finger drag |
| Zoom | Scroll wheel | Pinch |

Toolbar buttons: **Iso**, **Top**, **Bottom**, **Flip**, **PNG** (save a screenshot).

## Limitations

- **Missing outline**: without an outline layer, the board is drawn as the bounding box of the other layers plus a 1 mm margin.
- **Holes that cannot be cut**: holes that overlap each other or the board edge, and any beyond 4000, are drawn flat on the surface instead of cut through.
- **Texture resolution**: this is limited by your GPU (usually 4096 px across the longest board side). Very large boards show less fine detail.
- **No components**: this is a bare-board viewer. Components and 3D models are not shown.
- **Nested zips**: a zip inside a zip is not opened. Extract it first.

## Third-party libraries

These are loaded at runtime from public CDNs:

- [three.js](https://threejs.org/) r160, MIT License
- [JSZip](https://stuk.github.io/jszip/) 3.10.1, MIT License (dual-licensed MIT / GPLv3)

## License

Released under the MIT License. See [LICENSE](LICENSE).
