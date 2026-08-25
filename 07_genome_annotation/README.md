# Genome annotation

## BRAKER3

```bash
source <PATH>/conda_env/bin/activate braker

braker.pl \
  --genome=./GENOME.soft-masked.fa \
  --prot_seq=<PATH>/homology.pep \
  --bam=SAMPLE1.bam,SAMPLE2.bam \
  --workingdir=./braker3 \
  --threads 48 \
  --gff3 \
  --AUGUSTUS_CONFIG_PATH=<PATH>/augustus/config/ \
  --AUGUSTUS_BIN_PATH=<PATH>/augustus/bin/ \
  --AUGUSTUS_SCRIPTS_PATH=<PATH>/augustus/scripts/ \
  --PROTHINT_PATH=<PATH>/ProtHint/bin \
  --PYTHON3_PATH=<PATH>/python/bin/ \
  --BAMTOOLS_PATH=<PATH>/bamtools/bin \
  --SAMTOOLS_PATH=<PATH>/samtools/bin \
  --DIAMOND_PATH=<PATH>/diamond \
  --BLAST_PATH=<PATH>/blast/bin \
  --MAKEHUB_PATH=<PATH>/makehub \
  --CDBTOOLS_PATH=<PATH>/cdbtools \
  --GENEMARK_PATH=<PATH>/ETP/bin \
  &> braker3.log
```
