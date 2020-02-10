## What in the world is a FASTQ file?

FASTQ files contain unmapped sequence reads. These are the initial files we get back from a sequencing facility. 

For each sequence read the FASTQ file contains 4 lines of information:

| Line | Meaning | 
:----------|:-------------|
| 1 | Begins with @, contains information about read (like unique identifier, or a descriptor like sequence length) |
| 2 | The nucleotide sequence of the read |
| 3 | Begins with + and sometimes has same information as line 1 |
| 4 | A string of characters (of same length as line 2) that represent the quality (probability that corresponding nucleotide in line 2 is correct) | 

Let's check our toy example and look for this information.



### What's the deal with the weird looking quality scores?

Using these character codes, we have one character per nucleotide (instead of numbers like 0 to 40). This reduces file size. 

```
 Quality encoding: !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHI
                   |         |         |         |         |
    Quality score: 0........10........20........30........40
```

Side note to break the flow: This mapping corresponds to Phred-33. There also exists a Phred-66 mapping, but apparently it is not used as frequently in new NGS data [src](http://seqanswers.com/forums/showthread.php?t=81978). 

### Uh, so each character stands for a number. What does the number mean?

The number is the _quality score_ `Q`. It can be converted into the _probability  that the nucleotide called is wrong_ `P` using the following formula.

```
Q = -10 x log10(P), where P is the probability that a base call is erroneous
```

Basically, this just means that:

| Phred Quality score Q | Probability of incorrect base call P | Base call accuracy  | 
:----------|:-------------|:-------------|
| 10 | 1 in 10 | 90% |
| 20 | 1 in 100 | 99% |
| 30 | 1 in 1000 | 99.9% |
| 40 | 1 in 10,000 | 99.99% |


So, the higher the quality score Q corresponding to quality encoding character is, the more we are confident that the character at a given position in the read is accurate.  


Sources (and for more details, see): [Wikipedia on FASTQ format](https://www.wikiwand.com/en/FASTQ_format) and [this tutorial](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/lessons/02_assessing_quality.html). 


## Step-by-step tutorial to running fastqc on the HPC

*Super awesome useful link:* [http://hpc.sabanciuniv.edu/](http://hpc.sabanciuniv.edu/) 

Contains information on how to get started with HPC. Highly recommend going through it! The information here is a subset of that contained in this guide.

1. **Log in to HPC**: 
For mac & linux, open terminal and write this: `ssh username@10.39.60.250`. If the username is correct, you'll be asked to enter your password. Both the username and password should also be in the SABANCI HPC Cluster User Information email sent to you by someone from Compecta. Windows users, see the guide.

2. (Option 1) **Clone repo with data from github**:
In HPC, clone repository with data using: `git clone https://github.com/nabiafshan/fastqc-hpc-tutorial.git`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
OR 

2. (Option 2)
**Copy file from your computer to HPC**:
for mac & linux, open terminal and `scp SP1.fq  username@10.39.60.250:~/workfolder`. (Replace username and write password.) This will copy the file `SP1.fq` from your current directory to your `workfolder` on HPC. Windows users, see the guide.
This [StackExchange answer](https://unix.stackexchange.com/a/188289) has some useful information on `scp`. 

3. **View available modules**: 
Use `module avail`. Can also use `module avail fastqc` to find whether fastqc is available. 

4. **Get example job script from `/cta/share/`**: 
Set directory to `/cta/share/` like this: `cd /cta/share/`. Copy the `slurm_example.sh` file to your workfolder: `scp slurm_example.sh ~/workfolder/fastqc-hpc-tutorial/`. 

5. **Modify slurm_example.sh**: Go to directory with the data and open `slurm_example.sh` using an editor: e.g. `nano slurm_example.sh`. Scroll down to the `#Module File` comment and add this line to load the fastqc module: `module load fastqc-0.11.7-gcc-8.2.0-43xnlgy`. Add this line to run fastqc using the SP1.fq example file: `fastqc SP1.fq`. Save and exit editor.

6. **Submit your job**: Use `sbatch slurm_example.sh`

7. **Copy results to your computer**: Use `scp username@10.39.60.250:~/workfolder/SP1_fastqc.zip .` (from terminal on your machine!). Change username and enter your password. File should be in the directory you're in. Unzip and view the html file. 


## Other recommended links


* [Official website](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) for fastqc. Super useful to look at example good and bad results.
* [Fastqc manual](https://dnacore.missouri.edu/PDF/FastQC_Manual.pdf) explains what the different analysis modules mean (see Section 3).












