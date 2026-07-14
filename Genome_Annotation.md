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

### Step 4a - BUSCO results

One of the major questions we want to know is, how well did our annotation method do in finding genes? 

One way we can ask that question is to look to see if we find genes we expect to find in every organism. 

One way of doing this is by looking at BUSCO or Benchmarking Universal Single Copy Orthologs.


BUSCO genes have the following properties

- **Single-Copy**: BUSCO genes are present as one copy in the genome, and expected to be the same in most species within a taxonomic group. 
- **Orthologs**: Genes that share a common ancestral gene in different species. 
- **Conserved across taxa**: The selection of single-copy orthologs used in BUSCO datasets are highly conserved across a wide range of species. 

Running a BUSCO analysis will tell us about the number of BUSCO genes that are
- **Complete** - found in the genome assembly
- **Duplicated** - found in more than one copy in the genome
- **Framented** - only part of the gene was found
- **Missing** - the gene is absent due to technical or biological resons.

To find the BUSCO results you will need to navigate to the file `short_summary_GENOMENAME.txt` 

The path to this file is `predict_misc/busco/runGENOMENAMEX/1111111/short_summary_srrXXXXX.txt`
You will need to replace the `XXXXX` with your SRR number and the `1111111` will be a random set of numbers

&nbsp;
# LQ 3
## LQ 3a

What percent of the BUSCO genes are complete? 

## LQ 3b 

We are comparing our yeast genome to all of Dikarya (a subkingdom of Fungi). Therefore, we would expect a great genome annotation to have a BUSCO completeness score of >90%. Anything above 75% is acceptable.

How well was your genome annotated if we assume all the missing BUSCOs are missing because of technical issues?
- Very well annotated
- Adequately annotated
- Poorly annotated

&nbsp;
&nbsp;

### Step 4b - Analyze annotation results

We will now look at a file called `GENOMENAMEX.stats.json` in the `predict_results` folder. 

This file is in a special machine-readable format called `JSON`. But you can view the information using `cat`

&nbsp;

# LQ 4
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

cp GENOMENAMEX.cds-transcripts.fa /projects/class/binf3101_001/genome_annotations/.

```



# LQ 5
Confirm you copied your file over

