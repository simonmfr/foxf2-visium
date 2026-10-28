# Visium Foxf2 analysis

Spatial transcriptomics (10x Visium) of Foxf2iKO vs Ctrl mouse brains (n=4 each, one coronal section per mouse).

## Setup
- Space Ranger 2.0.0 output (mm10-2020-A): `/dss/dssfs03/pn52re/pn52re-dss-0000/2021-FOXF2/visium/spaceranger/sample<id>/outs`
- Data: `/dss/dssfs03/pn52re/pn52re-dss-0001/FOXF2/Visium`
  - `adatas/`: all h5ad objects: Saunders et al. 2018 reference (`saunders_merged.h5ad`), per-sample and integrated Visium
  - `geo/`: GEO h5ad (`Visium_Foxf2iKO_Ctrl.h5ad`), input for 04-05
  - `dea/`, `plots/`: outputs
- Packages: scanpy, squidpy, tangram-sc, harmonypy, scikit-learn, seaborn, openpyxl, papermill

## Notebooks
- `00_sc_reference`: scRNA-seq reference QC, normalization and cell type annotation
- `01_preprocess_sample`: per-sample QC, normalization, Tangram mapping (cluster mode), cell type probability maps
- `02_integrate`: merge, joint normalization, Harmony, Leiden, brain region annotation
- `03_geo_export`: GEO h5ad
- `04_dea`: Foxf2iKO vs Ctrl wilcoxon DEA across full brain and per region, in all spots and neuronal spots
- `05_neuron_prob`: Foxf2iKO vs Ctrl DEA in spots with high vs low neuron probability, correlation with neuron probability

04-05 only need `geo/Visium_Foxf2iKO_Ctrl.h5ad`.

```bash
for s in 733 734 735 737 738 739 740 741; do papermill 01_preprocess_sample.ipynb /tmp/01_$s.ipynb -r sample $s && rm /tmp/01_$s.ipynb; done
for nb in 03_geo_export 04_dea 05_neuron_prob; do papermill $nb.ipynb $nb.ipynb; done
```

## Note
Leiden (00, 02) is not reproducible across machines/versions. Annotations therefore refer to the saved paper objects: `adatas/20220823_saunders_annotated.h5ad` (00), `adatas/20220923_intermediate_umap_save_integrated.h5ad` (02). Its annotated version, `adatas/20240104_integrated_annotated.h5ad`, is the input for 03.
