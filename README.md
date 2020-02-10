# Step-by-step tutorial to running fastqc on the HPC

### Part 1: Login & transfer files to HPC

*Super awesome useful link:* [http://hpc.sabanciuniv.edu/](http://hpc.sabanciuniv.edu/) 

Contains information on how to get started with HPC. Highly recommend going through it! The information here is a subset of that contained in this guide.

1. **Log in to HPC**: 
For mac & linux, open terminal and write this: `ssh username@10.39.60.250`. If the username is correct, you'll be asked to enter your password. Both the username and password should also be in the SABANCI HPC Cluster User Information email sent to you by someone from Compecta. Windows users, see the guide.

2. **Copy .fq file from your computer to HPC**:
For mac & linux, open terminal and `scp SP1.fq  username@10.39.60.250:/workfolder`. (Replace username and write password.) This will copy the file `SP1.fq` from your current directory to your `workfolder` on HPC. 

This [StackExchange answer](https://unix.stackexchange.com/a/188289) has some useful information on `scp`. 






