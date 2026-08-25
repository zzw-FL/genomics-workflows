# Notion analysis workflow library

This repository contains the genomics and bioinformatics workflows recovered
from the Notion workspace project named `脚本`. The commands are grouped by
functional domain, not by the original project that produced them.

## Important notes

- Original absolute HPC paths, user names, project identifiers and internal
  sample names have been replaced with placeholders such as `<TOOL_PATH>`,
  `<DATA_PATH>`, `<REFERENCE>`, `<SAMPLE>` and `<PROJECT>`.
- Raw data, sample metadata, attachment files and publication-specific figures
  are not included. Where the original Notion page contained an attachment, the
  filename is listed but its content is omitted.
- Review each command before reuse; paths and resource requests are examples
  from the original environment and must be adapted.

## Categories

| Directory | Function |
| --- | --- |
| `01_genome_assembly/` | HiFi, ONT, Hi-C and organelle assembly |
| `02_mapping_qc/` | Read mapping, extraction of unmapped reads, coverage summaries |
| `03_variant_calling/` | GATK and bcftools variant calling, filtering and QC |
| `04_population_genetics/` | PCA, kinship, NJ tree, pi/FST, ADMIXTURE, CLUMPP, LD |
| `05_gwas/` | EMMAX association and Manhattan/Q-Q plotting |
| `06_phylogenetics/` | MAFFT, alignment conversion, trimming and IQ-TREE |
| `07_genome_annotation/` | BRAKER3 and structural annotation |
| `08_comparative_genomics/` | MUMmer, BLAST, BEDTools and interval reduction |
| `09_transcriptome_expression/` | RSEM and transcript expression |
| `10_structural_variation/` | Delly, Lumpy and Manta wrappers |
| `11_visualization/` | Circos and SNP-density plots |

## License

See [LICENSE](LICENSE).
