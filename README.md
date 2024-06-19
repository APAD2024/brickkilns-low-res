# APAD: Air Pollution Asset Detection - Brick Kiln Module

APAD (Air Pollution Asset Detection) is a project aimed at identifying and analyzing various pollution-contributing assets using technologies such as artificial intelligence (AI), remote sensing, and crowd sourcing. The primary objective of APAD is to provide accurate and actionable data to help mitigate the effects of air pollution.

This repository focuses on the geo-location of brick kilns, which are known to be significant sources of air pollution, especially in developing regions. By using Sentinel-2 satellite imagery and machine learning algorithms, specifically random forest classifiers, APAD aims to accurately detect and map brick kilns across the entire Indo-Gangetic Plain (IGP) region.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Installation

Follow these steps to set up the project locally.

### Prerequisites


**Python**: Ensure you have Python installed on your system. You can download it from [python.org](https://www.python.org/).

**Google Earth Engine Account**: You need to create an account on the Google Earth Engine (GEE) platform.

#### Setting Up Google Earth Engine

1. **Sign Up**:
   - Go to the [Google Earth Engine sign-up page](https://earthengine.google.com/signup/).
   - Follow the instructions to create an account. You will need a Google account to sign up.

2. **Enable the Earth Engine API**:
   - Visit the [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project or select an existing project.
   - Navigate to **APIs & Services > Library**.
   - Search for "Earth Engine API" and click **Enable**.

3. **Authenticate and Initialize Earth Engine in Python**:
   - Install the `earthengine-api` library:
     ```bash
     pip install earthengine-api
     ```
   - Authenticate your Earth Engine account by running:
     ```python
     import ee
     ee.Authenticate()
     ee.Initialize()
     ```

### Clone the Repository

```bash
git clone https://github.com/yourusername/yourproject.git
cd yourproject
```

### Install Dependencies

Install the required libraries using the requirements.txt file.

```bash
pip install -r requirements.txt
```

### requirements.txt

```plaintext
concurrent.futures
csv
ee
fiona
geemap
geopandas
geopy
joblib
logging
matplotlib
numpy
pandas
rasterio
requests
shutil
tempfile
threading
```

## Usage

Follow these steps to use the project for detecting brick kilns.

### Step 1: Extract 1x1 Square Grids from the Region's Shapefile

From the scripts directory, you need to run the notebook called get_shapefiles.ipynb. This notebook extracts 1x1 square grids from the entire region's shapefile.

#### Prerequisites

You need to create the initial shapefile from GIS software like QGIS, ARCGIS, etc. Follow these steps:

1. Create 1x1 grid shapefiles using your GIS software.
2. Merge these individual grid shapefiles into one.
3. Provide the path to this merged shapefile in the notebook.

#### Running the Notebook
- Navigate to the scripts directory.
- Open and run the get_shapefiles.ipynb notebook.
- Ensure the paths to the shapefile directories are correctly set in the notebook.
- This script will create necessary directories if they don't already exist and save each 1x1 grid as a separate shapefile in the specified output directory.

#### Cell to separate shapefiles

```bash
# Iterate over each grid and save it as a shapefile
for i, row in gdf.iterrows():
    grid_geometry = row['geometry']
    grid_gdf = gpd.GeoDataFrame(geometry=[grid_geometry])
    grid_gdf.to_file(os.path.join(output_dir, f'grid_{i}.shp'))
 ```

 #### Cell to extract .shp and .dbf files

 ```bash
 for filename in os.listdir(input_dir):
    if filename.endswith('.shp') or filename.endswith('.dbf'):
        src_file = os.path.join(input_dir, filename)
        dst_file = os.path.join(output_dir, filename)
        try:
            shutil.move(src_file, dst_file)
        except FileNotFoundError:
            print(f"File {src_file} not found.")
```

### Step 2: Run the model on the extracted shapefiles

- Open Notebook: From the scripts directory, open the notebook named get_coordinates.ipynb.

- Set Path to Shapefiles: In the notebook, set the path to the folder containing the shapefiles generated in Step 1.

    ```bash
    folder_path = '../../grids_final_pak_final_version'
    ```

- Load Random Forest Model: Load the Random Forest (RF) model using the following code:

    ```bash
    rf_classifier = load('../../models/RF_model_v3.pkl')
    ```

- Set Paths for CSVs:

    - Coordinate CSV: Path to store the coordinates of detected brick kilns.
    - Error CSV: Path to store any erroneous shapefiles or processing errors.
    - Last Shapefile CSV: Path to store the name of the last shapefile that was processed.

    ```bash
    csv_file = '../Data/coordinate_csvs/centers_2000_3000.csv'
    unused_files_csv = '../Data/error_csvs/not_processed_2000_3000.csv'
    text_file = '../Data/latest_processed/latest_processed_2000_3000.txt'
    ```

- Run the Entire Notebook: After setting the paths and loading the model, run the entire notebook to start the detection process.


## Methodology
[ ] to be done

## Contributors
- Suleman Hamdani
- Khizer Zakir
- Hassan Aftab

## Contact Us




