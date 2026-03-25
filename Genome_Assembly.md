# Genome Assembly

This tutorial will walk you through how to assemble the Nanopore genomes sequenced by Plasmidsaurus.

If you want to learn more about how Nanopore sequencing works there are lots of informative videos here: https://nanoporetech.com/platform/technology 

This tutorial will only cover the bioinformatics parts of the analysis. We will discuss the background information in person. 

Along the way you will report results in this spreadsheet: https://docs.google.com/spreadsheets/d/17ScIAdl7iKe4SDxmFpJEvZmdBlQJUDD8eWTuUf0sbvA/edit?usp=sharing 

&nbsp;
&nbsp;

## Step 1 - Set up your folder

&nbsp;

### Step 1a - create a new folder to work in 

In either your home or scratch directory create a new folder with your genomeID. I will use ABB_052825_01_A as an example throughout 

```mkdir ABB_052825_01_A```

Then move into that directory using `cd`

&nbsp;

### Step 1b - copy the fastq files into your directory 

If you have access to the `labella_lab` directory you can copy them from there.

If you do not, let Dr. LaBella know and she will get you the files. 

```
cp /projects/labella_lab/wild_yeast/summer_25_results/RKTLP_1_AB-01.fastq.gz .
```

&nbsp;

## Step 2 - Filter the sequencing reads

Not all sequencing reads will be of the same quality. We want to trim or remove sequences that are low quality. To do this we will use https://github.com/OpenGene/fastplong 

&nbsp;

### Step 2a - Install fastplong 

We need to install and activate fastplong before running it. 

```
#activate anaconda
module load anaconda3

#create a new conda environment named fastplong
conda create --name fastplong

#activate the conda environment - after doing this you should see (fastplong) before your username in the command line
conda activate fastplong

#install fastplong in your environment
conda install bioconda::fastplong

#when it asks you if you want to proceed type y and hit enter
```

&nbsp;

### Step 2b - Filter your sequencing reads

To filter the reads use this command

```
fastplong -i RKTL8P_10_ABB-01.fastq.gz -o RKTL8P_10_ABB-01.filtered.fastq.gz
```

Once this is done, you will have a new file and a report on the filtering saved to `fastplong.html`

Download the html file and take a look at it. Report on the spreadsheet your
- Perecent of Reads passed Filters
- Total Reads
- Median Lenght (in Kilo bases (K))

&nbsp;

### Step 2c - Deactivate your environment

We don't need to use fastplong anymore so deactivate the environment using the command

```
conda deactivate fastplong
```

&nbsp;
&nbsp;

## Step 3 - Assemble reads into the genome

We will use the assembler flye to assemble the nanopore genome https://github.com/mikolmogorov/Flye 

&nbsp;

### Step 3a - Load Flye on the cluster

Flye is available on the HPC so we will load it first

```
module load flye
```

&nbsp;

### Step 3b - Uncompress the fastq files

Flye expects uncompressed files so you will need to uncompress the gz file generated above

```
gunzip RKTL8P_10_ABB-01.fastq.filtered.gz
```

&nbsp;

### Step 3c - Run Flye using the high quality nanopore settings

The genome assembly will take more resources than we should use on the head node of the cluster.

Therefore we are going to submit a slurm job to the cluster. This should take less than an hour to complete

You will copy the slurm file `flye_assembly.slurm` from the project space. 

```
cp /projects/labella_lab/wild_yeast/flye_assembly.slurm
```

You will then need to change the name of the sequence in the file. You can see the text of the file below. 

```
#!/bin/bash
#SBATCH --job-name=flye_assembly
#SBATCH --output=flye_assembly_%j.out
#SBATCH --error=flye_assembly_%j.err
#SBATCH --partition=Orion
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --mem=64G
#SBATCH --time=2:00:00

module purge
module load flye

# Replace the following with your Flye command and input files
flye --nano-hq RKTL8P_10_ABB-01.filtered.fastq -o RKTL8P_10_ABB-01/ --threads 16


```

&nbsp;

### Step 3d - Refine assembly with Medaka

We will further refine our assembly using Medaka: https://github.com/nanoporetech/medaka

Again we will run this using a slurm script - this should take less than 30 minutes

To install medaka from conda follow this

```
conda create -n medaka
conda activate medaka
conda install bioconda::medaka
```

