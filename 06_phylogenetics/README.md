# Phylogenetics

## Align, convert, trim and infer a tree

```bash
mafft --maxiterate 1000 --localpair --anysymbol --thread 10 input.pep > input.pep.msa
perl Fasta2Phylip.pl input.pep.msa
perl trim_phy.pl input.pep.msa.phy 50
iqtree2 -s input.pep.msa.phy.trim.phy -b 500 -nt 10
```

The helper scripts `Fasta2Phylip.pl` and `trim_phy.pl` were attached to the
source Notion page and are not included in this repository.
