# Hydrological Phase Analysis of Gauge Series

Classifies every day of one or more **daily water-level (stage) series** into a
hydrological phase — **Low, Rising, High or Falling** — and produces
publication-ready figures and tables from the result. It was developed for the
Brazilian ANA stream gauges of the Pantanal, but it works with any set of daily
gauge series.

## What's in this folder

A single, self-contained Jupyter notebook —
[`hydrological_phase_analysis.ipynb`](./hydrological_phase_analysis.ipynb) —
organised in numbered blocks (configuration, data loading, classification,
statistics, three figure blocks, run). Given a folder of gauge CSV files (and,
optionally, a station metadata table) it produces:

| Output | Description |
|---|---|
| **Phase tables** | One CSV per station with `date, level, phase` for every observation. |
| **Figure 1 — river panels** | One PNG per river. Each station shows the complete series coloured by phase, a monthly phase heat-map, the recent period and the seasonality (monthly percentiles). |
| **Figure 2 — seasonality** | One compact PNG + PDF with the seasonal cycle of every station, grouped by river; each month is coloured by its seasonal phase. |
| **Figure 3 — station cards** | One PNG + PDF per river with a summary card per station (period, mean annual amplitude, observations, data gaps), plus a CSV with the same numbers. |

## Requirements

- Python with `pandas`, `numpy` and `matplotlib` (plus `jupyter` to run the notebook).
- Developed and tested with Python 3.14, pandas 2.3, numpy 2.4 and matplotlib 3.10.

## Input data

**Gauge series** — a folder (`input/gauge_series/` at the repository root by
default) with one CSV per station, named `<station code>.csv`, containing a
date column and a water-level column:

```csv
date,level
1965-12-13,2.50
1965-12-14,2.58
...
```

Column names are configurable (`date_col`, `level_cols`; dates can also come from
separate day/month/year columns via `date_parts_cols`). Rows with an invalid
date or level are dropped and duplicated dates keep their first occurrence. The
series are expected to be **daily**.

**Station metadata** *(optional)* — a CSV with one row per station
(`input/station_metadata.csv` by default):

```csv
code,river,region,name
66070004,Paraguay River,Northern,Caceres
...
```

- `code` must match the CSV file names; `river` groups stations into figures;
  `region` is a short tag printed next to the station code; `name` is informational.
- Without metadata, all stations are grouped as "Unknown" and Figures 2 and 3 are skipped.
- Figures 2 and 3 include only the stations listed in the metadata table.

## How to run

1. Put your files under `input/` at the repository root (or point `CFG` at wherever they are).
2. Open the notebook from within the repository. It locates the repository root automatically.
3. Edit `CFG` in **Block 0** if your paths or column names differ.
4. *Run All*. Individual outputs can be switched off with the `RUN_*` flags in **Block 7**.

## Configuration

All settings are fields of the `Config` dataclass in Block 0. The ones you are most likely to change:

| Field | Default | Meaning |
|---|---|---|
| `input_dir`, `metadata_path` | `input/gauge_series`, `input/station_metadata.csv` | Where the data is. |
| `date_col`, `level_cols` | `date`, `(level, stage, cota, Cota, wse)` | Column names in the gauge CSVs (first level column found is used). |
| `meta_code_col`, `meta_river_col`, `meta_region_col`, `meta_name_col` | `code`, `river`, `region`, `name` | Column names in the metadata table. |
| `output_dir` | `output` | Root folder for figures and summary tables. |
| `phase_tables_dir` | `output/phase_tables` | Where the per-station phase tables go. Set it to `input_dir` to overwrite the input CSVs in place. |
| `phase_table_cols` | `(date, level, phase)` | Column names of the phase tables. |
| `gap_days` | `15` | Gaps up to this many days are interpolated; longer ones are reported as "large gaps". |
| `trend_window_days` | `30` | Rolling-mean window used to smooth the level. |
| `low_quantile`, `high_quantile` | `0.25`, `0.75` | Thresholds for Low / High within each hydrological year. |
| `hydro_year_start_month` | `None` | `None` = month with the lowest mean level; or set 1-12. |
| `recent_start` | `2023-01-01` | Start of the "recent period" panels. |
| `river_style_overrides` | Pantanal presets | Per-river figure sizing (see below). |

## Output layout

```
output/
├── phase_tables/                          <code>.csv  (date, level, phase)
├── hydrological_analysis_by_river/        hydrological_analysis_<dataset>_<river>_panel.png
├── seasonality_by_river/                  seasonality_all_rivers_compact.png / .pdf
└── station_summaries_by_river/            station_summary_<river>.png / .pdf
                                           station_summary_by_river_values.csv
```

## Method

1. **Gap filling** — gaps of up to `gap_days` days are linearly interpolated; longer gaps stay empty.
2. **Hydrological year** — starts in the calendar month with the lowest mean level, so each flood pulse falls inside one hydrological year.
3. **Thresholds** — within each hydrological year, the 25th and 75th percentiles of the level are the *Low* and *High* thresholds.
4. **Trend** — the level is smoothed with a centred 30-day rolling mean.
5. **Labels**, applied in this order to each day: trend ≥ high threshold → **High**; trend ≤ low threshold → **Low**; trend higher than the previous day → **Rising**; otherwise → **Falling**.

For the seasonality figure, each calendar month is coloured by its seasonal phase: the three consecutive months with the highest summed monthly median (windows containing the peak month) form the *High* season, and the cycle continues *High → Falling → Low → Rising*, three months each.

## Figure sizing

Figure 1 is sized to fit an A4 page (`river_panel_width_in`); the height and font
sizes adapt to the number of stations on the river. `river_style_overrides`
holds hand-tuned presets for specific Pantanal rivers (matched on the river name).
For other basins pass `river_style_overrides=()` to use only the station-count presets.

## Reproducing the original Pantanal / ANA figures

The figures in the original analysis were produced from Brazilian ANA tables with
Portuguese column names, overwriting the input CSVs with the phase column. The same
run is obtained with:

```python
CFG = Config(
    input_dir=Path("dados_processados/ana/sem_nivelamento"),
    metadata_path=Path("analises/tabelas/ana_metadados.csv"),
    date_col="data",
    meta_code_col="Código", meta_river_col="Rio",
    meta_region_col="Pantanal", meta_name_col="Nome",
    output_dir=Path("figuras"),
    phase_tables_dir=Path("dados_processados/ana/sem_nivelamento"),  # in place
    phase_table_cols=("data", "wse", "phase"),
    dataset_label="sem_nivelamento",
)
```

## Notes

- **Reproducibility across library versions.** The phase rules compare floating-point
  values against thresholds (`trend >= high`, `trend > previous day`). A day whose trend
  falls *exactly* on a threshold can be labelled differently by different pandas/numpy
  versions because of differences of the order of 1e-16 (in one 19,193-day test series,
  5 days differed). Pin your library versions if you need bit-for-bit reproducibility.
- **Overwriting inputs.** With `phase_tables_dir = input_dir` the input files are replaced by
  files containing only the three phase-table columns. Keep a backup of the originals.

## Citation

If you use this code, please cite it — see [`CITATION.cff`](./CITATION.cff).

## License

MIT — see [`LICENSE`](./LICENSE). You are free to use, modify and
redistribute this code, including for commercial purposes, as long as the
copyright notice is kept and the work is cited (see Citation above).
