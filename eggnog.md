# EGGNOG Genome Annotation

## Background

After we _assemble_ the DNA sequencing reads into long continuous _contigs_ we need to figure out where the genes are. This is called **gene annotation.**

To learn more about gene annotation see
- https://youtu.be/_N3XrrB2u9k?si=QQKYRYZxd_5fWz11
- https://informatics.fas.harvard.edu/resources/tutorials/how-to-annotate-a-genome/

Now that we have the individual protein sequences or DNA coding sequences (CDS) for genes in the genome, we want to know _what_ these genes do. 

Functional annotation relies on the principle that similar protein sequences lead to similar protein structures and therefore similar functions. 

So, we compare our proteins of unknown function with proteins of known function. This allows us to hypothesize what the function of these proteins may be. 

To learn more about functional gene annotation see:
- https://pmc.ncbi.nlm.nih.gov/articles/PMC2907659/
- https://youtu.be/c-U7vC1gwUc?si=seIQgXfLSergSDoO

## Method
There are multiple methods for identifying the function of proteins. The one we will work with here is called eggNOG-mapper

https://youtu.be/c-U7vC1gwUc?si=seIQgXfLSergSDoO

Because eggNOG-mapper is not installed on our cluster, you will need to install it locally by using a conda environment. This keeps the program isolated from interfering with other operations on your account.
To read more about conda environments see: https://www.anaconda.com/docs/getting-started/concepts/what-is-an-environment

## Step 1 - Install eggNOG-mapper

### Step 1a - load requirements

Load all the required packages that are already installed on the cluster. You will need to do this **every time** you want to run eggNOG-mapper after restarting your window. 

```
module load anaconda3
module load diamond
module load hmmer
```

### Step 1b - creat0e conda environment

You will first create a conda environment to install things in. Then you will activate it

```
conda create -n eggnog-mapper
conda activate 
```

Now that the conda environment is active, you should see that it says eggnog-mapper is active
<img width="611" height="83" alt="image" src="https://github.com/user-attachments/assets/8ed0c6c9-d24f-4177-9ad9-39637622141b" />

### Step 1c - install dependencies & program

Now we will install the rest of the dependencies and the program itself 

```
conda install -c bioconda mmseqs2 
conda install bioconda::eggnog-mapper
```

These processes may take a while, and as they install, if you are asked a question type `y` to agree

## Step 2 - Download the required databases

We need the reference proteins to compare our protein to 

### Step 2a - make database folder

We need a place to keep the databases

```
mkdir eggnog-data
```

### Step 2b - Download the databases

This may take a while! Be patient. 

```
wget -c -O eggnog-data/eggnog.db.gz  http://eggnog5.embl.de/download/emapperdb-5.0.2/eggnog.db.gz

wget -c -O eggnog-data/eggnog.taxa.tar.gz   http://eggnog5.embl.de/download/emapperdb-5.0.2/eggnog.taxa.tar.gz

wget -c -O eggnog-data/eggnog_proteins.dmnd.gz   http://eggnog5.embl.de/download/emapperdb-5.0.2/eggnog_proteins.dmnd.gz
```

### Step 2c - Extract the databases

We need to uncompress the databases so that they are searchable 

```
cd eggnog-data
gunzip *gz
tar -xf eggnog.taxa.tar
```

## Step 3 - Test the installation

We want to make sure the program is working. We are going to test it with sample data 

### Step 3a - make a directory to run the analysis

```
mkdir eggtest
cd eggtest
```

### Step 3b - copy over the test file from the project space

```
cp /projects/labella_lab/wild_yeast/test.prot .
```

### Step 3c - run the test data

Run this command and then explore the output data

```
emapper.py -i test.prot --itype proteins  -o fungal_annotation  --output_dir test --data_dir /users/alabell3/eggnog-data --tax_scope 4751 --go_evidence non-electronic -m diamond
```

