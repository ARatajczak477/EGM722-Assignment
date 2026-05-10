# Flood Exposure Mapper

Maps flood extent and population exposure from Hurricane Beryl (July 2024) in the Houston/Gulf Coast region, Texas, using Sentinel-1 Synthetic Aperture Radar (SAR) data downloaded from NASA EarthData.

---

## Notebooks

| Notebook | Description |
|---|---|
| `01_data_download.ipynb` | Authenticate with NASA EarthData, search for OPERA Sentinel-1 scenes covering the study area, download tiles, download WorldPop population raster |
| `02_flood_analysis.ipynb` | Load and mosaic SAR tiles, reproject, threshold backscatter to produce a flood mask, vectorise polygons, estimate exposed population using zonal statistics, produce an interactive map |

---

## Requirements

- [Anaconda](https://www.anaconda.com/)
- A free NASA EarthData account: https://urs.earthdata.nasa.gov

---

## Setup

### 1. Clone the repository

```
git clone https://github.com/ARatajczak477/EGM722-Assignment.git
cd EGM722-Assignment
```

### 2. Create the conda environment

**Option A — Anaconda Navigator:**

1. Open Anaconda Navigator
2. Click **Environments** in the left sidebar, then **Import**
3. Browse to `environment.yml`, name the environment `egm722-assignment`, click **Import**
4. Wait for installation to complete (this may take several minutes)

**Option B — command line:**

```
conda env create -f environment.yml
conda activate egm722-assignment
```

### 3. Launch JupyterLab

**Anaconda Navigator:** Switch the *Applications on* dropdown to `egm722-assignment`, then click **Launch** next to JupyterLab.

**Command line:**

```
conda activate egm722-assignment
jupyter lab
```

### 4. NASA EarthData credentials

Run `01_data_download.ipynb`. The first login cell will prompt for your EarthData username and password and store them securely in a `.netrc` file. Subsequent runs will log in automatically.

**You may also need to accept the ASF DAAC data access agreement** the first time you download.
If the download fails with a EULA error, log in at https://urs.earthdata.nasa.gov, go to **My Profile → Applications**, find Alaska Satellite Facility Data Access, and click **Accept**. Or follow the link shown in the error.

---

## Running the notebooks

Run the notebooks **in order**:

1. **`01_data_download.ipynb`** — downloads SAR tiles and the WorldPop raster into `data/raw/`
2. **`02_flood_analysis.ipynb`** — reads from `data/raw/`, writes processed files to `data/processed/`, and saves `outputs/flood_map.html`

---

## Dependencies

| Package | Purpose |
|---|---|
| `earthaccess` | NASA EarthData search and download |
| `rasterio` | SAR raster I/O, reprojection, masking, merging |
| `geopandas` | Flood polygon vector operations |
| `shapely` | Geometry construction and repair |
| `rasterstats` | Zonal statistics for population exposure |
| `folium` | Interactive HTML map output |
| `matplotlib` | Backscatter histogram |
| `numpy` | Array operations |
| `jupyterlab` | Notebook interface |

---

## Data sources

| Dataset | Source |
|---|---|
| OPERA L2 RTC-S1 (Sentinel-1 SAR) | Downloaded automatically by `01_data_download.ipynb` via [NASA EarthData](https://search.earthdata.nasa.gov/) |
| WorldPop USA 2020 1 km | Downloaded automatically by `01_data_download.ipynb` via [WorldPop](https://www.worldpop.org/) |

---

## Outputs

| File | Description |
|---|---|
| `data/processed/sar_clipped.tif` | Mosaicked and clipped SAR backscatter |
| `data/processed/sar_utm.tif` | Reprojected to UTM Zone 14N |
| `data/processed/flood_mask.tif` | Binary flood classification raster |
| `data/processed/worldpop_clipped.tif` | WorldPop raster clipped to study area |
| `outputs/flood_map.html` | Interactive map (open in any browser) |
