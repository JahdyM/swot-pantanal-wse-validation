# Data

Public, analysis-ready data are separated by study component:

```text
data/
├── river_analysis/
│   └── ana_processed/
│       ├── sem_nivelamento/
│       └── nivelamento/
└── lake_analysis/
```

- [`river_analysis/`](river_analysis/README.md) contains data used for the
  Pantanal river analyses.
- [`lake_analysis/`](lake_analysis/README.md) is reserved for lake-analysis
  datasets and their provenance documentation.

Only reviewed, explicitly released datasets belong here. Raw SWOT downloads,
credentials, sensitive station information, temporary processing files, and
large unreviewed products must remain outside version control.

The [river WSE pipeline](../notebooks/river_validation/README.md) uses local
`input/stations.gpkg` and `input/water_masks.gpkg` layers and writes downloads,
tables, maps, and checkpoints to `output/`. Both directories remain ignored
by Git.
