# A cluster computing system
A cluster computing system consists of a set of nodes (compared to an independent PC) connected. Each node has multiple processors and each processor are segmented in multiple cores. CPU in cluster is somewhat different that we usually know for a PC. In cluster, a CPU is a core with one or multiple threads (the number of threads can be assigned by the user if system permits). For simplycity, in a cluster, A CPU combines one or multiple CPU in it.

![](https://github.com/fcrawford/handbook/blob/master/HPC/Images/nodes.png)

A structure of cluster computing system. Source: Frachtenberg (2008)

Yale HPC

![](https://github.com/fcrawford/handbook/blob/master/HPC/Images/cluster.png)
Structure of HPC.  Source: Yale HPC getting started guide

So in cluster computing setup, we can built our won powerful computing system to for faster computing.

## First time in HPC

For new user of Yale High Performance Computing (HPC) please follow the [Yale HPC getting started guide link](https://research.computing.yale.edu/support/hpc/getting-started) to open an account to HPC. Then we have to configure our own computer system to connect HPC. This might different for different operating system but any way that is simple to as guided in [Yale HPC user guide](https://research.computing.yale.edu/support/hpc/user-guide).

Assuming we already have everything setup and we can connect to the HPC. Once get connected, we, primarily connect to a computer called a login node. This is the get way to access all the clusters and storage. We can work with login node with our own PC.  We can execute any Linux command in the node from our PC.

## Tools for HPC

Computing involves with some programming languages (e.g. R, Python, Julia). To run the codes faster in HPC we need to know Simple Linux Utility for Resource Management (SLURM) to write codes on how we will be scheduling a job and of course the Bash scripting for writing jobs, file management and post data processing. We will be writing code for jobs in SLURM and Bash. We do not need to know each and every commands of SLURM and Bash immediately rather we will move forward based on our needs.

We will start with interactive sessions in HPC. During interactive session nothing to do with SLURM. All you need is Bash/Linux commands

[**Interactive session in HPC**](https://github.com/fcrawford/handbook/blob/master/HPC/interactive.md)
