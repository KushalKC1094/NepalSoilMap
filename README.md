# Soil Map: Web Mapping of Soil Properties in the Terai, Nepal

Historical web mapping project for visualizing soil properties across the Terai region of Nepal.

This repository contains a static, Leaflet-based web map. It displays interpolated maps of five soil properties (pH, organic matter, total nitrogen, clay content, and boron) and shows their values at any point the user clicks. It is preserved as a reference resource.

> **Repository status: Historical project (2018).** The site is preserved for future reference and is not actively maintained. Some external services it relies on have since changed or been retired (see [Notes for reuse](#notes-for-reuse)).

## Overview

The web map:

- Displays five soil property layers as semi-transparent map tiles that can be switched on and off.
- Reports the pH, organic matter, total nitrogen, clay, and boron values at any clicked location by reading the underlying GeoTIFFs in the browser.
- Overlays Nepal district boundaries.
- Provides a basemap switcher (street map and aerial imagery), a "locate me" control, and a legend for each layer.
- Includes a metadata page describing how the soil data were collected and analyzed.

**Live site:** https://kushalkc1094.github.io/kessi9400.github.io/index.htm (once GitHub Pages is enabled for this repository)

## Soil Data

Soil data come from a detailed soil survey (1:10,000 scale) in which soil pits were described and sampled. Soil samples were analyzed in a soil laboratory for texture, pH, total nitrogen, available P and K, boron, zinc, and organic matter. Five of these properties are mapped:

| Layer | Units | Laboratory method |
|---|---|---|
| Soil pH | — | 1:2 soil–water suspension; pH meter calibrated with pH 7 and 4.01 buffers |
| Organic matter | % | Modified Walkley–Black method |
| Total nitrogen | % | Micro-Kjeldahl digestion–distillation |
| Clay content | % | Hydrometer method and texture classification |
| Boron | ppm | — |

Map extent: approximately 80.0–88.2° E, 26.2–29.3° N (Web Mercator, EPSG:3857).

## Repository Contents

| Path | Description |
|---|---|
| `index.htm` | Main web map page. |
| `index1.html` | Metadata page: survey description, value ranges and laboratory methods. |
| `next.html` | Alternative layout of the soil property panel. |
| `SoilPH/`, `Organicmatter/`, `TN/`, `Clay/`, `Boron/` | Map tiles for each soil property (256 × 256 PNG, TMS scheme, zoom levels 0–13). |
| `tif/` | GeoTIFFs of each soil property, used to read values at clicked points. |
| `Legends/` | Legend images for the five layers. |
| `Nepal_district_geojson/` | Nepal district boundaries (GeoJSON). |
| `js/` | Leaflet, the Bing layer and locate plugins, and the district boundaries as a JavaScript variable (`Districts.js`). |
| `leaflet-geotiff.js`, `vendor/` | Libraries for reading and rendering GeoTIFFs in the browser (`geotiff.js`, `plotty.js`). |
| `css/`, `index/`, `semantic/` | Stylesheets and the Semantic UI framework used for page layout. |
| `tilemapresource.xml` | Tile set metadata generated when the tiles were created. |

## How It Works

1. The soil property rasters were exported as GeoTIFFs and cut into map tiles for zoom levels 0–13 (a GDAL `gdal2tiles`-style TMS tile set).
2. `index.htm` loads each tile set as a Leaflet tile layer, with a layer control to toggle them.
3. When the user clicks the map, the page reads the value of each GeoTIFF at that location (`leaflet-geotiff.js`) and shows it in the information panel.

## Running Locally

The site is plain HTML, CSS and JavaScript with no build step. Because the GeoTIFFs are loaded by the browser, serve the folder through a local web server rather than opening the file directly:

```bash
python -m http.server 8000
```

Then open http://localhost:8000/index.htm.

## Notes for Reuse

- **Basemaps:** the street basemap uses the Mapbox v4 API (`mapbox.streets`), which Mapbox has retired, and the aerial basemap uses Bing Maps. Replace them with a current tile provider such as OpenStreetMap.
- **API keys:** a Bing Maps key is written directly in `index.htm`. Do not reuse it; use your own key, or remove the Bing layer.
- **Library versions:** the site uses Leaflet 1.2.0, jQuery 3.1.1 and Semantic UI. Newer versions may need code changes.
- **Repository size:** the five tile folders contain about 100,000 small PNG files.

## Technologies

- **Leaflet** — interactive web mapping.
- **geotiff.js / plotty** — reading and rendering GeoTIFFs in the browser.
- **Semantic UI and jQuery** — page layout and interface.
- **GDAL** — raster tiling.
- **GitHub Pages** — hosting.

## Author

Kushal KC
