# Processed ANA gauge series

Daily water-level series from ANA gauges in the Pantanal, classified into the
hydrological phases **Low**, **Rising**, **High**, and **Falling**.

The classification workflow is documented in the
[`hydrological_phase_analysis.ipynb`](../../../notebooks/hydrological_phase_classification/hydrological_phase_analysis.ipynb)
notebook.

## Contents

- `sem_nivelamento/`: 28 station files named `<station_code>.csv`.
  Columns: `data`, `wse`, and `phase`.
- `nivelamento/`: 17 station files named
  `<station_code>_nivelada.csv`. Columns: `data`, `wse`, `phase`,
  `wse_orto_local`, `h_elipsoidal`, `wse_swot_ref`,
  `param_H_orto_ZR`, `param_N_local`, and `param_N_egm2008`.

`wse_swot_ref` is the water-surface elevation referenced to the EGM2008
geoid using the tide-free convention and is the value used for comparison
with SWOT.

## Provenance

- Source dataset: processed ANA gauge series for the Pantanal river study.
- Original project location: `artigo_wse/dados_processados/ana/`.
- Repository release date: 2026-09-22.

The source-data provider's terms and citation requirements remain applicable;
the repository's software license does not replace third-party data terms.
