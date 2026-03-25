# Outline

[Introduction](#introduction)

[VPN Instructions](#before-you-get-started)

[Account Setup](#account-setup)

[Logging in](#logging-in)

[Short Activity](#short-activity)

&nbsp;
&nbsp;

# Introduction

If this is your first time connecting to a high-performance computing (HPC) cluster or your first time connecting to our HPC, this guide will help you connect and get ready to assemble our genomes. 

If you are completely new to the command line, here are some websites to review before you begin 

- Basic Command Line Commands: https://www.geeksforgeeks.org/basic-linux-commands/
- Navigating in the command line https://www.youtube.com/watch?v=dzHscTzpAME 
- POSIX game to get you started: https://gitlab.com/slackermedia/bashcrawl
- What is SLURM!? https://blog.ronin.cloud/slurm-intro/

&nbsp;

# Before you get started
You will need your DUO authentication method and if you want to log in off-campus you will need to connect to the University via the VPN

YOU MUST HAVE THE VPN CONNECTED TO LOG IN OFF CAMPUS!!! Or anytime you are not on the UNC Charlotte wifi.

To set up the VPN see here: https://services.help.charlotte.edu/TDClient/33/Portal/KB/ArticleDet?ID=677

&nbsp;

# Account Setup

High Performance Computing Cluster
You will need access to the HPC here at UNC Charlotte. If you had access in a class this will not be sufficient for our work. You will need to request a research account by going to this link https://services.help.charlotte.edu/TDClient/33/Portal/Requests/ServiceDet?ID=140

Make sure you also ask for access to /projects/labella_lab

The HPC has some help pages, but I've put together a quick-start HPC guide here: https://github.com/The-Lab-LaBella/Welcome_to_the_Lab/blob/51e711fc28308f657b9648db7fc7e7ff8d9fc557/HPC_getting_started.md#submitting-jobs-to-the-cluster
 
&nbsp;

# Logging in

To access the remote computer servers, you will use the command line. There are numerous software/apps that will help you log in. This includes PuTTY and MobaXterm. Belowe are the instructions for the Windows PowerShell and standard mac command line. 

&nbsp;

## Using a Mac Computer

### Mac Step 1 - Open SSH Client

Navigate to the Utilities folder within applications and double-click on the Terminal application.

![image](https://github.com/user-attachments/assets/6b6ee9f0-b7dd-4fb7-98b2-79e5614892c4)

### Mac Step 2 - Log into the cluster

Once the terminal is open, you will log into the cluster using the command below. Your username is your UNC Charlotte username aka your email address without the @charlotte.edu portion 

Type the following command and hit enter/return.

`ssh username@hpc.charlotte.edu`

### Mac Step 3 - Enter your password

You will be prompted to enter your password. **You will not see anything as you type!!** 

Once your password is typed in hit enter/return.

### Mac Step 4 - Complete two factor authentication

Then you will be prompted to complete the DUO authentication. Enter 1, 2, or 3 for your preferred way of authenticating, and then press Enter/Return.

&nbsp;

## Using a Windows Computer

### Windows Step 1 - Open or install Windows PowerShell

The PowerShell comes installed on most new Windows machines. If you do not have it installed search "PowerShell" in the Windows Store and install the App

![image](https://github.com/user-attachments/assets/5e99d02c-0db8-438f-97f4-3163846438cd)

### Windows Step 2 - Log into the cluster

Once the terminal is open, you will log into the cluster using the command below. Your username is your UNC Charlotte username aka your email address without the @charlotte.edu portion 

Type the following command and hit enter/return.

`ssh -m hmac-sha2-512 username@hpc.charlotte.edu`

### Windows Step 3 - Enter your password

You will be prompted to enter your password. **You will not see anything as you type!!** 

Once your password is typed in hit enter/return.

### Windows Step 4 - Complete two factor authentication

Then you will be prompted to complete the DUO authentication. Enter 1, 2, or 3 for your preferred way of authenticating, and then press enter/return.

&nbsp;

# Short Activity

If you are new to the command line and the research cluster, below is a short activity that will help you practice essential skills for working in the command line

## Step 1 - Navigating the Cluster

You are currently in your home directory. You can return to this directory at any time by using the command ```cd```

To see where you are in the cluster at any time use the command ```pwd``` which stands for Print Working Directory

The cluster has folders and files just like your local computer. 

```bash
#change directory
cd
#present working directory
pwd
```
&nbsp;

### Step 1a - Make a Folder

To make a folder or directory you use the command ```mkdir```

Create a folder called ```candida_activity``` using the command below

```bash
mkdir candida_activity

```

**_TIP_** _**DO NOT** use spaces or other special characters in your folder or file names. Use only letters and numbers. If you want to differentiate words you can use_ ```_``` _or_ ```.```

&nbsp;
### Step 1b - List folders and directories

To list the files or folders/directories in your current location use the ```ls``` command

You can also use the command ```tree``` to view all the files and directories below where you are. 

```bash
#to list the files in the current directory
ls
#to see a tree of the files
tree
```

&nbsp;

### Step 1c - Enter your new directory 

To change directories you use the ```cd``` or change directory command. 

We want to enter the directory we just made. To do that execute the command below

```bash
cd candida_activity
```

Check to make sure you are where you think you are using ```pwd```. 

_**TIP**_ _You can autocomplete any file or directory currently accessible by using tab. For example, if you are in your home directory and want to enter the lab_1 directory, you can type_ ```cd la[tab]``` _and it will complete to_ ```cd lab_1```
_If there is more than one option it will list all of the options, and you will have to type more to complete the process._

&nbsp;

&nbsp;

## Step 2 - Upload the genome file 

We now need to get the file from your computer to the research cluster. You will first need to download the file from GitHub to your computer. 

To transfer a file **from** your computer, you need a terminal that is connected to your local computer and _not_ to the HPC.

### Step 2a - Open a new terminal and navigate to the folder

Open a second terminal window and navigate to wherever your file is saved. 

You can use ``cd`` to navigate to where your file is located. 

Another option is to find the exact path of your file and nagivate there. To do that, you can open the finder/folder view and right-click/control-click on the file. You will see an option to ``Copy [item] as Pathname`` or ``Copy as path``

You will then get a pathname like this: ``C:\Users\alabell3\Desktop\candida_albicans.fas.tar.gz`` the folder where the file is located is ``C:\Users\alabell3\Desktop\``

Navigate to that folder using a command such as

```
cd C:\Users\alabell3\Desktop\
```

Use ``ls`` to make sure the file is in your current directory. 

&nbsp;

### Step 2b - Upload file

The command for transferring a file from one computer to another is ``scp`` or "Secure CoPy". 

The general format for ```scp``` or secure copying a file from your local computer to the cluster is:
```bash
scp LOCAL_FILE username@hpc.charlotte.edu:DIRECTORY/IN/DESTINATION
```

In Windows you will have to add an extra option.

```bash
scp -o MACs=hmac-sha2-512 LOCAL_FILE username@hpc.charlotte.edu:/DIRECTORY/IN/DESTINATION
```

Now, when you go back to your terminal that is logged into the cluster and type ``ls`` you should see your file! 

&nbsp;

### Step 3 - Uncompress the file 

Sequence files can become quite large. There are numerous ways to compress a file using methods like tar, gzip, and zip. The file we are working with has been compressed using the tar command. 

If you were to use the command ```cat``` or ```head``` to look at our file right now it would go crazy! (you can try it). If you ever need to exit a command that is running you can use ```[Cntrl]+[c]``` in windows or ```[command]+[.]``` on a mac. 


To uncompress our file use the command
```bash
tar -xvf candida_albicans.fas.tar.gz
```

The ```-xvf``` tells the tar program what to do with the file listed

```-x``` = extract 

```-v``` = verbose (list all the files as they are being extracted)

```-f``` = next item is the file

You should now see a file called ```candida_albicans.fas```

**_HINT_** You can see the options for any command by typing the command and following that with ```--help``` or sometimes ```-h```. For example ```tar --help``` will provide you with a detailed set of options for the command

&nbsp;

### Step 4 - Visualize the file

This is a LARGE file. It is the whole genome of the yeast species _Saccharomyces candida_albicans_. It is in what is called FASTA format. Our file contains **nucleotide** data but FASTA files can contain amino acid or nucleotide data. The general format for FASTA format is:
```
>sequence identifier 1
sequence data
>sequence identifier 2
sequence data
```

Let's take a look at our file using the ```less``` command.

```bash
less candida_albicans.fas
```

This will display the file in your terminal. You can press enter to see more of the file. To quit this view hit the ```q``` key


Now let's take a look at our file using the ```head``` command. This command will print into your terminal the first set of lines in the file. You can also specify how many lines you would like it to display. Let's look at the first 5 lines. 

```bash
head -5 candida_albicans.fas
```

### Knowledge Check 1
What is the sequence identifier of the first sequence in the candida_albicans.fas file 

<details>
 <summary>Answer</summary>
 Ca22chr1A_C_albicans_SC5314 (3188363 nucleotides)
</details>

&nbsp;

### Step 5 - Count the number of sequences

There are a wide variety of tools to analyze sequence data. The built-in tools in our terminal can also be very powerful. One of the most powerful tools is ```grep```. 

The ```grep``` command can search for a pattern in a large file. 

If we want to count how many sequences there are in our file, we can simply count the number of times the ```>``` character appears in our file. 

Try out grep using this command:
```bash
grep ">" candida_albicans.fas
```

One of the options in ```grep``` is to count the number of instances instead of returning them. This is the ```-c``` option. 
```bash
grep -c ">" candida_albicans.fas
```

To see more options in ```grep``` try ```grep --help```

### Knowledge Check 2
How many sequences are in our file. 

<details>
 <summary>Answer</summary>
9
</details>

&nbsp;

### Step 6 - Analyze the base composition of our genome

While there are many useful programs already included in the terminal, there are programs that have been installed on the cluster as **modules**

To access these modules we will need to load them.

The program module we will be using in this section is called EMBOSS. Let's see if EMBOSS is installed in the cluster using the command ```module search```

```bash
module search EMBOSS
##this will return 
------------------------------------------------------------------ /apps/usr/modules/apps -------------------------------------------------------------------
        emboss/6.6.0: EMBOSS (European Molecular Biology Open Software Suite) is a software analysis package specially developed for the needs of the molecular biology (e.g. EMBnet) user community. The software automatically copes with data in a variety of formats and even allows transparent retrieval of sequence data from the web.
````

We can see that EMBOSS version 6.6.0 is installed on the cluster

**_TIP_** _Case (upper vs lower) matters when loading modules. You will need to type it exactly as written before the : in the description._

**_TIP_** _Placing a_ ```#``` _before any line of code means it's a comment and not a command. I will often put notes in the code using # that will be ignored._

To load emboss we can now type

```bash
module load emboss
```

Within emboss we want to run the ```geecee```. As you can see from the name, it is a program for calculating the fraction of G (gee) or C (cee) in a sequence file. To learn more about the program use ```geecee --help```


To run geecee on our sequences use the command below where we provide an informative output file name for the results to be saved

```bash
geecee -sequence candida_albicans.fas -outfile candida_albicans.geecee.out
```


### Knowledge Check 3
What is the maximum GC content observed in our yeast genome sequences?

<details>
 <summary>Answer</summary>
0.34
</details>

### Step 7 - Find and extract open-reading frames from our genome

Open Reading Frames (or ORFS) are the most simple method for identifying regions in a genome that may contain protein-coding genes. They consist of a start codon (ATG) and a stop codon. They _cannot_ account for more complicated gene structures like genes containing introns. They also do not exclude things that look like genes - but probably aren't.

For example, a protein with the amino acid sequence ```MTTTTTTTTTTTTT``` is probably not a real gene!

We will run this command by submitting a **slurm script** to the scheduler.


#### Step 7a - Copy the slurm script
First, you will need to copy the slurm script into your directory. 

This time we will will copy it over directly from github. 

```bash
wget "https://raw.githubusercontent.com/The-Lab-LaBella/Wild_Yeast_Summer_2025/refs/heads/main/emboss_candida_albicans.slurm"
```

#### Step 7b - View the slurm script

The slurm script contains all the information the slurm scheduler needs to run your job for you. 

View the slurm script using either ```cat``` or ```less```

```#! /bin/bash``` - This line tells the program reading the file that the language is bash

Anything starting with #SBATCH will not be executed as part of the code. It is special instructions for the scheduler

```#SBATCH --partition=Orion``` - This tells the scheduler what part of the 

```#SBATCH --job-name=emboss_sacch```- This is a name for the job

```#SBATCH --nodes=1``` - This is the number of nodes we are requesting to run the job

```#SBATCH --ntasks-per-node=1``` - This is the number of tasks per node (we only increase this if we have threaded programs)

```#SBATCH --time=1:00:00``` - This is **one of the most important parts**. This is the amount of time you are requesting for the job. The format is ```days-hours:minutes:seconds``` If you do not give enough time the program will end without completing and you will get an error!

```#SBATCH --output=emboss_sacch.out``` - The name of the file in which the slurm will save any output that would have been shown in the command line (not the output for our program). 

You will then see a bunch of lines starting with ```echo```. These are printed (or echoed) directly into our emboss_sacch.out file and they provide information about our run. 

Then we get to the commands that will be executed by the slurm scheduler for us. **You must provide ALL commands in the script that you would type in the terminal**

We have two commands, the ```module load``` and running the program ```getorf```. 

As you can see there are place-holder files INPUT and OUTPUT that we will need to edit. 

# Step 8 - Edit our slurm script

Where you see the capital letters, you must put in the input file and specify a name for the output file 

_click on image to enlarge_

<img src="https://github.com/user-attachments/assets/a85e8dc4-4958-4fff-8b74-8c9c63760adc" width="400">

There are many ways we can edit a file on the cluster. The most common ways are 
1. upload/download
2. nano
3. vi

&nbsp;
### Option 1 - Upload/download

One option is to download the file to your local computer, edit it in a text editor and then upload the file again. 
If you are using a GUI to transfer files you may be able to edit files in the GUI which is analogous to upload/download. 

This can get tricky if you aren't careful

&nbsp;
### Option 2 - nano

nano is a built-in editor that allows you to edit files in the command line. 

To edit our file in nano type

```nano emboss_candida_albicans.slurm```

This will open an editor that looks like this

<img src="https://github.com/user-attachments/assets/832ed857-6396-427c-bc0b-edbdc865186d" width="500">

You can then navigate to the INPUT using the arrow keys 

Once there delete INPUT and replace it with our input file. Then navigate to OUTPUT and replace that with an output file ending in fasta. 

To save the file use [Ctrl]+O or [Command]+O and then use enter/return to save to the same file name

To exit use [Ctrl]+X (You can see a list of the other commands along the bottom)

Check your edits using ```cat```

&nbsp;
### Option 3 - vi 

vi or vim is another way to edit files in the command line. 

To edit our file in vi use

```vi emboss_candida_albicans.slurm```

This will open an editor that looks like this:

<img src="https://github.com/user-attachments/assets/85c8605d-fa56-43ca-9872-0b53afdc4d81" width="400">

Use the arrow keys to navigate to where you want to edit. 
To enter editing mode you must press the [i] key (you will see -- INSERT -- show up along the bottom)

Once there delete INPUT and replace it with our input file. Then navigate to OUTPUT and replace that with an output file ending in fasta. 

To save (write) your file you need to type ```:```. This will open a bar at the bottom where you can enter commands 

Type ```w``` and then hit return/enter and you will then see ```"emboss_candida_albicans.slurm" 26L, 764C written```

To quit type ```:q``` and enter

Check your edits using ```cat``

&nbsp;
### Step 9 - Run the slurm script!

To send your slurm script to the scheduler type

**_TIP_** _Your slurm script will execute all the commands as though it is in the directory where you have the slurm script. If the script needs to access files in another directory you will need to specify the directory_

```bash
sbatch emboss_candida_albicans.slurm
```

You can see the jobs you have running using the command ```squeue -u username```

This job may run so quickly it doesn't show up! 

&nbsp;
### Step 10 - Analyze the output file 

Once the job is done running, you will see your output file appear in your directory. Next, we will analyze our file. 

First, take a look at the file using ```less``` or ```head```. 

You should see protein sequences in fasta format with a header like this (this is not the actual first sequence_

```
>Ca22chr1A_C_albicans_SC5314_20665 [3187944 - 3187810] (REVERSE SENSE) (3188363 nucleotides)
MRVLITIMACHXQRRNTHLRKKLTKKLLVLNIHLIQLIQEKQSEC
 ```

```Ca22chr1A_C_albicans_SC5314_20665``` - This is the sequence identifier. The first part ```NC_001141``` is the genome sequence in which the open reading frame was found. ```3935``` is the identifier for this coding sequence. It's the 3,935th coding sequence identified on this contig (genome sequence). 

```[3187944 - 3187810]``` - Is the position along the contig (genome sequence) where the ORF was found. 

```(REVERSE SENSE)``` - This identifier is only present in ORF frames that were found when reading in reverse-complement (complimentary sequence going from right-to-left) along the DNA sequence. 

Use commands such as ```grep``` to answer the questions below. Remember, you can look at the options for grep using ```grep --help```

### Knowledge Check 4
How many ORFs were annotated in the _Candida albicans_ genome? Is this more or less than the number of genes you would expect in this genome? (Use google to find the number of genes in _Candida albicans_)

<details>
 <summary>Answer</summary>
4249

This is fewer genes than we would expect. _C. albicans_ should have about 6,100 genes. This suggests that looking only at open reading frames with >300 amino acids does not capture the complete genetic diversity. Why might this be?
</details>

&nbsp;
### Step 11 - Extract data for one genomic contig

We are particularly interested in the contig **Ca22chr1A_C_albicans_SC5314** 

Let's extract all the ORFs belonging to this contig. There are ways using biopython, bioperl or other programs to load in and analyze a fasta file. We will, however, do this manually using bash. 

##### Step 11a - Making our file a one-line fasta file

If we look into our fasta file we will see a snippet like this:

```bash
>Ca22chr1A_C_albicans_SC5314_27 [2243 - 2686] (3188363 nucleotides)
HSLFALATTSKHNCIDXTLLISCCIPPVVINXPRVVLLLIVQPANTTTTALTTPSSFRVA
IPXTTRFTFPTAIDYSNYKLFYRPFSNXQAQRDTCLGIYNSFYSSFCICHAICPPPIIQP
ANTTATGIDNCFHCYDTTTDYMLFTQQT
>Ca22chr1A_C_albicans_SC5314_28 [2397 - 2807] (3188363 nucleotides)
LHPLHFVLQFPXPLGSHFPPPLTTQTTSCSIVPSPTSKHNEIHVWAFTIASTHHFASAMQ
SAHHPSSNQQTQPQRALTTASTAMTPPLTTCCSPSKHNTLHSSSSISHSTTAISTGSPSS
LTSVKYTTPQINYPXPA
>Ca22chr1A_C_albicans_SC5314_29 [2770 - 2889] (3188363 nucleotides)
NTPPHRSTIPXRLDFRKIHYKAYPLSDYPSVPQINYPXPA
>Ca22chr1A_C_albicans_SC5314_30 [2850 - 2954] (3188363 nucleotides)
LPLSPTDQLSPAGLTSVKXTTXSAPSTLSSSIDXQ
>Ca22chr1A_C_albicans_SC5314_31 [2690 - 3067] (3188363 nucleotides)
HLAQFKFNFPFYNCNFYWVPEQFDFRKIHHPTDQLSPAGLTSVKYTTKPTPCLTTPQSHR
STIPXRLDFRKIXYXVCPFYTLLFYRFXVNIFLFYVITTTLASLSVAAISXFSPILVRLT
SVHSPG
>
```
What we can see is that each **line is limited to 60 characters**. We want to re-format our file so it keeps the entire protein sequence on one line. 

To do this we can use this command (we will dive deeper into the powerful ```awk``` command later)

```bash
awk '/^>/ {printf("\n%s\n",$0);next; } { printf("%s",$0);}  END {printf("\n");}' < INPUT_FILE.fasta > OUTPUT_FILE.oneline.fasta
```
Replace the INPUT and OUTPUT file names with the names you used in the slurm script and execute the command

Now our output file should look more like this:

```bash
>Ca22chr1A_C_albicans_SC5314_27 [2243 - 2686] (3188363 nucleotides)
HSLFALATTSKHNCIDXTLLISCCIPPVVINXPRVVLLLIVQPANTTTTALTTPSSFRVAIPXTTRFTFPTAIDYSNYKLFYRPFSNXQAQRDTCLGIYNSFYSSFCICHAICPPPIIQPANTTATGIDNCFHCYDTTTDYMLFTQQT
>Ca22chr1A_C_albicans_SC5314_28 [2397 - 2807] (3188363 nucleotides)
LHPLHFVLQFPXPLGSHFPPPLTTQTTSCSIVPSPTSKHNEIHVWAFTIASTHHFASAMQSAHHPSSNQQTQPQRALTTASTAMTPPLTTCCSPSKHNTLHSSSSISHSTTAISTGSPSSLTSVKYTTPQINYPXPA
>Ca22chr1A_C_albicans_SC5314_29 [2770 - 2889] (3188363 nucleotides)
NTPPHRSTIPXRLDFRKIHYKAYPLSDYPSVPQINYPXPA
>Ca22chr1A_C_albicans_SC5314_30 [2850 - 2954] (3188363 nucleotides)
LPLSPTDQLSPAGLTSVKXTTXSAPSTLSSSIDXQ
>Ca22chr1A_C_albicans_SC5314_31 [2690 - 3067] (3188363 nucleotides)
HLAQFKFNFPFYNCNFYWVPEQFDFRKIHHPTDQLSPAGLTSVKYTTKPTPCLTTPQSHRSTIPXRLDFRKIXYXVCPFYTLLFYRFXVNIFLFYVITTTLASLSVAAISXFSPILVRLTSVHSPG
```

#### Step 11b - Extracting the sequences for Ca22chr1A_C_albicans_SC5314

Now we will extract all the sequences that contain the identifier "Ca22chr1A_C_albicans_SC5314" and save them to a new file

```bash
grep -A 1 "Ca22chr1A_C_albicans_SC5314" OUTPUT_FILE.oneline.fasta > Ca22chr1A_C_albicans_SC5314.fasta
```
Here is a breakdown of the command

```-A 1``` - This invokes the ``-A`` option which is also known as ``--after-context=NUM   print NUM lines of trailing context``. In this case, we want to obtain any sequence matching our search (which will be the header line starting with >) AND the sequence data saved directly after it. 

``` ""Ca22chr1A_C_albicans_SC5314"``` - This is the pattern we are searching for

```OUTPUT_FILE.oneline.fasta``` - This is the file we are searching in

If we were to stop the command at ```grep -A 1 "Ca22chr1A_C_albicans_SC5314" OUTPUT_FILE.oneline.fasta``` it would print the results directly onto the terminal. We want to **direct the output to a new file**. To do this we need to use the command ```>```. 

```> Ca22chr1A_C_albicans_SC5314.fasta``` - This directs the output of the command to be saved in a new file. **_BEWARE_** This will **automatically overwrite** any file that already has our output name. If this happens **you will not be able to undo this**



**_TIP_** _Options in programs typically have a short version that is one letter long and a long version that may be multiple characters long. For the option above for obtaining 1 line after your search, you can either use_ ```-A 1``` _or you can use the long version_ ```--after-context=1```. _Options are almost always case-sensitive. The_ ```-A``` _command means after-context while_ ```-a``` _is used while searching binary/compressed files_

### Knowledge Check 5
What percent of the ORFs annotated on the contig Ca22chr1A_C_albicans_SC5314 were annotated on the REVERSE strand? 

<details>
 <summary>Answer</summary>
There are **940** open reading frames on Ca22chr1A_C_albicans_SC5314

```
grep -c ">" Ca22chr1A_C_albicans_SC5314.fasta
940

```

There are  478 open reading frames on this contig that are in the Reverse strand

```
grep -c "REVERSE" Ca22chr1A_C_albicans_SC5314.fasta
478
```

Therefore 478/940 or 50.85% are on the reverse strand.
</details> 
