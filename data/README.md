# Data instructions

Research data are intentionally excluded from version control. Do not commit raw downloads, sensitive station information, provider credentials, or large derived products.

The [river WSE pipeline](../notebooks/river_validation/README.md) uses two additional local input layers at the repository root: `input/stations.gpkg` and `input/water_masks.gpkg`. Its downloads, per-station tables, maps, and checkpoints go to `output/`. Both directories are ignored by Git and must be created locally by the researcher.

Use the following local directories:

- `raw/`: immutable original downloads from SWOT data services;
- `external/`: ANA gauge observations, DAHITI/HydroWeb altimetry, ICESat-2 ATL13, and other reference datasets;
- `interim/`: temporary or partially processed files; and
- `processed/`: analysis-ready datasets created by reproducible workflows.

For every dataset, keep a local provenance record containing at least the provider, product name and version, download date, spatial and temporal extent, access URL or identifier, license/terms, coordinate reference system, vertical datum, units, and processing history. Store shareable provenance templates or inventories in `docs/`, without listing sensitive credentials or restricted locations.

Suggested local layout:

```text
data/
├── raw/
│   └── swot/{pixc,raster,riversp,lakesp}/
├── external/
│   ├── ana/
│   ├── dahiti/
│   ├── hydroweb/
│   └── icesat2_atl13/
├── interim/
└── processed/
```

The repository's `.gitignore` blocks common geospatial, scientific, tabular, and archive formats as an additional safeguard. If a small derived dataset is appropriate for public release, document its provenance and licensing, then add it deliberately after review.
