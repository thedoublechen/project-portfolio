# GeoLocation Training (Standalone)

Self-contained extraction of **`finetuner-ai-suite`** geolocation dashboards:

| Source (finetuner) | Here |
|--------------------|------|
| `pages/20_Retrain_Dashboard_Ori.py` | `geolocation_training/app/pages/1_StreetCLIP_Training.py` |
| `pages/20_Retrain_dashboard_AdvRegression.py` (20 Sep 2025 PM — **later** same day) | `3_Regression_Spherical_Archived.py` |
| `pages/20_Retrain_dashboard_Regression.py` (20 Sep 2025 AM — earlier) | `4_Regression_Classic_Archived.py` |
| `scripts/train_geoloca_model_ori.py` | `scripts/train_geoloca_model_ori.py` (copied) |

| (New standalone tool) | `geolocation_training/app/pages/2_Dataset_Stocktake.py` |
| (New standalone tool) | `geolocation_training/app/pages/5_ViT_Training.py` |

**Regression:** spherical (page 3) supersedes classic (page 4) within those experiments (same calendar day rewrite). **Production** geo fine-tuning is StreetCLIP staged training (page 1).

**Docker:** GPU image and compose — see [`DOCKER.md`](./DOCKER.md).

**ViT Trainer:** implemented — `geolocation_training/app/pages/5_ViT_Training.py`.

---

## Run locally with PDM (recommended)

Prerequisites:

- Python **3.11** or **3.12** (matches `requires-python`).
- [PDM](https://pdm-project.org/) installed globally (see below if `pip` / `pdm` are “not recognized”).
- NVIDIA GPU optional but expected for training; wheels below target **CUDA 12.8** (`cu128`).

### If `pip` or `pdm` is “not recognized” (Windows)

PowerShell often **does not** put `pip` on `PATH`; use **`py -m`** (Python Launcher) or **`python -m`** instead:

```powershell
# Pick one that prints a version (3.11 or 3.12)
py --version
# or: python --version

# Install PDM into your *user* site (no need for `pip` on PATH)
py -m pip install --user pdm

# Add PDM’s script folder to PATH for *this* session (adjust if `where pdm` differs)
$env:Path = "$env:APPDATA\Python\Python311\Scripts;$env:Path"   # or Python312\Scripts

# Now this should work:
pdm --version
```

Safer long-term: install PDM with **pipx** (isolated tool env): `py -m pip install --user pipx` then `pipx install pdm` — then use the path `pipx` prints for `pdm`.

**Wrong command:** `pdm requires 3.11` — PDM has no such subcommand.  
**Right command:** `pdm use 3.11` (or `pdm use 3.12`) to pin the interpreter for this project.

**Conda env** `(geolocation-training)`: if `pip` is missing, run `conda install pip` (or `python -m ensurepip`) in that env, *or* install PDM from a **base** Python with `py -m pip install --user pdm` and run `pdm` from the project folder (PDM will create `.venv` for the project anyway).

From the repo root **`geolocation-training/`**:

```powershell
cd C:\Users\chen\Python_Projects\geolocation-training

# Pin interpreter (once)
pdm use 3.11

# Resolve + install into .venv (creates lock file on first run)
pdm install

# If you add or change deps later:
pdm lock
pdm sync
```

`pdm install` installs **`torch` / `torchvision` / `torchaudio` from PyTorch’s `cu128` index** via `[[tool.pdm.source]]` in `pyproject.toml`, and everything else from PyPI — same CUDA line as Docker and AiLECS audio images.

### Environment variables (local paths)

Training scripts resolve the project root via **`GEOTRAIN_PROJECT_ROOT`** (defaults to **`Path.cwd()`**). Dataset defaults in the UI use **`GEOTRAIN_DATA_ROOT`**.

Example (PowerShell):

```powershell
cd C:\Users\chen\Python_Projects\geolocation-training
$env:GEOTRAIN_PROJECT_ROOT = (Get-Location).Path
$env:GEOTRAIN_DATA_ROOT = "F:/datasets/geolocation"
```

(Optional) Streamlit port:

```powershell
$env:GEOTRAIN_STREAMLIT_PORT = "8620"
```

### Start the UI (three equivalent options)

All must be run with **`cwd` = project root** so `scripts/` and `models/` paths match the finetuner layout.

```powershell
# Option A — entry script (recommended)
pdm run geolocation-train

# Option B — PDM-named script from pyproject.toml
pdm run streamlit-ui

# Option C — call Streamlit on the multipage Home (same entry as A/B)
pdm run python -m streamlit run geolocation_training/app/Home.py --server.port 8620
```

Direct **`.venv`** (after `pdm install`):

```powershell
.\.venv\Scripts\python.exe -m geolocation_training.app.main
```

or:

```powershell
.\.venv\Scripts\python.exe -m streamlit run geolocation_training\app\Home.py --server.port 8620
```

Open **http://127.0.0.1:8620** (or whatever you set).

### Smoke test (project root paths)

Quick check **without** training:

```powershell
cd C:\Users\chen\Python_Projects\geolocation-training
$env:GEOTRAIN_PROJECT_ROOT = (Get-Location).Path
pdm run python -c "from geolocation_training.paths import get_project_root; r=get_project_root(); print('root=', r); print('trainer exists=', (r/'scripts'/'train_geoloca_model_ori.py').is_file())"
```

You should see `trainer exists= True`.

If training fails on first launch, verify **base weights** live under **`models\streetclip_base_model`** (or the path you type in the form) — same expectation as **`finetuner-ai-suite`**.

### Lock file

After changing dependencies, run **`pdm lock`** and commit **`pdm.lock`** if you want identical installs across machines.

### CPU-only or different CUDA

The default **`pytorch-cu128`** source pulls GPU builds. For **CPU-only** dev, temporarily remove or change `[[tool.pdm.source]]` in `pyproject.toml` / torch pins, run `pdm lock` again, and install from PyPI defaults — or maintain a separate optional dependency group (`pdm add -dG cpu torch`) if you need both profiles.

---

## Run locally without PDM (`venv` + `pip`)

Same env vars as above:

```powershell
cd C:\Users\chen\Python_Projects\geolocation-training
python -m venv .venv
.\.venv\Scripts\activate
pip install -U pip
pip install torch==2.8.0 torchvision==0.23.0 torchaudio==2.8.0 --index-url https://download.pytorch.org/whl/cu128
pip install -e .
geolocation-train
```

(`pip install -e .` does not attach the cu128 index automatically unless you duplicate the installs as shown.)

---

## Docker

GPU container bind-mounts your dataset — see **`DOCKER.md`**.

Typical compose default: host **`F:\datasets\geolocation`** → container **`/data/geolocation`**; in the Streamlit UI use **`/data/geolocation/...`** paths inside Docker.
