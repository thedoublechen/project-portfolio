# ENF Dataset Processor

A Python tool designed to automate the extraction and processing of Electrical Network Frequency (ENF) data from the Australian grid. It parses raw CSV data exports, extracts the `REGION_FREQ_MEASURE` data, and compiles it into a structured, multi-sheet Excel report categorized by day.

## Features

- **Automated Extraction**: Automatically finds and unzips raw data archives from the `downloads/` directory into the `dataset/` folder.
- **Data Parsing & Cleaning**: Iterates through the raw `.CSV` files, specifically targeting the `REGION_FREQ_MEASURE` table. It cleans the data and computes the actual grid frequency (Baseline 50.0 Hz + Frequency Deviation).
- **Region Mapping**: Translates raw region IDs (e.g., `NSW1`, `VIC1`) into human-readable area names.
- **Excel Report Generation**: Aggregates the cleaned data and exports it to `Daily_Grid_Forensics.xlsx`, creating a separate worksheet tab for each individual day for easy analysis.

## Prerequisites

- **Python 3.x**
- Install the required dependencies using `pip`:

```bash
pip install pandas openpyxl
```

## Folder Structure

For the script to work correctly, ensure the following directory structure (the script will automatically create `downloads` and `dataset` if they don't exist):

```text
enf-dataset-processor/
│
├── downloads/                  # Place your raw .zip data files here
├── dataset/                    # Unzipped .CSV files will be extracted here
├── enf_analysis.py             # Main processing script
└── Daily_Grid_Forensics.xlsx   # Output report (Generated automatically)
```

## Usage

1. Place your downloaded `.zip` files containing the raw grid data into the `downloads/` folder.
2. Run the main script:
   ```bash
   python enf_analysis.py
   ```
3. The script will:
   - Extract any `.zip` files in `downloads/` to the `dataset/` directory.
   - Parse all `.CSV` files within `dataset/`.
   - Process, calculate, and format the frequency data.
   - Generate `Daily_Grid_Forensics.xlsx` in the root directory.

## Output Details

The generated Excel file (`Daily_Grid_Forensics.xlsx`) will contain multiple tabs, each named after the specific date (e.g., `2026-04-20`). The tables include timestamps, original frequency deviation values, calculated actual frequency (`Actual_Hz`), and the mapped geographical area.
