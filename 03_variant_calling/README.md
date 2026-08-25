# Variant calling and filtering

## GATK joint calling

Combine GVCFs:

```bash
gatk --java-options "-Xmx35g" CombineGVCFs \
  -R REFERENCE.fasta \
  -V SAMPLE1.g.vcf.gz \
  -V SAMPLE2.g.vcf.gz \
  -L CHR \
  -O CHR.raw.g.vcf.gz
```

Joint genotype:

```bash
gatk --java-options "-Xmx35g" GenotypeGVCFs \
  -R REFERENCE.fasta \
  -L CHR \
  --variant CHR.raw.g.vcf.gz \
  -O CHR.raw.vcf.gz
```

Select SNPs and indels:

```bash
gatk SelectVariants --select-type-to-include SNP \
  -R REFERENCE.fasta -V CHR.raw.vcf.gz -O CHR.raw.snp.vcf.gz
gatk SelectVariants --select-type-to-include INDEL \
  -R REFERENCE.fasta -V CHR.raw.vcf.gz -O CHR.raw.indel.vcf.gz
```

Hard filtering:

```bash
gatk VariantFiltration \
  -V CHR.raw.snp.vcf.gz \
  --filter-expression "QD < 2.0 || FS > 60.0 || MQ < 40.0 || SOR > 3.0 || MQRankSum < -12.5 || ReadPosRankSum < -8.0" \
  --filter-name "snp_filter" \
  -O CHR.flag.snp.vcf.gz

gatk SelectVariants --select-type-to-include SNP --exclude-filtered true \
  --restrict-alleles-to ALL -V CHR.flag.snp.vcf.gz -O CHR.allhard.snp.vcf.gz
gatk SelectVariants --select-type-to-include SNP --exclude-filtered true \
  --restrict-alleles-to BIALLELIC -V CHR.flag.snp.vcf.gz -O CHR.bialhard.snp.vcf.gz
```

MAF and missingness filter:

```bash
vcftools --gzvcf CHR.bialhard.snp.vcf.gz \
  --max-missing 0.9 --maf 0.05 --recode --recode-INFO-all --stdout |
  bgzip -c > CHR.maffiltered.snp.vcf.gz
```

Subset samples:

```bash
gatk SelectVariants -R REFERENCE.fasta -V CHR.maffiltered.snp.vcf.gz \
  --sample-name SAMPLE1 --sample-name SAMPLE2 \
  --exclude-filtered true --restrict-alleles-to BIALLELIC \
  -O CHR.subset.snp.vcf.gz
```

Index, validate and merge:

```bash
gatk IndexFeatureFile -F CHR.maffiltered.snp.vcf.gz
gatk ValidateVariants -V SAMPLE.g.vcf.gz -R REFERENCE.fasta -gvcf
gatk MergeVcfs --INPUT chr1.vcf.gz --INPUT chr2.vcf.gz -O merged.vcf.gz
gatk IndexFeatureFile --input merged.vcf.gz
```

Summarize and count:

```bash
gatk VariantsToTable -V input.vcf.gz \
  -F CHROM -F POS -F REF -F ALT -F HOM-REF -F HET -F HOM-VAR \
  -O input.vcf.gz.table
bcftools stats -s - input.vcf.gz > input.stats
```

## bcftools

Rename chromosomes:

```bash
bcftools annotate --rename-chrs rename.txt input.vcf.gz > input.renamed.vcf
```

Filter to target intervals:

```bash
bcftools filter -R regions.bed input.renamed.vcf.gz > input.filtered.vcf.gz
```

Generate stats:

```bash
bcftools stats input.filtered.vcf.gz > input.filtered.stats
```
