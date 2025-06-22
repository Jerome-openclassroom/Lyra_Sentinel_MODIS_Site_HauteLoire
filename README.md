# Lyra_Sentinel_MODIS_Site_HauteLoire

**A reproducible remote sensing pipeline combining Sentinel-2 NDVI and MODIS LST for a selected ecological site in Haute-Loire, France.**

## Project Overview

This repository demonstrates a compact, transparent workflow to extract and monitor two key biophysical variables:
- **Normalized Difference Vegetation Index (NDVI)** — Sentinel-2 Harmonized, clipped to a 1 km ROI.
- **Land Surface Temperature (LST)** — MODIS MOD11A1, averaged over a 5 km buffer.

Both metrics are computed for a precise point located in Haute-Loire, representing a semi-natural meadow-bocage ecosystem, and intended to feed into Lyra's open ecological models.

## Site Information

| Parameter | Value |
|-----------|-------|
| **Longitude** | 4.0966666 E |
| **Latitude**  | 45.0430555 N |
| **NDVI ROI (visualized)** | Square buffer ~1 km |
| **LST ROI (visualized)**  | Disk buffer ~5 km |
| **Time period** | May 15 — June 15, 2025 |

## Results

| Variable | Value |
|----------|-------|
| **Mean NDVI** | ~0.75 (2 decimal places) |
| **Mean LST (°C)** | ~18.8 °C (1 decimal place) |

These values reflect the site’s healthy vegetative activity and typical late spring surface temperatures in a mid-altitude French upland context.

## 📁 Repository Structure

```plaintext
Lyra_Sentinel_MODIS_Site_HauteLoire/
│
├── README.md                  # Project overview and instructions
├── data_and_results/          # All scripts, outputs and visual aids
│   ├── NDVI_LST_HauteLoire.ipynb   # Jupyter Notebook (full pipeline)
│   ├── NDVI_LST_HauteLoire.md      # Markdown version (easy reading)
│   ├── NDVI_LST_map.jpg            # Result map: NDVI square & LST disk
│   └── NDVI_LST_HauteLoire.csv     # (Optional) CSV export with computed values


   
## Dependencies

This pipeline relies on:
- `earthengine-api`
- `geemap` (Folium mode for robust interactive map display)

Install via:
```bash
pip install earthengine-api geemap
```

## How to Use

1. Clone this repo and open the notebook:
   ```bash
   git clone https://github.com/yourusername/Lyra_Sentinel_MODIS_Site_HauteLoire.git
   cd Lyra_Sentinel_MODIS_Site_HauteLoire
   ```

2. Authenticate your Google Earth Engine account inside the notebook:
   ```python
   import ee
   ee.Authenticate()
   ee.Initialize()
   ```

3. Run all cells to reproduce:
   - NDVI & LST computation
   - ROI clipping & visualization
   - CSV export to Google Drive
   - Interactive map with clear buffer overlays.

## Key Concepts

- **ROI Clipping:** NDVI visualization is clipped to a 1 km square to avoid displaying the full Sentinel-2 tile (~100 km²).
- **Buffering:** LST is computed using a larger disk to smooth out thermal heterogeneity.
- **Low Precision:** Outputs are rounded to 2 decimals (NDVI) and 1 decimal (LST) for ecological realism.

## Map Preview

![NDVI and LST buffers](NDVI_LST_map.png)

## Contact

For questions or contributions, open an issue or fork this repo.

Part of the Lyra ecological AI demonstration series — designed for open science and local monitoring.

**License:** MIT
