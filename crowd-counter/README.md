# Crowd Counter AI — AiLECS Lab

AI-powered crowd density estimation and counting tool. Upload any crowd image and receive an instant density heatmap with an estimated headcount, powered by state-of-the-art deep learning models.

Built by the [AiLECS Lab](https://www.ailecs.org/) (AI for Law Enforcement and Community Safety), Monash University & AFP.

---

## Features

- **Instant Crowd Counting** — Upload a JPEG/PNG and get an estimated headcount in seconds
- **Density Heatmap** — Visualise where the crowd is densest with a colour-mapped overlay
- **Multiple Models** — Easily switch between DM-Count variants (NWPU, QNRF, ShanghaiTech A/B) and CrowdDiff
- **Interactive Comparison** — Side-by-side or overlay slider to compare original image with the density map
- **Plug-and-Play Models** — Drop new `.pth`/`.pt` model files into the models folder and they appear automatically
- **GPU Accelerated** — CUDA support for fast inference; CPU fallback available

## Supported Models

| Model | Type | Speed | Best For |
|-------|------|-------|----------|
| **macro exp2** | VGG19-based | Instant (~0.5s) | **Currently Active Model**, Best overall macro-avg performance |
| DM-Count (NWPU) | VGG19-based | Instant (~0.5s) | General-purpose |
| DM-Count (QNRF) | VGG19-based | Instant (~0.5s) | Diverse density scenes |
| DM-Count (SH-A) | VGG19-based | Instant (~0.5s) | Dense crowds |
| DM-Count (SH-B) | VGG19-based | Instant (~0.5s) | Sparse/surveillance views |
| CrowdDiff | Diffusion-based | Slow (~30s+) | Experimental, novel approach |

---

## Recent Experiments & Benchmarks

We have been running continuous experiments using larger combined datasets and different mixture rates to train a highly robust, general-purpose model. These experiments have achieved excellent results. 

Our current active model, **`exp2_macro_eval`** (`macro exp2`), significantly outperforms previous single-dataset baselines across diverse testing environments, yielding the best overall macro-average MAE (Mean Absolute Error).

| Checkpoint | SH-A test | SH-B test | QNRF Test | NWPU val | UCF-CC-50 | macro-avg |
|------------|-----------|-----------|-----------|----------|-----------|-----------|
| model_nwpu | 72.30 | 10.81 | 90.28 | **64.90** | 415.86 | 130.83 |
| model_qnrf | 70.04 | 13.86 | 97.24 | 100.54 | 393.36 | 135.01 |
| model_sh_A | 60.79 | 23.78 | 140.03 | 138.43 | 408.38 | 154.28 |
| model_sh_B | 142.34 | **7.70** | 205.22 | 136.21 | 1102.89 | 318.87 |
| dmcount_exp1_1951_baseline | 58.48 | 8.19 | 87.83 | 87.80 | 105.29 | 69.52 |
| exp2_ShanghaiTechA_eval | **58.69** | 7.99 | 89.17 | 78.18 | 55.84 | 57.97 |
| **exp2_macro_eval** | 62.02 | 8.11 | **87.39** | 78.57 | **28.49** | **52.92** |

> **Note:** We are actively collecting more datasets and further training is underway to continue improving model accuracy and robustness.

---

## Project Structure

```
crowd-counter/
├── start.bat              # One-click launcher (Windows)
├── README.md              # This file
├── backend/
│   ├── main.py            # FastAPI application
│   ├── pyproject.toml     # PDM dependencies
│   ├── pdm.lock           # Locked dependency versions
│   ├── .venv/             # Python virtual environment
│   ├── services/
│   │   └── inference.py   # Model loading & inference logic
│   └── models/
│       ├── dmcount/       # DM-Count weights (.pth)
│       └── crowddiff/     # CrowdDiff weights (.pt)
└── frontend/
    ├── index.html         # Main UI page
    ├── styles.css         # Dark theme with blue accents
    ├── script.js          # Client-side logic & particle effects
    └── assets/            # AiLECS logos and branding
```

---

## Quick Start (Existing Setup)

If the environment is already set up (`.venv` exists), you can start the application immediately:

### Option 1: Double-click

Double-click **`start.bat`** in the project root. A terminal window will open and the server will start.

### Option 2: Command line

```bash
cd crowd-counter\backend
pdm run start
```

Then open your browser at: **http://127.0.0.1:8080**

### Stopping the Server

Press **`Ctrl + C`** in the terminal window to stop the server.

---

## Fresh Setup on a New Laptop

Follow these steps when moving the application to a new machine.

### Prerequisites

- **Python 3.12.x** installed ([python.org](https://www.python.org/downloads/))
- **PDM** package manager installed ([pdm-project.org](https://pdm-project.org/en/latest/))
- **NVIDIA GPU** with CUDA drivers (optional but recommended for speed)

### Step 1 — Install PDM

```bash
pip install pdm
```

### Step 2 — Install Python Dependencies

```bash
cd crowd-counter\backend
pdm install
```

This creates the `.venv` folder and installs all non-PyTorch dependencies.

### Step 3 — Install PyTorch with CUDA

> [!IMPORTANT]
> PyTorch must be installed separately via pip to get the correct CUDA build.

**For RTX 4050 / RTX 5080 (CUDA 12.8):**

```bash
.venv\Scripts\python.exe -m pip install torch==2.8.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```

**For CPU-only (no NVIDIA GPU):**

```bash
.venv\Scripts\python.exe -m pip install torch==2.8.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```

### Step 4 — Verify Model Files

Ensure your model weight files are present in:
- `backend/models/dmcount/` — `.pth` files (e.g., `model_nwpu.pth`)
- `backend/models/crowddiff/` — `.pt` files (e.g., `model114000.pt`)

### Step 5 — Update `start.bat` (if needed)

Edit `start.bat` and update the PDM path if it's different on the new machine. Find your PDM path with:

```bash
where pdm
```

Replace the path in `start.bat` or, if `pdm` is on your PATH, simplify to:

```bat
cd /d "%~dp0backend"
pdm run start
```

### Step 6 — Start the Application

```bash
cd crowd-counter\backend
pdm run start
```

Open **http://127.0.0.1:8080** in your browser.

---

## Adding New Models

When you train a new model, adding it to the application requires **zero code changes**:

1. **DM-Count models** → Copy the `.pth` file into `backend/models/dmcount/`
2. **CrowdDiff models** → Copy the `.pt` file into `backend/models/crowddiff/`
3. **Restart the server** → The new model appears in the dropdown automatically

---

## Development Mode

For live-reload during development (auto-restarts on file changes):

```bash
cd crowd-counter\backend
pdm run dev
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3.12, FastAPI, Uvicorn |
| Frontend | Vanilla HTML/CSS/JS (no build step) |
| ML Framework | PyTorch 2.8.0 + CUDA 12.8 |
| Models | DM-Count (VGG19), CrowdDiff (Diffusion) |
| Package Manager | PDM |

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Port 8080 already in use | Change port: `pdm run uvicorn main:app --port 9090` |
| `No module named pip` in venv | Run `.venv\Scripts\python.exe -m ensurepip` first |
| CUDA not detected | Verify `nvidia-smi` works, reinstall PyTorch with correct CUDA version |
| Models not appearing in dropdown | Check model files exist in `backend/models/dmcount/` or `crowddiff/` |
| Page loads but shows "Connection error" | Ensure the backend server is running on the same port |
