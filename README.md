# Dutch Forest Change Detection

A GitHub Pages site presenting an INFOMSSML (Spatial Statistics & Machine Learning) term project tracking deforestation in the Dutch provinces of Utrecht and Gelderland using NDVI temporal profiles derived from Landsat imagery (2013–2025).

**Live site:** [https://anthony-n-kamau.github.io/dutch-forest-change-detection/](https://anthony-n-kamau.github.io/dutch-forest-change-detection)

## Project overview

Since 2013, deforestation in the Netherlands has consistently outpaced new tree planting, with forested land increasingly converted to heathland and other nature types — a reversal of the large-scale planting subsidised by the Dutch government in 1986 to address a national wood shortage.

This project investigates how and where these changes have occurred across **Utrecht** and **Gelderland**, two provinces with well-documented forest loss, by analysing satellite-derived NDVI (Normalised Difference Vegetation Index) time series to detect local variation in forest greenness and identify where change has accelerated or reversed.

## Research question

> To what extent do NDVI temporal profiles differ between forests within Utrecht and Gelderland, and did the rate or direction of change shift between the 2013–2017 and 2017–2021 periods?

## Expected outputs

- Temporal NDVI profiles per forest zone across both provinces
- Maps of deforested areas broken down by time period
- Comparative analysis of NDVI change rates between the two study windows
- NDVI imagery visualising greenness trends over time

## Data & methods

- **Source:** Microsoft Planetary Computer
- **Imagery:** Landsat 7, 8, and 9 multispectral scenes
- **Derived variable:** NDVI (Normalised Difference Vegetation Index) time series
- **Study period:** 2013–2025, compared across two windows: 2013–2017 and 2017–2021

Approach:

1. Compute NDVI from near-infrared and red spectral bands
2. Build temporal NDVI profiles across both study windows
3. Run change detection to identify deforestation hotspots
4. Apply spatial statistics and machine learning to analyse local patterns

## Tools & software

- **Python** — Planetary Computer API, `rioxarray`, `geopandas`, `pystac`
- **R** — `sf`, `tmap`, `spatialRF`
- **QGIS** — spatial visualisation and map production

## Repository structure

```
├── index.html          # GitHub Pages site (project overview)
├── images/              # Site imagery (forest, NDVI maps, satellite scenes)
└── README.md
```

*(Update this once analysis notebooks/scripts are added to the repo.)*

## Course context

- **Course:** INFOMSSML — Spatial Statistics and Machine Learning
- **Programme:** MSc Applied Data Science, Utrecht University
- **Term:** 2026, Group 4
- **Supervisors:** Dr. SM Labib (Dept. of Human Geography & Spatial Planning), Dr. Menno Straatsma (Dept. of Physical Geography)
