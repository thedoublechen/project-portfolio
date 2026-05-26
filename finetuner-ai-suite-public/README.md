# AiLECS Lab Testbed (Finetuner AI Suite)

The **AiLECS Lab Testbed** is a comprehensive, local-first demonstration suite of custom-trained and fine-tuned AI models. It serves as an experimentation environment for cutting-edge AI capabilities across computer vision, audio processing, and generative AI.

The suite is built with a Streamlit interface and leverages advanced models fine-tuned to solve specific challenges such as regional geographic limitations and audio source separation.

## Key Projects & Applications

Built into the `pages/` directory, these core functionalities drive the suite:

### 1. Metior Loca (Geolocation with GEO Reasoning)
An enhanced geolocation system designed to combat the limitations of open-source models (like StreetCLIP) in non-Northern Hemisphere locations. 
- **Features:** A multi-agent framework leveraging LoRA-adapted FastVLMs and OCR reasoning. It performs hierarchical predictions (Country → State → City → District) and achieves up to 85% accuracy in well-referenced Australian cities compared to the 11% baseline of vanilla models.
- **Path:** `pages/2_Metior_Loca_Hybrid_TestBed_LoRa_UI.py`

### 2. AI Audio Separator & Target Speaker Extraction (TSE AI)
A powerful audio separation and enhancement tool designed for isolating voices or denoising environments.
- **Features:** Integrates `speechbrain` separation models (WHAMR, Libri2Mix) alongside advanced Multi-pass Target Speaker Extraction (TSE). Through custom Fine-tuned TSE V2 and TF-GridNet integrations, the system can lift a specific target speaker's voice out of heavily noisy or overlapping audio using a brief enrollment sample.
- **Path:** `pages/3_Audio_Separator.py`

### 3. Smart Video Search
An intelligent CCTV / video footage indexing and search engine using Vision-Language Models.
- **Features:** Combines YOLOv8 object detection with CLIP embeddings to extract keyframes and detect persons or vehicles. It allows investigators to search hours of video using natural language (e.g., "person wearing red jacket"). Also uses FastVLM models to generate chronologically intelligent, textual scene summaries.
- **Path:** `pages/3_Smart_Video_Seaarch.py`

### 4. Lyrebird
A robust voice cloning environment coupled with AI transcription capabilities.
- **Features:** Supports both cloud APIs and local secure workflows for realistic voice generation from short reference audio samples.
- **Path:** `pages/3_Lyrebird.py`

### 5. Deepfake Audio Analyzer
A forensic audio authenticity tool focused on ENF (Electric Network Frequency) spectrogram analysis.
- **Features:** Compares ENF traces using STFT-based analysis to help distinguish real microphone recordings from synthetic or manipulated audio.
- **Path:** `pages/3_Deepfake_Audio_Analyzer.py`

### 6. Crowd Counter
An AI-powered crowd density estimation and counting tool for images and videos.
- **Features:** Includes DM-Count (recommended) and CrowdDiff (experimental) model options for different crowd-scene conditions. Includes video processing support and a training tool for custom models.
- **Paths:** 
  - `pages/4_Crowd_Counter.py` (Image)
  - `pages/4_Crowd_Video_Counter.py` (Video)
  - `pages/52_Crowd_Count_Training.py` (Training)

### 7. Additional Demonstration Tools
The suite also includes several other powerful proof-of-concept features:
- **Real-Time Threat Detection (`pages/4_Live_Threat_Detection.py`):** A dual-AI system combining FastVLM for contextual understanding and YOLO for object detection (weapons, phones) operating on live camera feeds.
- **Object Detection (`pages/5_Live_Object_Detection.py`):** Live object detection pipeline for visual scene monitoring workflows.
- **OCR Text Extraction (`pages/13_Scan_To_Text.py`):** Fast text extraction from images using EasyOCR with paragraph grouping and translation logic.
- **Local Image Search (`pages/17_Image_Search.py`):** CLIP-powered natural language search querying directly against a local directory of images.
- **Audio Enhancer (`pages/5_Audio_Enhancer.py`):** AI-powered voice enhancement for denoising and clarity improvement.
- **Dynamic Image Classifier (`pages/6_Dynamic_Classifier.py`):** Flexible image classification interface for rapid model evaluation.
- **Geo Check Explainer (`pages/6_Geo_Explainer.py`):** Explainable geolocation support and diagnostic visualization workflows.
- **Geolocation Sorter (`pages/7_Geolocation_Sorter.py`):** StreetCLIP-assisted geolocation organization and analysis utility.
- **Anomaly Spotting (`pages/10_Anomaly_Spotting.py`):** Interactive anomaly discovery interface for investigative image review.
- **Ground Truth Lookup (`pages/48_GT_Lookup.py`):** Crowd-counting dataset verification helper for GT label checks.
- **Audio Separation Demo (`pages/50_Audio_Separation_Demo.py`):** Lightweight demo for source separation behavior inspection.
- **Evaluation Dashboard (`pages/49_evaluation_dashboard_hierarchical.py`):** Hierarchical geolocation evaluation and performance dashboard.
- **Idea Portal & Project Catalog (`pages/999_Ideas_Portal.py`, `pages/998_Project_Catalogue.py`):** Built-in team tracking tools for monitoring AI development status and brainstorming implementations.

## Environment & Setup Instructions

> [!IMPORTANT]
> **Shared Virtual Environment:** This public demonstration repo securely utilizes the virtual environment (`.venv`) from the main, non-public `finetuner-ai-suite` project.

> [!WARNING]
> **RTX 5080 Hardware Compatibility:** For deployments utilizing an RTX 5080, standard PyTorch installations may face initialization or CUDA compatibility issues.

To reproduce the environment:

1. Ensure Python 3.10 to <3.13 is installed.
2. Link or create your virtual environment referencing the core non-public repository's `.venv`.
3. If setting up from scratch, first initialize dependencies excluding PyTorch:
   ```bash
   pdm install
   ```
4. **Install PyTorch:** Crucially, to ensure compatibility with the RTX 5080 architecture, you must install the specific `cu128` Torch `2.8.0` wheels by running:
   ```bash
   pip install torch==2.8.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
   ```
5. **Start the application:**
   ```bash
   pdm run start
   ```
   *or*
   ```bash
   streamlit run 1_Home.py
   ```

*Note: Models such as YOLO weights (.pt), CLIP models, and FastVLM checkpoints run locally and need to be present in their respective checkpoint / model directories as detailed in the source files.*
