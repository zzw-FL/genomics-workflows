# Visualization

## Circos/circlize

```r
library(circlize)
library(ComplexHeatmap)

sv <- read.delim("sample.cis", header = FALSE, stringsAsFactors = FALSE)
gene <- read.delim("gene2.list", header = FALSE, stringsAsFactors = FALSE)

circos.par("start.degree" = 90)
circos.initialize(factors = paste0("Chr", 1:10), xlim = CHROMOSOME_LENGTHS)
circos.track(ylim = c(0, 1), panel.fun = function(x, y) {
  circos.text(CELL_META$xlim[2] / 2, 0.5, CELL_META$sector.index, cex = 1)
}, bg.col = "grey90", track.height = 0.1)
circos.trackHist(gene$V1, gene$V2, col = "#d73027", bin.size = 1000000)
circos.clear()
```

## SNP density

```bash
vcftools --gzvcf input.vcf.gz --SNPdensity 10000 --out input.density
```
