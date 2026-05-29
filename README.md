# CCTV Spacetrace
https://auroraxiw.github.io/cctv-space-analysis

A suite of browser-based tools for mapping CCTV person-tracking data and isovist/Space Syntax analysis results onto architectural floor plans — no server, no installation, no dependencies. Everything runs entirely in your browser from a single HTML file.

Built for spatial behaviour research in ICE (Isolated, Confined, and Extreme) environments at the [Cambridge Cognitive Architecture Lab / NeuroCivitas Lab](https://www.neurocivitas.com), University of Cambridge.

Compatible with [YOLOv11 + BoT-SORT + ReID](https://github.com/ultralytics/ultralytics) tracking pipeline output and [DepthMap / depthmapX](https://www.spacesyntax.net/software/) isovist analysis output.

---

## Tools

### 1. `index.html` — Spatial Analysis Studio ⭐ (main tool)

The integrated tool combining all features:

- **Space Analysis layer** — visualise any isovist/Space Syntax metric (Integration, Choice, Vista, etc.) as a colour-mapped point cloud on the floor plan
- **CCTV replay layer** — replay YOLOv11 person tracking data as live dots, trajectory trails, and accumulating heatmap
- **Independent calibration** for both data sources — affine transform (least-squares, no deformation) using point-pair matching
- **Zone drawing** with `Private / Semi-public / Public` type labels and live dwell-time statistics
- **Zone type filter** — show/hide zones by type during replay
- **Camera panel** — displays reference frame or video at natural aspect ratio with BBox overlay
- **Playback speeds** from 0.25× to 500×

#### Export options
| Export | Contents |
|--------|----------|
| Analysis CSV | Original + `floor_x`, `floor_y`, `zone`, `zone_type` |
| CCTV CSV | Original + `floor_x`, `floor_y`, `zone`, `zone_type` |
| **Merged CSV** | Every CCTV row + nearest analysis point's full metric values (Area, Vista, Integration HH, etc.) |
| PNG | Floor plan with all visible layers composited |

---

### 2. `cctv_floor_mapper.html` — CCTV Floor Mapper

Standalone CCTV-only tool for importing YOLOv11 tracking CSV and mapping it onto a floor plan.

**7-step wizard:**
1. Import CSV
2. Upload reference frame or video
3. Upload floor plan
4. Homography calibration (IDW, click point pairs)
5. Draw zones
6. Replay
7. Export

**Features:** heatmap, trajectory trails, live person dots, dwell-time statistics, person ID labelling, multi-camera support, CSV re-export.

---

### 3. `space_analysis_mapper.html` — Space Analysis Mapper

Standalone tool for isovist/Space Syntax CSV visualisation.

**5-step wizard:**
1. Load analysis CSV
2. Upload floor plan
3. Calibrate (affine transform, point-pair matching on point cloud preview)
4. Draw zones
5. Export

**Features:** 4 colour maps, adjustable point size and opacity, zone polygon drawing with delete/undo, zone dwell assignment, CSV export with `floor_x/y/zone` columns.

---

## Input CSV Formats

### YOLOv11 + BoT-SORT tracking CSV (for CCTV tools)

| Column | Description |
|--------|-------------|
| `frame` | Frame number |
| `time_sec` | Seconds from video start |
| `datetime` | ISO datetime string |
| `camera` | Camera ID e.g. `C1` |
| `track_id` | Raw tracker ID |
| `person_id` | Resolved person ID e.g. `AA01` |
| `x1`, `y1`, `x2`, `y2` | Bounding box in original image pixels |
| `cx_norm`, `cy_norm` | Normalised centre point [0–1] |
| `confidence` | Detection confidence |

### Space Syntax / isovist analysis CSV

| Column | Description |
|--------|-------------|
| `x`, `y` | Real-world coordinates (any unit) |
| `Area`, `Vista`, `Integration (HH)`, … | Analysis metric columns (any names) |

The tool auto-detects all metric columns beyond `x`, `y`, and `ref`.

---

## Calibration Method

Both tools use **affine transform** least-squares fitting:

```
floor_x = a·src_x + b·src_y + tx
floor_y = c·src_x + d·src_y + ty
```

- 6 parameters: independent X/Y scale, rotation, translation
- **No deformation** — parallel lines stay parallel
- Solved by closed-form least squares from ≥3 point pairs
- Status display shows `scaleX`, `scaleY`, `rotation`, mean reprojection error

This differs from the older CCTV Floor Mapper which uses IDW interpolation (local, non-rigid).

---

## File Structure

```
spatial-analysis-studio/
├── index.html                    ← Integrated studio (main tool)
├── cctv_floor_mapper.html        ← CCTV-only tool
├── space_analysis_mapper.html    ← Space analysis visualiser
├── README.md
├── LICENSE
├── .gitignore
└── example/
    ├── sample_tracking_format.csv     ← Example YOLOv11 CSV format
    └── sample_analysis_format.csv     ← Example space analysis CSV format
```

---

## Browser Compatibility

Tested in Chrome 120+ and Firefox 121+. Requires Canvas API and File API. No WebGL, no external CDN, no cookies, no server.

> Large CSV files (>25 MB) are parsed in chunks on the main thread — progress bar shown. The browser UI remains responsive throughout.

---

## Camera Labels (default, CCTV Mapper)

| ID | Space |
|----|-------|
| C1 | Atrium |
| C2 | Junction |
| C3 | Kitchen |
| C4 | Operations |
| C5 | Mech WS |
| C6 | Elec WS |
| C7 | Bio Lab |

Edit the `CAM_LABELS` constant in `cctv_floor_mapper.html` to match your site.

---

## Research Context

Developed for doctoral research on **spatial configuration and human behaviour in confinement environments**, Cambridge Cognitive Architecture Lab / NeuroCivitas Lab, University of Cambridge. Data collected at [Lunares Research Station](https://lunares.space), Poland (LunAres M1 mission, 7 participants, 14 days, 7 cameras).

---

## GitHub Pages

To deploy as a live web tool (no download required for users):

1. Push this repo to GitHub
2. Settings → Pages → Deploy from branch `main`, folder `/`
3. Access at `https://YOUR_USERNAME.github.io/spatial-analysis-studio`

---

## License

MIT License — see `LICENSE`.
