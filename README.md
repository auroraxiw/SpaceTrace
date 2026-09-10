# SpaceTrace — Spatial Analysis Tool

**SpaceTrace** is a browser-based spatial analysis tool for ICE (Isolated, Confined, and Extreme) environment research. It calibrates Space Syntax point clouds and CCTV tracking data onto floor plans, enabling replay, heatmap, proximity, and behavioural analysis without any installation.

---

## Files

| File | Description |
|------|-------------|
| `spacetrace-calibrate.html` | Step-by-step calibration wizard |
| `spacetrace-analyse.html` | Replay and analysis interface |

**Both files must be in the same folder.** Open `spacetrace-calibrate.html` to begin.

---

## Workflow Overview

```
spacetrace-calibrate.html  →  Open Analysis →  spacetrace-analyse.html
         ↑                                              |
         └──────────── ← Calibrate ───────────────────┘
```

---

## Part 1 — Calibrate

Open `spacetrace-calibrate.html` in any modern browser (Chrome, Safari, Edge).

### Step 1 — Floor Plan

Upload a PNG or JPG of your floor plan. This is the spatial reference for all calibration. Any resolution works.

### Step 2 — Space Syntax Calibration *(optional)*

Upload a depthmapX CSV containing `x`, `y`, and metric columns (e.g. Integration, Connectivity).

**To calibrate:**
1. Click a recognisable point on the **left panel** (SS point cloud) — e.g. a corner, doorway, or column
2. Click the matching point on the **right panel** (floor plan)
3. Repeat for **5–8 pairs**, spread across the full extent of the space
4. Click **Apply** — a blue overlay appears on the floor plan; check alignment
5. If the fit looks good, click **Looks good → Continue**

> **Tips:** Cover all four corners and the centre. The fit quality score (Excellent / Good / Fair / Poor) helps diagnose bad pairs. Click Re-do to start again.

**Preset:** Save your calibration as a JSON file for reuse. Import it next time to skip this step.

### Step 3 — Tracking Data Calibration *(optional)*

Upload a CCTV reference frame (screenshot) and your tracking CSV.

**Supported tracking CSV formats:**

| Columns present | How position is computed |
|-----------------|--------------------------|
| `floor_x`, `floor_y` | Used directly — no calibration needed |
| `cx_norm`, `cy_norm` | Normalised bounding box centre |
| `x1`, `y1`, `x2`, `y2` | Bottom-edge centre `((x1+x2)/2, y2)` — foot contact point |

**To calibrate:**
1. Click a recognisable point on the **left panel** (camera frame)
2. Click the matching point on the **right panel** (floor plan)
3. Repeat for **4+ pairs**
4. Click **Apply calibration**

**Preset:** Save and reload as JSON.

### Step 4 — Zones *(optional)*

Draw named polygons on the floor plan to tag areas (e.g. Workspace, Kitchen, Corridor). Each zone has a **Privacy** setting: Public / Shared / Private. Zone labels are embedded in the exported CSVs.

### Step 5 — Download & Open Analysis

Download the calibrated CSVs:
- `space_syntax_calibrated.csv`
- `tracking_calibrated.csv`

Click **Open Analysis →** to open the analysis interface with your data pre-loaded.

---

## Part 2 — Analyse

The analysis interface opens from calibrate, or can be opened directly with your calibrated CSVs.

### Uploading Data

In the **Data** panel (Step 1):

- **Tracking CSV** — Upload `tracking_calibrated.csv` (must contain `floor_x`, `floor_y`)
- **Floor Plan** — Upload the same floor plan image used in calibration
- **Space Syntax CSV** *(optional)* — Upload `space_syntax_calibrated.csv`

After loading, use **Filter by Person** / **Filter by Session** to narrow the dataset.

### Replay Controls

| Control | Function |
|---------|----------|
| ▶ / ⏸ | Play / Pause |
| ‹ › | Step one frame |
| ↺ | Reset to frame 1 |
| Timeline bar | Scrub to any frame |
| Speed selector | 0.5× · 1× · 2× · 5× · 10× · 50× · 100× · 500× |

### Display Layers (Step 3)

Toggle layers on/off:

- **Heatmap** — Cumulative presence density
- **Tracks** — Historical movement trails
- **Live dots** — Current frame positions
- **Floor plan** — Background image

### Space Analysis (Step 4)

Overlay Space Syntax metrics (Integration, Connectivity, Visual Depth, etc.) as a bivariate colour map on the floor plan. Select the metric from the dropdown and adjust opacity.

### Behavioural Analysis (Step 5)

Six analysis modules, each with configurable time window (Current / Past 10–500 frames / Custom):

| Module | What it shows |
|--------|---------------|
| **Heatmap** | Cumulative spatial density |
| **Raw Trajectory** | Full movement paths per person |
| **Proximity** | Interpersonal distances (Hall 1966 zones: Intimate / Personal / Social / Public) |
| **Visual Encounter** | Co-presence based on isovist overlap |
| **Congregation** | Spatial clustering over time |
| **Approach & Avoid** | Movement vectors toward/away from others |

### Video Validate *(optional)*

Click **▶ Video Validate** in the top bar to open a floating panel.

1. Click the panel to upload your CCTV video (MP4)
2. The video syncs frame-by-frame with the replay via the `time_sec` column
3. Bounding boxes and person IDs are drawn on the video
4. Use ‹ › buttons to step one frame at a time
5. Drag the panel anywhere on screen; resize from the corner

> The replay is always the master clock. The video seeks to match each frame's `time_sec` value.

---

## CSV Column Reference

### Tracking CSV (input to calibrate)

| Column | Required | Description |
|--------|----------|-------------|
| `person_id` | ✓ | Unique identifier per person |
| `time_sec` | ✓ | Timestamp in seconds (for replay sync) |
| `floor_x` / `floor_y` | or bbox | Normalised position [0–1] on floor plan |
| `x1`, `y1`, `x2`, `y2` | or above | Bounding box pixel coordinates |
| `cx_norm`, `cy_norm` | or above | Normalised bounding box centre |
| `day` | recommended | Day label (for multi-day datasets) |
| `session` | recommended | Session label |
| `camera` | optional | Camera ID (for multi-camera filtering) |
| `zone_type` | optional | Activity type (for filtering) |

### Space Syntax CSV (input to calibrate)

| Column | Required | Description |
|--------|----------|-------------|
| `x`, `y` | ✓ | depthmapX coordinates |
| Any metric | recommended | e.g. `Integration (HH)`, `Connectivity` |

---

## Browser Compatibility

| Browser | Support |
|---------|---------|
| Chrome / Edge | ✓ Recommended |
| Safari | ✓ Supported |
| Firefox | ✓ Supported |

No installation, no server, no internet connection required after download.

---

## Navigation

- **Calibrate → Analyse:** Click **Open Analysis →** in calibrate. The latest version of the analysis interface opens in a new tab with your data pre-loaded.
- **Analyse → Calibrate:** Click **← Calibrate** to return. If opened via calibrate, the tab closes and calibrate returns to focus.

---

## Credits

Built at the **NeuroCīvitās Lab**, University of Cambridge, as part of the **SPACE4SPACE** project investigating spatial configuration and crew behavioural health in ICE environments.

**Principal Investigator:** Dr. Michal Gath-Morad  
**Developer:** Aurora Xi Wang  
**Supervisors:** Prof. Koen Steemers, Dr. Davide Schaumann

---

*SpaceTrace v0.1 · Cambridge NeuroCīvitās Lab · 2026*
