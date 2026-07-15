## Outline

[Introduction](#introduction)

[Step 1](#step-1-set-up-your-lab-4-folder)

[Step 2](#step-2-prep-your-genome)

[Lab Question 1](#lq-1)

[Lab Question 2](#lq-2)

[Step 3](#step-3---annotate-the-genome-with-funannotate)

[Step 4](#step-4---examine-the-results)

[Lab Quesiton 3](#lq-3)

[Lab Quesiton 4](#lq-4)

[Step 5](#step-3---copy-over-your-annotation-to-the-shared-folder)

[Lab Quesiton 5](#lq-5)

&nbsp;
&nbsp;

## Introduction

In the lab today we will be annotating our genomes. Annotation is the process of finding the genes. 

We will be using an excellent software called FunAnnotate 

To read more about this software visit https://github.com/nextgenusfs/funannotate 

## Step 1: Set up your Lab 4 Folder

From your home directory (/users/username) create a new folder called annotation

```bash
mkdir annotation
```

Now we want to copy our genome into the annotation folder 

```bash
cp /projects/labella_lab/wildyeast/GENOMENAME annotation/.
```

NOTE: If you are practicing, you can copy a genome from `/projects/labella_lab/wildyeast/assemblies/` 
To see a list of the possible genomes, use `ls /projects/labella_lab/wildyeast/assemblies/`

As a reminder the ```.``` command means "here". So ```annotation/.``` means "here in the annotation folder. 


&nbsp;
&nbsp;

## Step 2: Prep your genome


### Step 2a: Rename and sort the contigs. 

Our genome likely contains highly repetitive regions. We want to hide or mask those regions. This helps the genome annotation software ignore these repeated regions. We will use the `funannotate sort` command. 

From the Funannotate documentation

```bash
This script sorts the input contigs by size (longest->shortest) and then relabels
the contigs with a simple name (e.g. scaffold_1).  Augustus can have problems with
some complicated contig names.
```

```bash
#load funannotate
module load funannotate

funannotate sort -i GENOMENAME -o GENOMENAME.sort.fa --minlen 500

```
&nbsp;
&nbsp;

1 
### Step 2b: RepeatMask the assembly

We will also use funannotate to mask the genome. 

If you have closed the terminal you will need to reload funannotate `module load funannotate`

```bash
funannotate mask -i GENOMENAME.sort.fa -o GENOMENAME.masked.fa

```

Our new masked genome from this pipeline will be **GENOMENAME.masked.fa**


The results of the masking will be in a file that starts with `funannotate.mask`


Inspect that file and answer the questions below

&nbsp;
&nbsp;

# REPORT

Fill this out on the shared google sheet. What percent of your genome was masked due to repeats?

&nbsp;
&nbsp;

## Step 3 - Annotate the genome with Funannotate

### Step 3a - Setup GeneMark

Funannotate calls a program called genemark. We need to tell genemark where to look for its configuration files

```bash
#go to your home directory
cd
#enter this command exactly
cp /projects/labella_lab/wild_yeast/.gm_key $HOME/.gm_key
```
&nbsp;

### Step 3b - Set up the slurm script

By submitting this slurm script, we will be sending these commands to the HPC to run

To copy the slurm script to your current directory use

```bash
#go back into lab 4
cd annotation
cp /projects/labella_lab/wild_yeast/funannotate.slurm .
```

Below is what is in the funannotate.slurm script. If you use `cat funannotate.slurm` you will see the following commands. _You do not need to run these commands_

```
module purge
module load genemark
module load anaconda3
conda activate funannotate
export FUNANNOTATE_DB=/projects/labella_lab/funannotate_db
export GENEMARK_PATH="/apps/pkg/anaconda3/apps/genemark-4.72/gmes_linux_64/"

funannotate predict -i INPUT -o annotation -s "NAME" --cpus 4
```

You will need to edit the file to replace INPUT and NAME with your input file and genome name

&nbsp;

### Step 3c - Run the script

Once the script is edited, you will submit the job to run on the cluster. 

This will take **THREE TO FOUR HOURS**

```bash
sbatch funannotate.slurm
```

You can check to see if your job is running using the `squeue -u` command 

```bash
squeue -u username
```

&nbsp;
&nbsp;

## Step 4 - examine the results 

You will see a new folder called `GENOMENAMEX` with your SRR number. 

Within that folder you will find three folders
- `logfiles` : the logs for the programs run
- `predict_misc` : other miscellaneous predictions
- `predict_results` : the results for our genome annotation

You will examine several of the files from our output to answer the questions below

&nbsp;



### Step 4a - Analyze annotation results

We will now look at a file called `GENOMENAMEX.stats.json` in the `predict_results` folder. 

This file is in a special machine-readable format called `JSON`. But you can view the information using `cat`

&nbsp;

# REPORT

Report the following statistics for your genome annotation
- Number of protein-coding genes
- Number of tRNA genes
- Average length of the genes
- Number of transcripts with at least one intron
- Average length of genes in amino acids

&nbsp;

### Step 5 - Copy over your annotation to the shared folder

To save a copy of your genome annotation, you will make a copy in our shared class space. We will copy over the nucleotide sequences

This file is `GENOMENAMEX.cds-transcripts.fa` and is located in the `predict_results` folder. 

Copy this using the command below

```bash

cp GENOMENAMEX.cds-transcripts.fa /projects/labella_lab/wild_yeast/

```



