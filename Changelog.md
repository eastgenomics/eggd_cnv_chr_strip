# Changelog

## 1.0.0
Initial release. New app (not converted from an applet) written for the `eggd_atlas_cnv` somatic CNV
workflow. Writes Ensembl-named (`*.nochr.*`) copies of the CNV text files for downstream apps that
expect `1..22,X,Y,MT` naming, **retaining the chr-prefixed originals**. Strips the `chr` prefix from
the `chromosome` column only (`chrM`/`chrMT`→`MT`), leaving gene names and every other column
untouched. Delegates the transform to the bundled, unit-tested pure-Python `atlas/chr_strip.py`.