```
#!/bin/bash
#SBATCH --job-name=medaka_assembly
#SBATCH --output=medaka_assembly_%j.out
#SBATCH --error=medaka_assembly_%j.err
#SBATCH --partition=Orion
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --mem=32G
#SBATCH --time=4:00:00

module purge
module load artic

# Replace the following with your medaka command and input files
medaka_consensus -i RKTL8P_10_ABB-01.filtered.fastq -d RKTL8P_10_ABB-01/assembly.fasta -o RKTL8P_10_ABB-01 -t 16

```

&nbsp;
&nbsp;

## Step 4 - Analyze your final genome

Your final genome is now held in your new folder under the name `consensus.fasta`

### Step 4a - Copy your genome

We want to rename the genome to it's proper name

```cp RKTL8P_10_ABB-01/consensus.fasta ABB_052825_01_A.fasta```

&nbsp;

### Step 4b - Analyze your genome quality 

```
#remove any active modules
module purge

#load QUAST
module load quast

#run quast and output the results to the directory quast_results
quast -o quast_results ABB_052825_01_A.fasta
```
&nbsp;

### Step 4c - Report your results

Look at your results in the quast_results folder 
- Number (#) of Contigs
- Total Length
- N50
- GC%

&nbsp;
&nbsp;
&nbsp;

# Genome Annotation

Now we need to find genes in the genome! This process takes a long time, so it likely won't finish in one day. 

&nbsp;

## Step 1 - Preparing our assembly

We will prepare our assembly following these instructions: https://funannotate.readthedocs.io/en/latest/prepare.html#prepare

### Clean the genome

```
funannotate clean --input ABB_052825_01_A.fasta --out ABB_052825_01_A.clean.fasta
```

### Sort and rename FASTA headers

We will also remove any contigs less than 500bp

```
funannotate sort --minlen 500 --input ABB_052825_01_A.clean.fasta --out ABB_052825_01_A.clean.sorted.fasta
```

### Repeatmask our genome

```
funannotate mask --input ABB_052825_01_A.clean.sorted.fasta --out ABB_052825_01_A.masked.fasta
```

Take a look at the terminal and report the **masked repeats**

### Run the genome annotation!

Copy over the `funannotate.slurm` file from the project space, edit it, and then submit!

```
#!/bin/bash
#SBATCH --job-name=funannotate
#SBATCH --output=funannotate_%j.out
#SBATCH --error=funannotate_%j.err
#SBATCH --partition=Orion
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=32G
#SBATCH --time=12:00:00

module purge
module load genemark
module load anaconda3
conda activate funannotate
export FUNANNOTATE_DB=/projects/labella_lab/funannotate_db

funannotate predict -i ABB_052825_01_A.masked.fasta -o annotation -s "ABB_052825_01_A" --cpus 4

```

&nbsp;
&nbsp;
&nbsp;

# Species Prediction 

While we wait for the genome anntotation to complete, we can figure out what species you have!

We will use the ITS sequence: https://en.wikipedia.org/wiki/Fungal_DNA_barcoding

## Step 1 - Install the ITS extraction program

```
module purge
module load anaconda3
conda create -n extract_its bioconda::itsx bioconda::barrnap conda-forge::biopython conda-forge::pandas conda-forge::git
conda activate extract_its
git clone https://github.com/fantin-mesny/Extract-ITS-sequences-from-a-fungal-genome
```

## Step 2 - Find the ITS sequence

run the script below

```
python Extract-ITS-sequences-from-a-fungal-genome/extractITS.py -which ITS2 -i ABB_052825_01_A.masked.fasta -o ITS/ -name ABB_052825_01_A

```

## Step 3 - BLAST the ITS Sequence

Open the ITS fasta file in the `ITS` folder and copy the sequences 

Go to blastn and see what species you have! https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn 

Report your species in the table 

&nbsp;
&nbsp;
&nbsp;


# BUSCO Analysis

BUSCO is a way to assess genome completeness 

Read more here: https://www.numberanalytics.com/blog/ultimate-guide-busco-genome-assembly-annotation

## Step 1 - Run BUSCO

```
module purge
module load busco
mkdir ~/augustus_config
export AUGUSTUS_CONFIG_PATH=~/augustus_config/
busco -i ABB_052825_01_A.masked.fasta -o ABB_052825_01_A.busco --out_path busco -m geno -l saccharomycetes_odb10
```


