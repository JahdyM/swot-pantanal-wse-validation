# SWOT Multi-Product WSE Pipeline — Pantanal Rivers

Code used in:

> Moreno-Oliveira, J., Fassoni-Andrade, A., Trigg, M. A., Moreira, D. M., & Novo,
> E. M. L. de M. *Multi-product validation of SWOT water surface elevation in
> the Pantanal Rivers*. Manuscript submitted to *Science of Remote Sensing*;
> in peer review.
>
> a. National Institute for Space Research (INPE), São José dos Campos, Brazil
> b. Institute of Geosciences, University of Brasília (UnB), Brasília, Brazil
> c. School of Civil Engineering, University of Leeds, Leeds, United Kingdom
> d. Geological Survey of Brazil, Rio de Janeiro, Brazil

Code author: **Jahdy Moreno-Oliveira**.

## What's in this folder

A single, self-contained Jupyter notebook —
[`swot_pantanal_wse_pipeline.ipynb`](./swot_pantanal_wse_pipeline.ipynb) —
that, for a set of in-situ gauging stations, downloads and processes all four
SWOT Level-2 hydrology products (PIXC, Raster, Node, Reach), corrects the
Reach product's water surface elevation (WSE) for the offset between the
reach center and the gauge location, cross-checks completeness across the
four products (recovering any date that produced valid data in one product
but is missing from another), and de-duplicates the final tables. See the
notebook's own first cell for the full block-by-block description, the
processing order, and the output folder layout.

For PIXC and Raster, the reported WSE per overpass is not an aggregate
statistic averaged (or independently computed) across pixels/cells — it is
the complete set of attributes from the ONE real pixel/cell whose WSE sits at
the median rank for that overpass (exact rank, never interpolated). This
keeps every reported row physically self-consistent (height, tides, sig0,
quality flags, ... all from the same observation), the same way the Node and
Reach products already report one river feature's own attributes rather than
a mix drawn from several.

## Requirements

- Python ≥ 3.10, with `geopandas`, `pandas`, `numpy`, `xarray`, `rioxarray`,
  `earthaccess`, `shapely`, `pyproj`, `matplotlib`, and (for the Raster
  product) a working GDAL install with the `gdalmdimtranslate`
  command-line tool on `PATH`.
- A NASA Earthdata account configured for `earthaccess` — create
  `~/.netrc` with your Earthdata credentials (see
  [earthaccess's authentication docs](https://earthaccess.readthedocs.io/en/latest/howto/authenticate/)).
  The notebook authenticates strictly from `~/.netrc` and never falls back
  to an interactive login prompt, so it runs unattended end to end.

## Setup

Provide two input vector files under `input/` at the repository root (paths
are configurable in the notebook's config cell):

- `input/stations.gpkg` — one point per gauging station, with a station-code
  field (`Codigo` by default, configurable via `STATION_CODE_FIELD`).
- `input/water_masks.gpkg` — one polygon per station, delineating the water
  surface to sample (river channel / lake extent around the gauge). This is
  what actually constrains which SWOT pixels/nodes/reaches "belong" to a
  given station. If a station has no matching polygon here, the pipeline
  falls back to a circular buffer around its point (`DEFAULT_SEARCH_BUFFER_KM`).

## How to run

Open the notebook from within the repository and run all cells. The notebook
locates the repository root automatically. Every processing stage is gated by a
boolean flag in the last cell (`RUN_DOWNLOAD_AND_PROCESS`,
`RUN_COMPLETENESS_CHECK`, `RUN_DUPLICATE_AUDIT`, `RUN_WSE_CORRECTION`), and
`STATION_CODES` lets you restrict a run to a subset of stations (`None`
processes every station in `input/stations.gpkg`). Nothing in the notebook
reads from stdin, so "Run All" always completes unattended.

The pipeline is resumable by construction: every stage checks what already
exists under `OUTPUT_DIR` before doing any work and logs what it found, so
re-running it after an interruption — partial or complete — never
re-downloads or re-processes a granule/date that was already handled.

## Output

Results are written under `output/Station_<code>/` at the repository root — one CSV per product
(`pixc.csv`, `raster.csv`, `node.csv`, `reach.csv`), QC maps, and a set of
checkpoint/cache files that make the pipeline resumable. See the notebook's
first cell for the complete folder-tree description.

## Citation

If you use this code, please cite the manuscript above. The repository's
[`CITATION.cff`](../../CITATION.cff) contains both the software metadata and
the preferred manuscript citation.

## License

MIT — see [`LICENSE`](../../LICENSE). You are free to use, modify, and
redistribute this code, including for commercial purposes, provided that the
copyright notice is retained. For academic or other published work, please
also cite it as described above.
