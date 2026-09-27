# CrowdLens — Crowd Density Analyzer

A local web app that detects and visualizes crowd density in images and videos using **YOLOv8** (with HOG fallback).

---

## 📁 Project Structure

```
crowdlens/
├── app.py                  ← Flask backend (YOLO + OpenCV)
├── requirements.txt        ← Python dependencies
├── templates/
│   └── index.html          ← Frontend HTML
├── static/
│   ├── css/style.css       ← Dark surveillance UI styles
│   └── js/app.js           ← Frontend logic (FIXED)
├── uploads/                ← Temp upload dir (auto-created)
└── outputs/                ← Output dir (auto-created)
```

---

## 🚀 Quick Setup

### 1. Python environment (recommended: Python 3.10+)

```bash
cd crowdlens
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

> **Note:** `opencv-python-headless` is used (no GUI needed — runs in Flask).  
> On first run, YOLOv8n (~6MB) downloads automatically from Ultralytics.

### 3. Run the app

```bash
python app.py
```

Open your browser at: **http://127.0.0.1:5000**

---

## 🖥️ How to Use

### Image Analysis
1. **Drop or select** a PNG/JPG image in the left panel
2. Adjust **Grid Rows / Columns** (default 10×10)
3. Click **RUN ANALYSIS**
4. Switch between **Original / Grid+Boxes / Heatmap** view tabs
5. Read the **density gauge**, **people count**, **hotspot zones**, and **spatial grid**

### Video Analysis
1. **Drop or select** a video file (MP4, AVI, MOV — up to 500MB)
2. Adjust the **Sample Every N Frames** slider (lower = more accurate, slower)
3. Click **RUN ANALYSIS**
4. See the **timeline chart** of crowd count over time + aggregate heatmap

---

## 🔧 What Was Fixed

| Problem | Fix |
|---|---|
| `app.js` was from a completely different project (CrowdWatch live dashboard) with WebSockets, login, zone maps — none of which CrowdLens needs | Rewrote `app.js` entirely for CrowdLens |
| Missing `runAnalysis()` function | Implemented with fetch + FormData + progress feedback |
| Missing `setView()` function for image tabs | Implemented with base64 image switching |
| Missing gauge animation | Canvas arc animation with color-coded levels |
| Missing `renderGrid()`, `renderHotspots()`, `renderTimeline()` | All implemented |
| No drag-and-drop handling | Full drag/drop with visual feedback |
| Slider values not updating | Added input event listeners |
| No status LED / progress bar logic | Fully wired up |

---

## ⚙️ Configuration

| Setting | Default | Description |
|---|---|---|
| Grid Rows | 10 | Horizontal grid divisions |
| Grid Cols | 10 | Vertical grid divisions |
| Sample Rate (video) | 30 | Analyze every Nth frame |
| Max file size | 500MB | Configurable in app.py |

---

## 🐛 Troubleshooting

**`ModuleNotFoundError: ultralytics`** → Run `pip install ultralytics`

**YOLO download fails** → The app falls back to OpenCV's HOG detector automatically

**`cv2` not found** → Run `pip install opencv-python-headless`

**Port 5000 in use** → Change `port=5000` to another port in `app.py`

**Large videos time out** → Increase `sample_rate` slider (analyze fewer frames)
