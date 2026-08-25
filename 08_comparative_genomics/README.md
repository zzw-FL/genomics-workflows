# Comparative genomics

## Whole-genome alignment with MUMmer

```bash
nucmer --prefix=PREFIX REFERENCE.fa QUERY.fa -t 2
delta-filter -1 -i 70 -l 100 PREFIX.delta > PREFIX.filter.1.delta
show-coords -r -c -l PREFIX.filter.1.delta > PREFIX.1.coords
```

## Reduce overlapping intervals with R

```r
library(GenomicRanges)
all <- read.table("filter.1.list", as.is = TRUE)
grall <- with(all, GRanges(V1, IRanges(V2, V3)))
grall <- reduce(grall, min.gapwidth = 0)
write.table(grall, file = "filter.1.list_region",
            row.names = FALSE, col.names = FALSE, quote = FALSE)
```

## BLAST

```bash
makeblastdb -in reference.cds.fa -dbtype nucl -input_type fasta -out reference.cds
blastn -query query.contig -out query.out \
  -db reference.cds -outfmt 6 -evalue 1e-5
```

## BEDTools

```bash
bedtools intersect -a A.bed -b B.bed
bedtools intersect -a A.bed -b B.bed -wa -wb
bedtools subtract -a A.bed -b B.bed > C.bed
```
