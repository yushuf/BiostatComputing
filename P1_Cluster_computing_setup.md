# A cluster computing system #


A cluster computing system consists of a set of nodes (comparable to a personal computer) connected to each other. Each node has multiple processors and each processor are segmented in multiple cores. 

In HPC, we often require to request computing setup suitable for our computation. CPU is the smallest unit that we can request. Depending upon system configuration A CPU can be either a core or a thread (a core with one or more hardware context within that). Our conventional explanation of CPU(central processing unit) implies the processor which is little confusing with the idea of CPU in cluster setup. For simplicity let us consider a CPU refers to a core of a processor within a node in HPC.

![alt text](https://github.com/yushuf/BiostatComputing/blob/master/Images/nodes.png)

Layout of a cluster computing system. Source: Frachtenberg (2008)

## Yale HPC ##



## First time in HPC ##
To use the Yale High Performance Computing (HPC) we have to go through the steps as follows;

step 1. Open an account in HPC using [Yale HPC getting started guide link](https://research.computing.yale.edu/support/hpc/getting-started) 

Step 2. Configuring our own computer system to connect HPC. This might different for different operating system but in any ways that is not too hard as guided in [Yale HPC user guide](https://research.computing.yale.edu/support/hpc/user-guide). 

![alt text](https://github.com/yushuf/BiostatComputing/blob/master/Images/cluster.png)

Structure of HPC.  Source: Yale HPC getting started guide

Once we have an account and our own computer is ready we can have access to HPC any time from any places (VPN support may needed to connect from outside the Yale network).  When we get connected to HPC, we, primarily connect to a node called as login node. This is the get way to access all the other nodes and storage of HPC. We can operate the login node from our own PC.  We can execute any Linux command in the node as we do in our linux PCs.

## Tools for HPC ##
Computing involves some programming languages (e.g. R, Python, Julia). To run the codes faster in HPC by utilizing its capacity, we need to know the simple Linux utility for resource management (SLURM). SLURM is a language to write codes on how we will be scheduling a job and how many nodes or CPUs do we need. In addition to SLURM,  we will need the Bash scripting codes for writing jobs, file management and post data processing. We do not need to know each and every commands of SLURM and Bash immediately rather we will move forward based on our needs.

We will start with interactive sessions in HPC. During interactive session nothing to do with SLURM. All we need is Bash/Linux commands

Interactive sessions with HPC

