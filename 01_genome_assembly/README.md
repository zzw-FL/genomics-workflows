# Genome assembly

## HiFi + Hi-C assembly with hifiasm

```bash
hifiasm -o ASSEMBLY_PREFIX \
  --h1 <PATH>/hic_read1.fastq.gz \
  --h2 <PATH>/hic_read2.fastq.gz \
  -t 24 \
  <PATH>/hifi_reads.fastq.gz
```

For polyploid or multi-haplotype samples:

```bash
hifiasm -o ASSEMBLY_PREFIX \
  --n-hap 4 \
  --h1 <PATH>/hic_read1.fastq.gz \
  --h2 <PATH>/hic_read2.fastq.gz \
  -t 24 \
  <PATH>/hifi_reads.fastq.gz
```

Convert GFA to FASTA:

```bash
awk -v id="$1" '/^S/{print ">"id"_"$2"\n"$3}' "$1"
```

## ONT assembly with NextDenovo

Create `run.cfg`:

```ini
[General]
job_type = local
job_prefix = SAMPLE_PREFIX
task = all
rewrite = yes
deltmp = yes
rerun = 3
parallel_jobs = 4
input_type = raw
read_type = ont
input_fofn = ./input.fofn
workdir = ./01.rundir

[correct_option]
read_cutoff = 1k
genome_size = 2848M
seed_cutoff = 32582
blocksize = 5g
pa_correction = 4
seed_cutfiles = 4
sort_options = -m 20g -t 8 -k 40
minimap2_options_raw = -x ava-ont -t 8
correction_options = -p 8

[assemble_option]
random_round = 20
minimap2_options_cns = -x ava-ont -t 8 -k17 -w17
nextgraph_options = -a 1
```

Run:

```bash
python <PATH>/nextDenovo run.cfg
```

## Plastid assembly with NOVOPlasty

```bash
perl <PATH>/NOVOPlasty.pl -c config.txt
```

## Long-read scaffolding with SOAPdenovo-LR

```bash
<PATH>/SOAPdenovo_LR-127mer sparse_pregraph -s lib.cfg -d 2 -K 79 -R -o PREFIX -p 32 -z 16000000000
<PATH>/SOAPdenovo_LR-127mer contig -g PREFIX -D 1 -R -p 32
```
