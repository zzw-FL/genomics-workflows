# Population genetics

## Convert VCF to PLINK formats

```bash
plink --vcf input.vcf.gz --allow-extra-chr --double-id \
  --set-missing-var-ids @:# --make-bed --out PREFIX
plink --bfile PREFIX --allow-extra-chr --recode12 \
  --output-missing-genotype 0 --transpose --out PREFIX
```

## Kinship and PCA

```bash
emmax-kin -v -h -d 10 PREFIX

gcta64 --bfile PREFIX --make-grm --out PREFIX.pca
gcta64 --grm PREFIX.pca --pca 10 --out PREFIX.pca
```

## NJ tree

```bash
plink --bfile PREFIX --distance-matrix --out PREFIX
# Build a PHYLIP distance matrix and infer a neighbor-joining tree
fneighbor -datafile individual.datamatrix -outfile tree.txt \
  -matrixtype s -treetype n -outtreefile tree.nwk
```

## Pi and FST

```bash
vcftools --gzvcf input.vcf.gz \
  --out output_100kb \
  --window-pi 100000 --window-pi-step 100000

vcftools --gzvcf input.vcf.gz \
  --out output_fst \
  --weir-fst-pop pop1.txt --weir-fst-pop pop2.txt \
  --fst-window-size 100000 --fst-window-step 100000
```

## ADMIXTURE and CLUMPP

Run ADMIXTURE:

```bash
admixture --cv PREFIX K -j4 -s REPEAT | tee log.admixture.kK.repeat.out
```

Generate CLUMPP parameter files and submit:

```perl
#!/usr/bin/perl -w
use strict;
my $cluster = shift;
my $individual_number = shift;
my $repeat_count = shift;
my $name = shift;
my $clumpp = '<PATH>/CLUMPP';
open OUT, ">$name\_k$cluster.paramfile";
print OUT "DATATYPE 0\n\n";
print OUT "INDFILE $name\_k$cluster.indfile\n\n";
print OUT "OUTFILE $name\_k$cluster.outfile\n\n";
print OUT "MISCFILE $name\_k$cluster.miscfile\n\n";
print OUT "K $cluster\n\n";
print OUT "C $individual_number\n\n";
print OUT "R $repeat_count\n\n";
print OUT "M 3\n\n";
print OUT "S 2\n\n";
print OUT "GREEDY_OPTION 2\n\n";
print OUT "REPEATS 100\n\n";
print OUT "PRINT_PERMUTED_DATA 1\n\n";
close OUT;
```

Plot cross-validation error with R:

```r
library(ggplot2)
x <- read.table(file = "CV.txt", header = TRUE)
x$K <- as.factor(x$K)
p <- ggplot(x, aes(x = K, y = CV)) +
  stat_boxplot(geom = "errorbar", width = 0.5) +
  geom_boxplot() +
  xlab("K") +
  ylab("Cross-validation error") +
  theme_classic()
ggsave("cv.png", p, width = 8, height = 6, dpi = 300)
```

## Linkage disequilibrium decay

```bash
perl PopLDdecay.pl -input vcf.list -maxdis 500 -outDir output -project PROJECT -mem 1 -maf 0
```
