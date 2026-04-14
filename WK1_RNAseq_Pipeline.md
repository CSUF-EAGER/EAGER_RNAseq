## RNAseq Pipeline Guide (Week 1)
This guide outlines a basic RNA-seq pipeline including trimming, alignment, and quantification.

# Trimming
Trimming is quality control process for removing low-quality bases and adapter sequences from raw sequencing reads to improve overall 
data quality. This step helps ensure more accurate alignment and reliable downstream analysis in RNA-seq workflows. Trim Galore is a 
widely used tool that automates quality and adapter trimming by combining Cutadapt and FastQC into a simple, user-friendly pipeline.

1. Enter your trimming environment, go to ```scripts/```, and create a ```<trim>.sh``` file
2. Use ```Trim Galore``` to cut our low quality bases, short reads and adapters on one of our sample (SRR11517726.fastq or
   HealthyLung_1.fastq)
```bash
# Default Trimgalore Code
trim_galore <input_fastq> -o <output>
```
3. Exit our of your script and run it
```bash
nohup bash <trim>.sh > ../logs/<run_trim>.log 2>&1 &
```
4. After trimming, go to ```trimmed/``` and run fastqc and see quality report
   
# Alignment
Alignment is the process of mapping trimmed sequencing reads to a reference genome to determine their origin. In RNA-seq, 
this requires a splice-aware aligner, which can correctly map reads that span exon–exon junctions by accounting for introns. 
HISAT2 is a fast and efficient splice-aware aligner that uses a graph-based approach to accurately align reads and support downstream 
analysis.

1. Enter your alignment environment, go to ```scripts/```, and create an ```<alignment>.sh``` file
2. Use ```hisat2``` to align our trimmed read to reference genome
```bash
#Default hisat2 alignment code
hisat2 -x <index> -U <reads.fq> -S <output.sam>
```
2. Exit out of your script and run:
```bash
nohup bash <alignment>.sh > ../logs/<alignment>.log 2>&1 &
```
3. Once complete check your ```alignment/``` and you should ```sorted.bam``` file and ```summary.txt```
4. Use samtools to convert ```.sam``` to ```_sorted.bam``` and sort it
```bash
samtools sort -o output_sorted.bam input.sam
```
# Quantification (Counting)
Quantifying RNA is the process of measuring gene expression levels by counting how many sequencing reads correspond to each gene 
in RNA-seq data. HTSeq is a Python-based tool used in this step to assign aligned reads to genomic features like genes based on 
annotation files. It produces a count table of reads per gene.

1. Enter your counting environment, go to your ```scripts/```, and create a ```<count>.sh``` file
2. Use ```htseq``` to quantify reads by counting
```bash
#Default code
htseq-count input.bam annotation.gtf > output.bam
```
3. Run the script
```bash
nohup bash <count>.sh > ../logs/<count>.log 2>&1 &
```
4. Check for results in ```counts/```
5. Download counts to your local computer using ```scp```
