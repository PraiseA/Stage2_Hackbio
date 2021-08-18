# Stage2_Hackbio
Replicating Identification of somatic and germline variants from tumor and normal sample pairs workflow

Link to workflow: https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/somatic-variants/tutorial.html
## Create directory name Stage2
### Create a folder under Stage2 named datasets to download all dataset for this process

## Download all data needed
The data from Zenodo using wget command line 

```
wget https://zenodo.org/record/2582555/files/SLGFSK-N_231335_r1_chr5_12_17.fastq.gz
```
This command line is used to download all four data and reference genome. 

## Quality Control
FastQC was used to perform quality control on both normal and tumor tissue to make sure that the input data is of good quality, i.e., that there haven’t been any major issues during DNA preparation, exon capture, or during actual sequencing.

```
fastqc datasets/sample_r1_chr5_12_17.fastq.gz datasets/sample_r2_chr5_12_17.fastq.gz
```
## Trimming and filtering of reads
Trim off low quality parts of reads or poor quality reads to avoid spurious variant calls using Trimmomatic 
```
trimmomatic PE -threads 8 raw_data/${sample}_r1_chr5_12_17.fastq.gz raw_data/${sample}_r2_chr5_12_17.fastq.gz \
                trimmed_reads/${sample}_r1_paired.fq.gz trimmed_reads/${sample}_R1_unpaired.fq.gz \
                trimmed_reads/${sample}_r2_paired.fq.gz trimmed_reads/${sample}_R2_unpaired.fq.gz \
                ILLUMINACLIP:TruSeq3-PE.fa:2:30:10:8:keepBothReads \
                LEADING:3 TRAILING:10 MINLEN:25
```

## Mapping reads to the human reference genome
### First step is to index the reference genome.
```
bwa index -p *reference genome name* reference_genome.fa
```

## Mapping reads to reference genome
This step is done twice for both trimmed normal tissue and tumor tissue
```
bwa mem -M -t 2 \
*path to index reference genome* \
*path to trimmed forward reads* *path to trimmed reverse reads* \
> *path to save result/(sample).sam
```

## Convert sam to bam file 
```
samtools view -S -b /path to/sam file > /path to bam file/name of bam file
```

## Filter mapped reads using bamtools
```
bamtools filter -in input_alignment.bam -out output_filtered.bam -length 100
```
## Duplicate Reads Removal
By default, this command works on paired-end reads
```
samtools rmdup <input_filtered.bam> <output.bam>
```
## Reads are re-aligned using BamLeft-Align
```
samtools view -bh "${input_bam}" | bamleftalign
--fasta-reference "${reference_fasta_filename}"
 -c
 --max-iterations "${iterations}"
 > "${output_bam}"
 ```
 ## Recalibrate read map
 ```
 samtools calmd -bAr aln.bam > aln.baq.bam
 ```
## 
