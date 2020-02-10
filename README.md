# What in the world is a FASTQ file?

FASTQ files contain unmapped sequence reads. These are the initial files we get back from a sequencing facility. 

For each sequence read the FASTQ file contains 4 lines of information:

| Line | Meaning | 
:----------|:-------------|
| 1 | Begins with @, contains information about read (like unique identifier, or a descriptor like sequence length) |
| 2 | The nucleotide sequence of the read |
| 3 | Begins with + and sometimes has same information as line 1 |
| 4 | A string of characters (of same length as line 2) that represent the quality (probability that corresponding nucleotide in line 2 is correct) | 


For more details, see [Wikipedia on FASTQ format](https://www.wikiwand.com/en/FASTQ_format) and [this link](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/lessons/02_assessing_quality.html). 




# Step-by-step tutorial to running fastqc on the HPC

*Super awesome useful link:* [http://hpc.sabanciuniv.edu/](http://hpc.sabanciuniv.edu/) 

Contains information on how to get started with HPC. Highly recommend going through it! The information here is a subset of that contained in this guide.

1. **Log in to HPC**: 
For mac & linux, open terminal and write this: `ssh username@10.39.60.250`. If the username is correct, you'll be asked to enter your password. Both the username and password should also be in the SABANCI HPC Cluster User Information email sent to you by someone from Compecta. Windows users, see the guide.

2. **Copy .fq file from your computer to HPC**:
For mac & linux, open terminal and `scp SP1.fq  username@10.39.60.250:~/workfolder`. (Replace username and write password.) This will copy the file `SP1.fq` from your current directory to your `workfolder` on HPC. 

This [StackExchange answer](https://unix.stackexchange.com/a/188289) has some useful information on `scp`. 

3. **View available modules**: 
Use `module avail`. Can also use `module avail fastqc` to find whether fastqc is available. 

4. **Get example job script from `/cta/share/`**: 
Set directory to `/cta/share/` like this: `cd /cta/share/`. Copy the `slurm_example.sh` file to your workfolder: `scp slurm_example.sh ~/workfolder/`. 

5. **Modify slurm_example.sh**: Open `slurm_example.sh` using an editor: e.g. `nano slurm_example.sh`. Scroll down to the `#Module File` comment and add this line to load the fastqc module: `module load fastqc-0.11.7-gcc-8.2.0-43xnlgy`. Add this line to run fastqc using the SP1.fq example file: `fastqc SP1.fq`. Save and exit editor.

6. **Submit your job**: Use `sbatch slurm_example.sh`

7. **Copy results to your computer**: Use `scp username@10.39.60.250:~/workfolder/SP1_fastqc.zip .` (from terminal on your machine!). Change username and enter your password. File should be in the directory you're in. Unzip and view the html file. 


## Other recommended links

* [Here](https://hbctraining.github.io/Intro-to-rnaseq-hpc-O2/lessons/02_assessing_quality.html) is some to-the-point, useful information on the contents of FASTQ files.
* [Official website](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) for fastqc. Super useful to look at example good and bad results.
* [Fastqc manual](https://dnacore.missouri.edu/PDF/FastQC_Manual.pdf) explains what the different analysis modules mean (see Section 3).

## Honorable Mentions
* Can also `git clone link/to/repo` in HPC. (but `scp` will surely be useful someday.)










