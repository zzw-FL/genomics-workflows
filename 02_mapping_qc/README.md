# Read mapping and QC

## Extract unmapped reads

```bash
samtools view -f 4 sample.bam > sample.unmapped.sam
```

Split unmapped reads into read1-only, read2-only and both-unmapped BAMs:

```bash
samtools view -u -f 4  -F 264 alignments.bam > unmap.tmps1.bam
samtools view -u -f 8  -F 260 alignments.bam > unmap.tmps2.bam
samtools view -u -f 12 -F 256 alignments.bam > unmap.tmps3.bam

samtools merge -u - tmps1.bam tmps2.bam tmps3.bam | samtools sort -n - unmapped

bamToFastq -bam unmapped.bam -fq1 unmapped_reads1.fastq -fq2 unmapped_reads2.fastq
```

## Coverage summaries with IGVTools

```bash
igvtools count -w 100000 SAMPLE.realigned.bam SAMPLE.realigned.bam.tdf REFERENCE.fasta
igvtools tdftobedgraph SAMPLE.realigned.bam.tdf SAMPLE.realigned.bam.bedgraph
less -S SAMPLE.realigned.bam.bedgraph | awk '$2!=$3' > SAMPLE.realigned.bam.Bedgraph
```
