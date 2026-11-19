# Floorplan Editor

**Live demo: https://o3ll.github.io/floorplan-editor/**

A single-file, in-browser floor plan editor that exports 3D-printable STL models. No build step, no server: open `index.html`.

The interface is in French; all dimensions are in centimetres, measured from the inside of the room.

## Features

- **2D plan**: draw walls (snapped to existing corners, automatic right angles), edit length, thickness and height numerically or by dragging corner handles. Wall lines are the interior faces; thickness is added outwards, so displayed dimensions are the ones you measured.
- **Openings**: windows, doors (hinge side and swing direction) and full-height openings, positioned along a wall.
- **Rooms**: traced automatically from a closed loop of walls, or point by point; area in m².
- **Furniture**: rectangular blocks and L-shaped corner sofas, magnetic snapping to walls (hold Ctrl to disable), `R` to rotate by 90° (`Shift+R`: 15°).
- **3D view**: orbit, zoom, pan (three.js).
- **Export**: binary STL already scaled (1:50 by default) with slab, walls cut at openings and optional furniture; PNG of the plan; JSON save/load. Work is autosaved in the browser's local storage.

## Usage

Open `index.html` in a browser, or serve the folder with any static server (GitHub Pages works as is). The 3D view and zipped STL export load three.js r128 and JSZip 3.10.1 from cdnjs, so they need a network connection; everything else works offline.

`examples/simple-room.json` is a sample plan to load with the "Charger JSON" (Load JSON) button.

## Shortcuts

| Key | Action |
| --- | --- |
| `S` / `M` / `P` / `Space` | Select / Wall / Room / Pan |
| `R`, `Shift+R` | Rotate selected furniture by 90° / 15° |
| `Ctrl` while dragging | Disable wall snapping |
| `Delete` | Delete selection |
| `Ctrl+Z`, `Ctrl+Y` | Undo / Redo |
| `Esc` | Cancel current drawing, clear selection |

## Plan format

A plan is one JSON object: `walls` (`x1,y1,x2,y2`, thickness `t`, height `h`, `side` = which side the thickness goes), `openings` (attached to a wall by id, `pos` and `w` along it, `z0`/`z1` heights, `type` among `fenetre` (window), `porte` (door), `ouverture` (opening)), `rooms` (polygon `pts`) and `furn` (centre `x,y`, `w`, `d`, `h`, `rot` in degrees; `shape: "L"` with `w2`, `d2`, `corner` for corner sofas). The y axis points down, as on screen.

## Limitations

- Walls and slab overlap slightly in the STL instead of being a single fused solid; slicers merge them.
- A room polygon only follows a wall if its vertices coincide with the wall corners; after moving walls, recreate it with "Pièce auto" (Auto room).
- Furniture is exported as extruded blocks.

## License

MIT
