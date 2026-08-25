# Structural variation

The source Notion page contained `delly.pl`, `lumpy.pl` and `manta.pl` wrapper
scripts. Their contents are not included here; the tool commands follow the
standard invocations below and should be adapted to the installed versions.

```bash
delly call -g REFERENCE.fasta -o delly.bcf SAMPLE.bam
lumpyexpress -B SAMPLE.bam -S split.bam -D disc.bam -o lumpy.vcf
manta --bam SAMPLE.bam --referenceFasta REFERENCE.fasta --runDir manta_dir
```
