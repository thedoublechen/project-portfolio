# Geoloca Data Fetcher

Geoloca Data Fetcher is a comprehensive suite of Python tools designed to automate the collection of street-level imagery and geographical data from various sources across Australian suburbs. The collected datasets are organized by State, City, District, and Suburb to support research, machine learning models, and geographical analysis.

## Features

- **Multi-Source Fetching**: Scripts available to fetch imagery from:
  - **Mapillary**: Deduplicated and rate-limited extraction using Mapillary's graph API.
  - **KartaView**: Extracts sequences and images.
- **Intelligent Boundaries**: Utilizes OpenStreetMap (Nominatim) to dynamically fetch and cache suburb polygons and bounding boxes for highly accurate sampling.
- **Robustness**: Includes duplicate prevention, rate limiting, logging, retry mechanics, and checkpointing for long-running batch operations.
- **Dataset Organization**: Automatically structures data into an intuitive `Dataset -> State -> City -> District -> Suburb` hierarchy.

## Prerequisites

- **Python 3.8+**
- Required Python libraries:
  ```bash
  pip install requests
  ```

## Configuration & Setup

1. **Mapillary Token (config.py)**:
   To use Mapillary, copy your developer token into `config.py`:
   ```python
   # config.py
   MAPILLARY_ACCESS_TOKEN = "your_actual_long_access_token_here"
   ```
   > **⚠️ Security Warning:** Ensure `config.py` is excluded from version control. It has already been added to `.gitignore`.

2. **Suburb Master List**:
   The `suburbs_master_list.py` contains the comprehensive dictionary of target regions in Australia categorized by city and district.

## Usage

Each script is designed to be run independently based on the data source you wish to target. 

### Mapillary
```bash
python mapillary_fetcher.py
```
- Fetches imagery bounded by accurate OSM bounding boxes. 
- Includes robust API error handling and tracks downloaded images to prevent duplication.

### KartaView
You can also run the KartaView fetcher using:
```bash
python kartaview_data_fetcher.py
```

## Utility Scripts

- `verify_and_organize.py`: Validates dataset structure and organizes stray images.
- `count_images_by_state_city.py`: Generates statistics and distributions of the dataset.
- `generate_report.py`: Creates a comprehensive breakdown summary of collected data across platforms.

## Logging and Monitoring
Progress and errors are written to timestamped log files and the console output to allow for easy monitoring of overnight fetching processes.

## License
*This project is built for localized research and data scraping. Ensure you adhere to the terms of service of Mapillary and KartaView when utilizing their APIs.*
