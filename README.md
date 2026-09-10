# SpaceTrace

A browser-based tool for spatial analysis of human movement in confined environments. No installation. No server. Open the HTML file and go.

---

## What it does

SpaceTrace takes CCTV tracking data and Space Syntax output, maps them onto a floor plan, and lets you replay movement, visualise density, and run proximity and behavioural analysis.

---

## Quick start

1. Download the repository
2. Open `spacetrace-calibrate.html` in Chrome or Safari
3. Follow the 5-step calibration wizard
4. Click **Open Analysis →** when done

Both files must be in the same folder.

---

## Calibrate

### Step 1 — Floor plan

Upload a PNG or JPG of your floor plan. Any resolution.

### Step 2 — Space Syntax *(optional)*

Upload a depthmapX CSV with `x`, `y`, and metric columns (Integration, Connectivity, etc.).

Click matching point pairs between the SS point cloud (left) and the floor plan (right). Use corners, doorways, and columns. Aim for 5–8 pairs spread across the full space. Click **Apply** and check the blue overlay alignment.

Save your calibration as a JSON preset to reuse later.

### Step 3 — Tracking data *(optional)*

Upload a CCTV reference frame and your tracking CSV.

Accepted column formats:

| Format | Columns |
|--------|---------|
| Pre-calibrated | `floor_x`, `floor_y` |
| Normalised centre | `cx_norm`, `cy_norm` |
| Bounding box | `x1`, `y1`, `x2`, `y2` |

For bounding box input, SpaceTrace uses the bottom-edge centre `((x1+x2)/2, y2)` as the floor contact point.

Click 4+ point pairs between the camera frame and floor plan. Click **Apply**.

### Step 4 — Zones *(optional)*

Draw named polygons on the floor plan. Assign each zone a privacy level: Public, Shared, or Private. Zone labels are written into the exported CSV.

### Step 5 — Download

Download the calibrated CSVs:
- `space_syntax_calibrated.csv`
- `tracking_calibrated.csv`

Click **Open Analysis →** — your data loads automatically.

---

## Analyse

### Loading data

Upload files in the **Data** panel:
- **Tracking CSV** — must contain `floor_x`, `floor_y`, `person_id`, `time_sec`
- **Floor plan** — same image used in calibration
- **Space Syntax CSV** *(optional)*

### Replay

| Control | Action |
|---------|--------|
| ▶ / ⏸ | Play / Pause |
| ‹ › | Step one frame |
| ↺ | Reset |
| Timeline | Scrub |
| Speed | 0.5× to 500× |

### Display layers

Toggle in the **Display** panel: Heatmap · Tracks · Live dots · Floor plan

### Space Analysis

Overlay Space Syntax metrics as a colour map. Select a metric and adjust opacity.

### Behavioural Analysis

| Module | What it shows |
|--------|---------------|
| Heatmap | Cumulative density |
| Raw Trajectory | Movement paths |
| Proximity | Interpersonal distances (Hall 1966 zones) |
| Visual Encounter | Co-presence by isovist overlap |
| Congregation | Spatial clustering |
| Approach & Avoid | Movement vectors toward/away from others |

Each module has a configurable time window: Current · Past 10–500 frames · Custom.

### Video Validate

Click **▶ Video Validate** to open a floating panel. Upload an MP4. The video syncs frame-by-frame with the replay using the `time_sec` column. Bounding boxes and person IDs are drawn on the video. Step through frames with ‹ ›. Drag and resize the panel freely.

---

## Tracking CSV columns

| Column | Required | Notes |
|--------|----------|-------|
| `person_id` | ✓ | |
| `time_sec` | ✓ | Seconds, used for replay and video sync |
| `floor_x`, `floor_y` | ✓ | Normalised [0–1] position on floor plan |
| `day` | recommended | For multi-day datasets |
| `session` | recommended | |
| `camera` | optional | For multi-camera filtering |

---

## Navigation

- **Calibrate → Analyse** — Click **Open Analysis →**. The latest analysis interface opens in a new tab with data pre-loaded.
- **Analyse → Calibrate** — Click **← Calibrate** to go back.

---

## Browser support

Chrome, Safari, Edge. Firefox works but is slower on large datasets.

---

## Project

Built at the [NeuroCīvitās Lab](https://www.neurocivitas.com), University of Cambridge.  
SPACE4SPACE project — spatial configuration and crew behavioural health in ICE environments.

PI: Dr. Michal Gath-Morad · Developer: Aurora Xi Wang · Supervisors: Prof. Koen Steemers, Dr. Davide Schaumann
