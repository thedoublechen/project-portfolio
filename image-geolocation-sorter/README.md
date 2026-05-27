# StreetCLIP Image Sorter

📸 A Streamlit web application that automatically categorizes images into folders based on their geographical location or visual characteristics using a locally stored StreetCLIP model.

## Features

- **Local Processing**: Uses locally downloaded StreetCLIP models (or any compatible Hugging Face vision-language models) to process images.
- **Customizable Labels**: Define your own categories (e.g., "Australia", "Non-Australia", "USA", "Europe").
- **Confidence Threshold**: Automatically filter out uncertain predictions into a separate `_unsure_` folder.
- **Hardware Acceleration**: Automatically detects and uses GPU (CUDA) if available for faster processing.
- **Supported Formats**: Processes `.jpg`, `.jpeg`, `.png`, and `.webp` images recursively from the input directory.

## Requirements

- Python 3.8+
- [PyTorch](https://pytorch.org/) (CUDA version recommended for much faster processing)
- Transformers
- Streamlit
- Pillow

You can install the necessary packages using pip:

```bash
pip install streamlit torch torchvision transformers pillow
```

## Setup

1. Clone or download this repository.
2. Create a `models` directory in the root of the project (if it doesn't already exist).
3. Download a StreetCLIP model (e.g., `geolocal/StreetCLIP`) from Hugging Face and place it in a subdirectory inside `models/`. 
   - For example, you should have `models/streetclip-model/config.json`, `models/streetclip-model/pytorch_model.bin` (or `model.safetensors`), etc.
4. Prepare your input images in a source directory (e.g., `source/`).

## Usage

1. Start the Streamlit app:

```bash
streamlit run app.py
```

2. Open your web browser to the provided local URL (usually `http://localhost:8501`).
3. Configure the settings in the sidebar:
   - **Select a Model**: Choose the locally stored model from the dropdown.
   - **Input Folder Path**: Enter the absolute path to your folder containing the images to be sorted.
   - **Output Folder Path**: Enter the absolute path where categorized subfolders will be created.
   - **Labels**: Define your categories as a comma-separated list (e.g., `Australia, Non-Australia`).
   - **Confidence Threshold**: Adjust the slider. Images with a prediction confidence below this threshold will be placed in an `_unsure_` folder.
4. Click **Start Sorting**. 

The app will read the images, predict their categories, and move them to the corresponding subfolders in the output directory.

## Project Structure

- `app.py`: The main Streamlit application script.
- `models/`: Directory where local StreetCLIP models should be stored.
- `source/`: A directory you can use to place input images.
- `output/`: A directory you can use for the sorted image output.
