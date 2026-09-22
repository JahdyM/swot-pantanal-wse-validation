# SWOT Mission Data Validation for Pantanal Hydrodynamic Analysis

[![Status: research scaffold](https://img.shields.io/badge/status-research%20scaffold-6f42c1)](#project-status)
[![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?logo=python&logoColor=white)](environment.yml)

Reproducible analysis repository for the master's dissertation **“Surface Water and Ocean Topography (SWOT) Mission Data Validation for Pantanal Hydrodinamic Analysis”**, by **Jahdy Moreno Oliveira**, National Institute for Space Research (INPE), 2026.

## Project status

This repository is being populated with documented analysis code. It currently includes the [SWOT multi-product WSE download and preprocessing pipeline](notebooks/river_validation/swot_pantanal_wse_pipeline.ipynb) and the [hydrological phase classification analysis](notebooks/hydrological_phase_classification/hydrological_phase_analysis.ipynb) associated with a Pantanal Rivers manuscript submitted to *Science of Remote Sensing*. Other workflows, derived products, and dissertation figures will be added as they are prepared for release. Empty directories are preserved with `.gitkeep` files.

### Associated manuscript

Moreno-Oliveira, J., Fassoni-Andrade, A., Trigg, M. A., Moreira, D. M., & Novo, E. M. L. de M. *Multi-product validation of SWOT water surface elevation in the Pantanal Rivers*. Manuscript submitted to *Science of Remote Sensing*; in peer review.

The [river-validation notebook guide](notebooks/river_validation/README.md) covers inputs, authentication, the four SWOT product streams, resumable processing, and outputs. The [hydrological phase analysis guide](notebooks/hydrological_phase_classification/README.md) documents daily gauge-series classification and its publication-ready figures. Code author: **Jahdy Moreno-Oliveira**.

## Research scope

The project is organized around:

- river water-surface-elevation analysis, including absolute validation,
  dynamic validation, and analyses of potential error drivers;
- lake water-surface-elevation validation;
- hydrological phase classification; and
- reproducible generation of figures and other outputs.

## Repository structure

```text
.
├── config/                         # Non-sensitive analysis configuration
├── data/
│   ├── raw/                        # Original source data (not versioned)
│   ├── external/                   # Third-party reference data (not versioned)
│   ├── interim/                    # Intermediate products (not versioned)
│   └── processed/                  # Analysis-ready data (not versioned)
├── docs/                           # Methods and supporting documentation
├── figures/                        # Versionable, publication-ready figures
├── notebooks/
│   ├── river_validation/          # Article-linked download/preprocessing notebook
│   ├── lake_validation/
│   └── hydrological_phase_classification/ # Gauge-series phase classification
├── outputs/                        # Generated tables and other results
├── src/swot_pantanal/              # Reusable Python source code
│   ├── river_validation/
│   ├── lake_validation/
│   └── hydrological_phase_classification/
└── tests/                          # Automated tests
```

## Data sources

The dissertation draws on the following high-level data sources:

- SWOT Level 2 High-Rate products: **PIXC**, **Raster**, **RiverSP**, and **LakeSP**;
- in situ gauge observations from Brazil's **Agência Nacional de Águas e Saneamento Básico (ANA)**;
- conventional satellite-altimetry time series from **DAHITI** and **HydroWeb**; and
- **ICESat-2 ATL13** inland-water surface-height data for lake analyses.

### Data availability and handling

**Raw data are not included in this repository.** Source archives may be large, subject to provider terms, or contain station/location information that should be reviewed before sharing. Obtain data directly from the relevant providers and follow their current licenses, access rules, and citation requirements.

See [`data/README.md`](data/README.md) for the expected local layout. The `.gitignore` rules exclude research data by default while retaining directory documentation. Before every commit, confirm that no credentials, access tokens, sensitive coordinates, restricted records, or large binary files have been staged.

For the river pipeline specifically, local station and water-mask layers belong under `input/`, and generated downloads and results go under `output/`; both are ignored by Git.

## Planned workflow

1. Create the software environment.
2. Acquire source data from the official providers.
3. Record provenance and place local files under the appropriate `data/` subdirectory.
4. Add reusable processing and validation functions under `src/swot_pantanal/`.
5. Use notebooks for documented analyses and quality checks.
6. Export final, shareable graphics to `figures/` and other generated results to `outputs/`.

## Environment

The supplied environment includes the Python and geospatial dependencies for the published notebook:

```bash
conda env create -f environment.yml
conda activate swot-pantanal-wse-validation
```

The Raster stage also requires `gdalmdimtranslate` on `PATH` (provided by the `gdal` package in this environment). Pin exact versions before producing an archival release. A full run additionally requires NASA Earthdata access and local station/mask files, which are not included here.

## Reproducibility conventions

- Keep reusable logic in `src/`; use notebooks for analysis narratives and exploration.
- Use relative paths or configuration files; do not hard-code local machine paths.
- Preserve coordinate reference system, vertical datum, units, acquisition time, quality flags, and provider identifiers in derived-data metadata.
- Record all filtering, spatial matching, temporal matching, and uncertainty assumptions.
- Fix random seeds where applicable and document the software environment used for final results.

## Citation

If you use this repository, cite the associated manuscript and software using the metadata in [`CITATION.cff`](CITATION.cff). Publication details will be updated when the manuscript is accepted.

## License

The code is released under the [MIT License](LICENSE), with an additional request to cite the work in academic or other published outputs as described in [`CITATION.cff`](CITATION.cff). Third-party data retain their original licenses and terms of use.

## Author

**Jahdy Moreno Oliveira**  
National Institute for Space Research (INPE)  
Master's dissertation, 2026
