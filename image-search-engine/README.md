# Image & Video Search Engine

A Streamlit-based application that leverages OpenAI's CLIP model and YOLOv8 to perform natural language searches on local image folders and video files.

## Features

- **Local Image Search (`app.py`)**:
  - Index folders of images locally.
  - Search through your images using natural language queries (e.g., "a photo of a happy dog").
  - Uses the CLIP model for zero-shot image classification and text-image matching.

- **Video POI/Vehicle Search (`app_video_search.py`)**:
  - Extract frames from a video.
  - Run YOLOv8 detection to find persons and vehicles.
  - Crop detected objects and compute CLIP embeddings.
  - Search for specific persons or vehicles using natural language (e.g., "person wearing red jacket", "white sedan").

- **Live Object Detection (`5_Live_Object_Detection.py`)**:
  - Real-time object detection using YOLO models.

## Requirements

- Python 3.8+
- [PyTorch](https://pytorch.org/) (CUDA recommended for faster processing)
- Streamlit
- Transformers (Hugging Face)
- Ultralytics (YOLOv8)
- OpenCV

## Installation

1. Clone the repository:
   ```bash
   git clone https://gitlab.com/doublechen/image-search-engine.git
   cd image-search-engine
   ```

2. Create and activate a virtual environment (optional but recommended):
   ```bash
   python -m venv .venv
   # On Windows:
   .venv\Scripts\activate
   # On macOS/Linux:
   source .venv/bin/activate
   ```

3. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   # For video specific dependencies:
   pip install -r requirements_video.txt
   ```

## Setup Models

1. Create a `models` directory in the root folder if it doesn't exist.
2. Download your preferred CLIP models into the `models` directory. The application will scan this directory to list available models.
3. Download YOLOv8 weights (e.g., `yolov8n.pt`, `yolov8s.pt`, etc.) into the project root directory for the video search application.

## Usage

### Image Search
To run the image search application:
```bash
streamlit run app.py
```
1. Enter the path to your image folder in the UI.
2. Click "Index Folder".
3. Use the search bar to find images using natural language.

### Video Search
To run the video search application:
```bash
streamlit run app_video_search.py
# Alternatively, use the batch file on Windows:
.\run_video_search.bat
```
1. Enter the path to your video file.
2. Configure sampling FPS and YOLO confidence.
3. Select classes to detect (e.g., person, car).
4. Click "Index Video".
5. Search for specific objects or persons within the indexed video crops.

## Project Structure

- `app.py`: Streamlit application for image search.
- `app_video_search.py`: Streamlit application for video indexing and search.
- `video_indexer.py`: Script containing video indexing logic.
- `search_interface.py`: CLI or backend search interface logic.
- `5_Live_Object_Detection.py`: Real-time object detection script.
- `requirements.txt`: Python dependencies.

## License

This project is licensed under the MIT License.
