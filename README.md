# SWOT Mission Data Validation for Pantanal Hydrodynamic Analysis

[![Status: research scaffold](https://img.shields.io/badge/status-research%20scaffold-6f42c1)](#project-status)
[![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?logo=python&logoColor=white)](environment.yml)

Reproducible analysis repository for the master's dissertation **“Surface Water and Ocean Topography (SWOT) Mission Data Validation for Pantanal Hydrodinamic Analysis”**, by **Jahdy Moreno Oliveira**, National Institute for Space Research (INPE), 2026.

## Project status

This repository is an initial scaffold. Analysis code, notebooks, derived products, and dissertation figures will be added as the research is prepared for release. The directory structure is preserved with `.gitkeep` files so that contributors can place future work in a consistent location.

## Research scope

The project is organized around:

- river water-surface-elevation validation;
- lake water-surface-elevation validation;
- hydrological phase classification;
- absolute validation against independent reference measurements;
- dynamic validation of temporal water-level variations;
- analyses of potential error drivers; and
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
│   ├── river_validation/
│   ├── lake_validation/
│   ├── hydrological_phase_classification/
│   ├── absolute_validation/
│   ├── dynamic_validation/
│   └── error_driver_analyses/
├── outputs/                        # Generated tables and other results
├── src/swot_pantanal/              # Reusable Python source code
│   ├── river_validation/
│   ├── lake_validation/
│   ├── hydrological_phase_classification/
│   ├── absolute_validation/
│   ├── dynamic_validation/
│   └── error_driver_analyses/
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

## Planned workflow

1. Create the software environment.
2. Acquire source data from the official providers.
3. Record provenance and place local files under the appropriate `data/` subdirectory.
4. Add reusable processing and validation functions under `src/swot_pantanal/`.
5. Use notebooks for documented analyses and quality checks.
6. Export final, shareable graphics to `figures/` and other generated results to `outputs/`.

## Environment

The supplied environment is intentionally minimal while the analysis code is being assembled:

```bash
conda env create -f environment.yml
conda activate swot-pantanal-wse-validation
```

Add dependencies to `environment.yml` as workflows are introduced. Pin exact versions before producing the dissertation's archival release.

## Reproducibility conventions

- Keep reusable logic in `src/`; use notebooks for analysis narratives and exploration.
- Use relative paths or configuration files; do not hard-code local machine paths.
- Preserve coordinate reference system, vertical datum, units, acquisition time, quality flags, and provider identifiers in derived-data metadata.
- Record all filtering, spatial matching, temporal matching, and uncertainty assumptions.
- Fix random seeds where applicable and document the software environment used for final results.

## Citation

If you use this repository, cite it using the metadata in [`CITATION.cff`](CITATION.cff). Citation details may be updated when the dissertation and an archived software release are publicly available.

## License

No software license has been selected yet. Unless a license is added, the repository contents remain under the copyright holder's default rights. Third-party data retain their original licenses and terms of use.

## Author

**Jahdy Moreno Oliveira**  
National Institute for Space Research (INPE)  
Master's dissertation, 2026
