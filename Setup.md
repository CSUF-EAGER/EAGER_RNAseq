# Installing Miniconda3 and Setting Up RNA-seq Environment
This is the setup guide covering Miniconda installation, setting up a dedicated environment, and downloading the necessary sequencing data and genome resources to begin the workflow.

## 1. Install Miniconda3

1. Download and install Miniconda3:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
bash Miniconda3-py39_4.12.0-Linux-x86_64.sh

# Reload Shell Configuration and verify installation
source .bashrc
conda --version
```

## 2. Configure Conda Channels
1. Check current channels:
```bash
conda config --show channels
```
You should see ```defaults```.

2. Add required bioinformatics channels:
```bash
conda config --add channels conda-forge
conda config --add channels bioconda
```

## 3. Create Conda Environments
1. Set up separate environments for each step of the workflow:
```bash
# Trimming and QC
conda create -n trimming -c bioconda -c conda-forge trim-galore fastqc

# Alignment
conda create -n alignment -c bioconda -c conda-forge hisat2 samtools

# Read Counting
conda create -n counting -c bioconda -c conda-forge htseq
```
2. To view and activate your installed Conda environments, type:
```bash
# View the available environments
conda env list
# Activate an environment (replace <environment_name> with the name of the environment you want to use
conda activate <environment_name> 
# When you are done with a task, deactivate it with:
conda deactivate
```
(Optional)
For faster environment switching, you can add aliases to your .bashrc or .bash_profile:

## 4. Set up your project working directory
1. Create a project directory and set up subdirectories and symlinks to large data files pre-loaded on Kepler
```bash
# Navigate to your EAGER working directory
cd ~/EAGER/users/<USERNAME> #replace <USERNAME> with the name of your working folder
# Create a new folder for this project
mkdir -p RNA-seq; cd RNA-seq
# Create subdirectories and symlinks
mkdir -p trimmed_reads fastqc hisat2 alignments counts scripts logs
ln -s ~/EAGER/modules/rnaseq-covid/data/* .
```

2. (Depracated instructions - no need to rename files)
For ease of downstream analysis, rename the downloaded SRR files to more descriptive sample names. Below is the mapping between the original accession IDs and their corresponding filenames
```bash
SRR11517726.fastq HealthyLung_1.fastq
SRR11517733.fastq HealthyLung_2.fastq
SRR11517729.fastq CovidLung_1.fastq
SRR11517737.fastq CovidLung_2.fastq
```

## 5. Run FASTQC on Raw FASTQ Files
1. Run FASTQC on all raw reads files
```bash
cd ~/EAGER/users/<USERNAME>/RNA-seq/
conda activate trimming
fastqc reads_raw/*.fastq -o fastqc
```
