# Sentinel-2 Water Quality Monitoring Pipeline — Lake Liddell, NSW

An automated Python pipeline for monitoring water quality proxies in mine-adjacent
waterbodies using Sentinel-2 L2A imagery from Microsoft Planetary Computer.

## Study Area
Lake Liddell, Hunter Valley, NSW — adjacent to the former Liddell coal mine
(closed 2023, now in rehabilitation). The lake has documented water quality
concerns associated with acid mine drainage and sediment runoff.

## Pipeline Overview
1. **Acquisition** — STAC query and band download via Planetary Computer
2. **Cloud masking** — Scene Classification Layer (SCL) band filtering
3. **Index computation** — NDWI (water extent), NDTI (turbidity proxy),
suspended sediment ratio (B04/B03)
4. **Zonal statistics** — Per-scene aggregation over waterbody AOI
5. **Report generation** — Automated HTML/PDF summary with time-series plots

## Stack
- `pystac-client` + `planetary-computer` — data acquisition
- `rasterio` + `numpy` — raster processing
- `geopandas` + `rasterstats` — vector handling and zonal statistics
- `matplotlib` + `seaborn` — visualisation
- `jinja2` + `fpdf2` — automated report output

## Skills Demonstrated
- Local Python geospatial environment management (conda)
- Automated satellite data acquisition via STAC API
- Cloud masking and preprocessing of Sentinel-2 L2A
- Water quality index computation (NDWI, NDTI)
- Zonal statistics and time-series anomaly detection
- Automated report generation

## Study Context
This project forms part of a five-project GIS and remote sensing portfolio
targeting Australian environmental consulting and mining sectors. It complements
[Project 1](https://github.com/YOUR_USERNAME/mount-arthur-ndvi-monitoring)
(GEE-based vegetation recovery monitoring at Mount Arthur Coal Mine) by
demonstrating local Python tooling and pipeline automation.

## Requirements
See `environment.yml` for full dependencies. Requires a free
[Microsoft Planetary Computer](https://planetarycomputer.microsoft.com) account
for authenticated STAC access.

## Usage
```bash
conda env create -f environment.yml
conda activate water-quality-pipeline
jupyter notebook notebooks/01_acquisition.ipynb


Outputs

• Time-series plots of NDWI, NDTI, and turbidity proxy (2021–2024)
• Anomaly detection flagging high-turbidity episodes
• Automated summary report (HTML + PDF)

Author

Luke Kennedy | MSc GIS & Remote Sensing, UCDPortfolio: github.com/YOUR_USERNAME
