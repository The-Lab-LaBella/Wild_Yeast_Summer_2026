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

Open the ITS fasta file in the ITS folder and copy the sequences

Go to blastn and see what species you have! https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn

Report your species in the table
