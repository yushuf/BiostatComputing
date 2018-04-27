# A cluster computing system #


A cluster computing system consists of a set of nodes (compared to an independent PC) connected. Each node has multiple processors and each processor are segmented in multiple cores. Depending upon system configuration A CPU can be either a core or a thread (a core with one or more hardware context within that). For simplicity let us think, a CPU refers to a core as was done in Yale High Performance Computing (HPC).

![alt text](https://github.com/yushuf/BiostatComputing/blob/master/Images/nodes.png)

A structure of cluster computing system. Source: Frachtenberg (2008)

Yale HPC

![](C:\Onedrive\remote computing\Yale HPC\cluster.png)
Structure of HPC.  Source: Yale HPC getting started guide

## First time in HPC ##
For new user of Yale High Performance Computing (HPC) please follow the [Yale HPC getting started guide link](https://research.computing.yale.edu/support/hpc/getting-started) to open an account to HPC. Then we have to configure our own computer system to connect HPC. This might different for different operating system but any way that is simple to as guided in [Yale HPC user guide](https://research.computing.yale.edu/support/hpc/user-guide). 

Assuming we already have everything setup and we can connect to the HPC. Once get connected, we, primarily connect to a computer called a login node. This is the get way to access all the clusters and storage. We can work with login node with our own PC.  We can execute any Linux command in the node from our PC.

## Tools for HPC ##
Computing involves with some programming languages (e.g. R, Python, Julia). To run the codes faster in HPC we need to know Simple Linux Utility for Resource Management (SLURM) to write codes on how we will be scheduling a job and of course the Bash scripting for writing jobs, file management and post data processing. We will be writing code for jobs in SLURM and Bash. We do not need to know each and every commands of SLURM and Bash immediately rather we will move forward based on our needs.

We will start with interactive sessions in HPC. During interactive session nothing to do with SLURM. All you need is Bash/Linux commands

Interactive sessions with HPC

