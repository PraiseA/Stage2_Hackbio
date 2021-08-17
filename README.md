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
fastqc datasets/SLGFSK-N_231335_r1_chr5_12_17.fastq.gz datasets/SLGFSK-N_231335_r2_chr5_12_17.fastq.gz
```

## Mapping reads to the human reference genome

```
