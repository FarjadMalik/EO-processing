# EO Processing

Analyze Earth Observation satellite imagery for flood detection and mapping using Maxar open data. This project provides tools to process, visualize, and analyze flood extent before and after events using cloud-optimized geospatial data.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Usage](#usage)

## Overview

This Earth Observation (EO) processing pipeline is designed to:

- Download and process satellite imagery from Maxar's open data archives
- Detect and extract flood extents from multispectral imagery
- Generate visual comparisons of flood impact (pre/post analysis)
- Handle large-scale raster data efficiently with cloud-native formats (COG - Cloud Optimized GeoTIFFs)

This is particularly useful for disaster response, environmental monitoring, and flood risk assessment.

## Features

✨ **Key Capabilities:**

- 🛰️ Access to Maxar open satellite data via AWS S3
- 🔍 Flood extent detection algorithms
- 🗺️ Interactive web-based visualizations (Folium/Leaflet maps)
- 📊 Support for multi-band raster analysis
- ☁️ Cloud-native geospatial data processing (COG, STAC)
- 🚀 Distributed processing with Dask
- 📍 Geospatial vector operations with GeoPandas

## Prerequisites

Before you begin, ensure you have:

- **Python 3.13+** installed on your system
- **uv** - A fast Python package installer and manager ([Install uv](https://docs.astral.sh/uv/getting-started/installation/))
- **Git** for cloning the repository
- **AWS Credentials** (Optional: for uploading files)

### Installing uv

If you don't have `uv` installed yet, install it first:

**Windows (PowerShell):**
```powershell
powershell -ExecutionPolicy BypassUser -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**macOS/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Verify the installation:
```bash
uv --version
```

## Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/FarjadMalik/EO-Processing.git
cd EO-Processing
```

### Step 2: Create a Virtual Environment with uv

Create a Python 3.13+ virtual environment:

```bash
uv venv --python 3.13
```

Activate the virtual environment:

**Windows (Command Prompt):**
```cmd
.venv\Scripts\activate
```

**Windows (PowerShell):**
```powershell
.venv\Scripts\Activate.ps1
```

**macOS/Linux:**
```bash
source .venv/bin/activate
```

### Step 3: Install Dependencies with uv

Install all project dependencies in one command:

```bash
uv sync
```

This will install all dependencies specified in `pyproject.toml` and create a `uv.lock` file for reproducible environments.

### Alternative: Install from requirements.txt (if needed)

If you prefer using `requirements.txt`:

```bash
uv pip install -r requirements.txt
```

## Configuration

### Setting Up Environment Variables

The project requires AWS credentials and configuration to access Maxar data. Create a `.env` file in the project root:

```bash
cp .env.example .env  # copy or create manually
```

Edit `.env` with your configuration:

```env
# AWS S3 Configuration
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_BUCKET_NAME=maxar-open-data
AWS_BUCKET_PATH=events/
# Define event and collection id of interest
CATALOG_URL="https://maxar-opendata.s3.amazonaws.com/events/catalog.json"
EVENT_NAME="Sudan-flooding-8-22-2022"
COLLECTION_ID=None # We will base ourselves on an area of interest, a collection can be specified if needed
FLOOD_DATE='2022-08-22'
# Maxar asset key for multispectral 8-band/4-band imagery
ASSET_KEY="ms_analytic"
MAX_TILES=2
```

**⚠️ Security Note:** Never commit the `.env` file to version control as it contains sensitive informaton (AWS Credentials). It's included in `.gitignore` for a reason.

#### AWS Credentials

If you already have credentials:

1. Add them to your `.env` file
2. Ensure your credentials have access to the S3 bucket

Otherwise contact support or reach out.

## Quick Start

### Run Flood Analysis

Use the Jupyter notebook for interactive analysis:

```bash
uv run --with jupyter jupyter lab
```
Proceed to src/Test_TL_FarjadMalik_Sudan_22August2022.ipynb and `Run All`

This notebook demonstrates:
- Loading maxar satellite imagery
- Processing tiles and generating mosaics
- Analyzing and Classifying flood extents
- Generating pre/post comparison
- Exporting results

### 3. Generate Outputs

The pipeline produces:

- **COG Files** - Mosaic and Flood mask COGs which are uploaded to s3.
- **Plots** - Visualizations

## Project Structure

```
EO_Processing/
├── README.md              # This file
├── pyproject.toml         # Project metadata and dependencies
├── requirements.txt       # Pinned package versions 
├── .env                   # Environment variables (NOT in git)
├── .env.example           # Template for .env file
├── .gitignore             # Git ignore rules
├── src/
│   ├── main.py            # Hello world main script
│   └── Test_TL_FarjadMalik_Sudan_22August2022.ipynb
│                           # Interactive analysis notebook (case study)
├── outputs/               # Generated results
└── .venv/                 # Virtual environment (auto-created with uv)
```

## Usage

### Running Python Scripts

With the virtual environment activated:

```bash
python src/<name>.py
```

### Running Jupyter Notebooks

```bash
uv run --with jupyter jupter lab
```

Then navigate to `src/Test_TL_FarjadMalik_Sudan_22August2022.ipynb` to explore the flood analysis workflow.

### Processing New Events

1. Update EVENT_NAME in `.env` file along with FLOOD_DATE and related fields.
2. Modify the notebook or create new Python scripts referencing target imagery
3. Run the analysis
45. Results will be saved to `outputs/`

## Key Technologies

- **numpy/GeoPandas** - Vector geospatial data analysis
- **s3fs/Boto3** - AWS S3 integration
- **Dask/XArray** - Distributed computing for large datasets
- **STAC/COG** - Cloud-native geospatial formats

## Troubleshooting

### Module Not Found Errors

Ensure your virtual environment is activated:

**Windows (PowerShell):**
```powershell
.venv\Scripts\Activate.ps1
```

**Windows (Command Prompt):**
```cmd
.venv\Scripts\activate
```

### AWS Credentials Issues

Verify your `.env` file:
```bash
cat .env  # macOS/Linux
type .env # Windows
```

Ensure all required variables are set and valid.

### Jupyter Not Found

Install it with uv:
```bash
uv add jupyterlab ipykernel
```
Set up a targetted kernel in your venv using:
```bash
uv run ipython kernel install --user --env VIRTUAL_ENV .\.venv\ --name=eo-processing
```

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License.

## Support

For issues, questions, or suggestions, please open an issue on the GitHub repository.

---

**Last Updated:** March 2026
