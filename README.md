# Sentinel-2 Water Quality Monitoring Pipeline — Lake Liddell, NSW

An automated Python pipeline for monitoring water quality proxies in a mine-adjacent
waterbody using Sentinel-2 L2A imagery from Microsoft Planetary Computer. The pipeline
detects a clear post-industrial transition signal following the closure of Liddell
Power Station in April 2022.

## Key Finding

Analysis of 119 Sentinel-2 scenes (2021–2024) reveals a sharp and sustained shift
in water quality indices coinciding with the April 2022 closure of Liddell Power
Station. Pre-closure scenes show high NDWI (0.6–0.9) and low turbidity proxy (0.3–0.5),
consistent with actively managed cooling water. Post-closure scenes stabilise at
lower NDWI (~0.08) and higher turbidity (~0.89), consistent with an unmanaged
waterbody adjusting to natural conditions. Seven anomalous scenes were detected
against the post-transition baseline, including elevated turbidity episodes in
April–May 2022 (sediment resuspension) and anomalously clear water episodes in
late 2023–2024 (above-average Hunter Valley rainfall).

## Study Area

Lake Liddell, Hunter Valley, NSW — a former cooling water reservoir adjacent to
Liddell Power Station (closed April 2022, now in rehabilitation). The lake is subject
to ongoing water quality monitoring obligations under NSW environmental legislation.

## Pipeline Overview

1. **Acquisition** — STAC query via Planetary Computer API, 267 scenes retrieved
2. **Cloud masking** — Scene Classification Layer (SCL) band filtering, scenes >20% cloud rejected
3. **Preprocessing** — Band download, AOI clipping, 20m→10m upsampling, DN scaling
4. **Index computation** — NDWI (McFeeters 1996), NDTI (Lacaux 2007), turbidity proxy (B04/B03)
5. **Deduplication** — Overlapping tile handling, 119 clean scenes retained
6. **Anomaly detection** — Post-transition baseline with 2 standard deviation flagging
7. **Report generation** — Automated PDF summary with time-series plots and anomaly table

## Results

| Metric | Value |
|---|---|
| Scenes retrieved | 267 |
| Scenes processed | 119 (after dedup + pixel filter) |
| Analysis period | January 2021 – December 2024 |
| Mean cloud cover | 5.1% |
| Anomalous scenes detected | 7 |
| Post-closure NDWI baseline | 0.084 ± 0.021 |
| Post-closure turbidity baseline | 0.892 ± 0.024 |

## Stack

- `pystac-client` + `planetary-computer` — automated STAC data acquisition
- `rasterio` + `numpy` — raster processing and band operations
- `scikit-image` — 20m to 10m band upsampling
- `geopandas` — AOI handling and CRS reprojection
- `matplotlib` — time-series and spatial visualisation
- `fpdf2` — automated PDF report generation
- `conda` — local environment management

## Skills Demonstrated

- Local Python geospatial environment management (conda/Miniconda)
- Automated satellite data acquisition via STAC API (no manual downloads)
- Sentinel-2 L2A cloud masking using Scene Classification Layer
- Water quality index computation (NDWI, NDTI, turbidity proxy)
- Multi-scene time-series processing (119 scenes)
- Statistical anomaly detection against a defined baseline
- Automated PDF report generation suitable for client/regulator delivery
- Git version control with documented outputs

## Requirements

See `environment.yml` for full dependencies. No account or API key required —
data access is handled automatically via the Planetary Computer public STAC API.

## Usage

```bash
conda env create -f environment.yml
conda activate water-quality-pipeline
jupyter notebook notebooks/01_acquisition.ipynb
```

Note: the time series loop (Cell 7) processes 267 scenes and takes approximately
20–40 minutes depending on connection speed. Processed CSVs are saved to
`data/outputs/` and can be reloaded directly for subsequent cells.

## Outputs

- `data/outputs/01_scene_availability.png` — monthly scene count and cloud cover distribution
- `data/outputs/02_test_scene_indices.png` — spatial index maps for a single test scene
- `data/outputs/03_water_quality_timeseries.png` — 4-year NDWI, NDTI, turbidity time series
- `data/outputs/04_anomaly_detection.png` — post-transition anomaly overlay with baseline band
- `data/outputs/water_quality_timeseries_dedup.csv` — full deduplicated scene results
- `data/outputs/water_quality_anomalies.csv` — anomaly-flagged scenes with z-scores
- `data/outputs/lake_liddell_water_quality_report.pdf` — automated summary report

## Author

Luke Kennedy | MSc GIS & Remote Sensing, UCD
Portfolio: [github.com/lukek6892](https://github.com/lukek6892)