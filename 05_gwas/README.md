# GWAS

## EMMAX association

```bash
emmax -v -d 10 \
  -t GENOTYPE_PREFIX \
  -p PHENOTYPE.txt \
  -k GENOTYPE_PREFIX.kinf \
  -c PCA_COVARIATES.txt \
  -o OUTPUT_PREFIX
```

## Manhattan and Q-Q plots

```r
library(qqman)

args <- commandArgs(trailingOnly = TRUE)
gwas.file <- args[1]
chromosome.file <- args[2]

chromosome <- read.table(chromosome.file, header = FALSE, as.is = TRUE)
colnames(chromosome) <- c("chr", "length")
chromosome.length <- cumsum(as.numeric(chromosome$length))

gwas.result <- read.table(text = gsub(":", "\t", readLines(gwas.file)), as.is = TRUE)
colnames(gwas.result) <- c("chr", "pos", "beta", "pval")
gwas.result$cum.pos <- gwas.result$pos

for (j in 2:nrow(chromosome)) {
  mt <- gwas.result$chr == paste0("chr", j)
  gwas.result[mt, "cum.pos"] <- gwas.result[mt, "pos"] + chromosome.length[j - 1]
}

chromosome.color <- rep(c(1, 8), length = nrow(chromosome))

png(paste0(gwas.file, ".png"), width = 1200, height = 300)
plot(gwas.result$cum.pos, -log10(gwas.result$pval),
     pch = 16, cex = 1, col = chromosome.color[as.numeric(sub("chr", "", gwas.result$chr))],
     bty = "n", xaxt = "n", yaxt = "n", xlab = "", ylab = expression(-log[10](italic(P))))
axis(2, las = 1)
abline(h = -log10(5e-2 / nrow(gwas.result)), col = 2, lty = 2)
dev.off()

png(paste0(gwas.file, ".qqplot.png"), width = 300, height = 300)
qq(gwas.result$pval, pch = 16, col = "black", cex = 1)
dev.off()
```
