# Detecting Deforestation in the Netherlands

Analysing NDVI (Normalized Difference Vegetation Index) time series from Sentinel-2 imagery to track annual forest condition changes across four Dutch forests between 2016 and 2025.

Live site: https://anthony-n-kamau.github.io/Dutch-forest-change-detection/

## Overview

National-level deforestation reports in the Netherlands are typically published at five-year intervals using topographic map overlays, aggregated at the provincial level. This obscures annual variation and differences between individual forests within the same province.

This project addresses that gap by building an NDVI-based monitoring pipeline that:

- Retrieves cloud-filtered Sentinel-2 Level-2A imagery for four forest areas
- Computes annual growing-season NDVI composites (2016–2025)
- Quantifies year-over-year and long-term (2016 → 2025) NDVI change per forest
- Cross-references the NDVI signal against high-resolution aerial orthophotos to distinguish canopy loss from canopy stress

**Research question:** To what extent do NDVI temporal profiles differ between forests within different Dutch provinces, and did the rate or direction of change shift on an annual basis between 2016 and 2025?

## Study area

Four forests across two provinces, chosen to span a range of canopy densities:

| Forest | Province | Density type |
|---|---|---|
| Soesterduinen | Utrecht | Sparse |
| Amerongse Bos | Utrecht | Dense |
| Speulderbos | Gelderland | Dense |
| Hoge Veluwe | Gelderland | Mixed |

Each forest is represented by a single polygon manually delineated in QGIS.

## Method summary

1. **Data source:** Sentinel-2 L2A surface reflectance imagery, retrieved via the Microsoft Planetary Computer STAC API, filtered to ≤20% cloud cover.
2. **Cloud masking:** Applied using the Scene Classification Layer (SCL) band, excluding no-data, cloud, cloud-shadow, cirrus, and snow/ice pixels.
3. **NDVI:** Computed as `(NIR − Red) / (NIR + Red)` from bands B08 and B04 at 10 m resolution.
4. **Compositing:** Annual median NDVI over the growing season (April–October) per forest polygon.
5. **Change detection:** Long-term difference maps (2025 − 2016) and year-by-year change rates, both per pixel and as forest-level spatial means.
6. **Validation:** Visual comparison against PDOK aerial orthophotos (2016 vs. 2025) to check whether NDVI changes reflect canopy extent or canopy condition.

## Key findings

- All four forests show a stable NDVI baseline (~0.75–0.85) through 2021, followed by an abrupt, synchronous drop to ~0.5 between 2021 and 2022 — roughly five times larger than any other year-to-year change in the study period.
- NDVI remained flat after 2022 with no clear recovery through 2025.
- The synchrony across forests of differing density and province points to a shared external driver rather than localised deforestation — most plausibly the 2022 drought and heatwave in northwestern Europe, with legacy effects from repeated hot, dry years suppressing recovery.
- Orthophoto comparison shows canopy extent actually increased in all four forests between 2016 and 2025, ruling out deforestation and supporting a drought-stress (rather than tree-removal) interpretation of the NDVI decline.
- A possible confound is a Sentinel-2 L2A processing baseline update around the same period, which could partly affect absolute NDVI values; this is noted as an open question for future work.

## Repository structure

```
.
├── deforestation.ipynb     # Full analysis pipeline: data retrieval, cloud masking,
│                            # NDVI computation, compositing, and visualisation
├── Group4_project.pdf       # Written report describing background, methods, results,
│                            # and discussion
├── Forests/                 # Forest AOI shapefiles (not included — see Data below)
└── scratch/                 # Output figures, GeoTIFFs, and exported polygons
                              # (generated when the notebook is run)
```

## Requirements

- Python 3.10+
- `pandas`, `numpy`, `matplotlib`
- `xarray`, `rioxarray`, `stackstac`, `dask`
- `geopandas`, `shapely`
- `pystac-client`, `planetary-computer`
- `rich`

Install with:

```bash
pip install pandas numpy matplotlib xarray rioxarray stackstac dask[array] geopandas shapely pystac-client planetary-computer rich
```

## Data

- Satellite imagery is streamed on demand from the [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/) STAC catalog — no manual download needed.
- The forest boundary polygons (`Forests/Forests.shp`) are not bundled in this repository; provide your own shapefile with matching forest names, or adapt the AOI-loading cell to your own areas of interest.
- Aerial orthophoto comparisons use imagery from [PDOK](https://www.pdok.nl/).

## Usage

1. Clone the repository and install the requirements above.
2. Place forest boundary polygons at `Forests/Forests.shp` (see Data).
3. Open `deforestation.ipynb` and run the cells in order — later sections depend on outputs from earlier ones (STAC search → stacking → cloud masking → NDVI → compositing → analysis).
4. Figures and exported GeoTIFFs/polygons are written to `./scratch`.

## Limitations

- Forest boundaries were manually delineated and may include non-forest pixels.
- Only four forests across two provinces were analysed, limiting generalisability.
- Annual median compositing can obscure short-lived intra-seasonal dynamics.
- NDVI alone cannot fully distinguish drought stress from actual tree removal; canopy height models or complementary datasets would strengthen causal claims.
- A Sentinel-2 L2A processing baseline change during the study period is a possible (unresolved) source of systematic bias.

## License

Add a license of your choice (e.g. MIT) if you intend this to be reused.
