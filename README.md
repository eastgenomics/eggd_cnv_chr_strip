# eggd_cnv_chr_strip

Final stage of the [`eggd_atlas_cnv`](https://github.com/eastgenomics/eggd_atlas_cnv) somatic CNV
workflow. Writes Ensembl-named (`*.nochr.*`) copies of the CNV text files for downstream apps that
expect `1..22,X,Y,MT` chromosome naming, without modifying the chr-prefixed originals.

## What it does
For each supplied CNV file, rewrites **only the `chromosome` column** (`chr1→1`, `chrX→X`,
`chrM`/`chrMT`→`MT`; already-Ensembl values unchanged) via the bundled `atlas/chr_strip.py`, a
line-preserving rewrite (not a CSV round-trip). A header-only input yields a header-only output.

## Inputs

`sample_id` is required; all six CNV file inputs are optional (the
[`eggd_atlas_cnv`](https://github.com/eastgenomics/eggd_atlas_cnv) workflow links all six
automatically):

| Input | Source app | File type |
|---|---|---|
| `sample_id` | — | Sample identifier used as the output file stem |
| `cnvkit_cnr` | eggd_cnvkit | Copy-number ratio file (`.cnr`) |
| `cnvkit_cns` | eggd_cnvkit | Copy-number segments file (`.cns`) |
| `cnvkit_call_cns` | eggd_cnvkit | Called segments file (`.call.cns`) |
| `cnvkit_genemetrics` | eggd_cnvkit | Gene-level copy-number metrics (`.genemetrics.tsv`) |
| `purple_cnv_somatic` | eggd_purple | PURPLE somatic copy-number segments (`.purple.cnv.somatic.tsv`) |
| `purple_cnv_gene` | eggd_purple | PURPLE gene-level copy-number estimates (`.purple.cnv.gene.tsv`) |

## Outputs
`{sample}.nochr.cnr`, `.nochr.cns`, `.nochr.call.cns`, `.nochr.genemetrics.tsv`,
`{sample}.purple.cnv.somatic.nochr.tsv`, `{sample}.purple.cnv.gene.nochr.tsv`.

## Notes
- Never modifies its inputs (CNVkit originals stay in the batch outputs; PURPLE originals in `purple_tar`).
- The IGV plotter deliberately keeps the **chr-prefixed** files (igv.js/hg38) — it does not use these outputs.
- Instance `mem1_ssd1_v2_x2`; timeout 1 h. No `execDepends` beyond the system `python3`.
