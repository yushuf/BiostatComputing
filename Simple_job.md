# Jobs #
A job consists of two parts

Request for resources:  Depending on the volume of the job and how quickly we want to get the output, the resource request varies. We fix the number of nodes, CPUs, expected time of execution, memory allocation required, how to get the outputs etc. in this part. This part will be written in *SLURM*.
Optimum resource allocation is important. A large allocation for a small job cause the delay in getting the resource free. Once, the resource allocated for the job most of the system will remain idle. On the other hand conservative allocation will require longer time to finish your job than expected and often dropped due to insufficient memory or time allocation.

Tasks : This part requires what must be needed to run our code. For example we need to create some output folder location, start required software such as *R*, *Python*, etc. commands to run codes and post processing codes for results. Tasks are usually written in Bash scripting code.
Example of a job

    #!/bin/sh
    #SBATCH --job-name=example#Job name
    #SBATCH --ntasks=1 # number of requested CPU
    #SBATCH --mem-per-cpu=1gb # Per processor memory
    #SBATCH -t 1:00:00 # Walltime 1 hours
    
    module load Python/3.5.1-foss-2016a R/3.3.1-foss-2016a
    python example1.py
    Rscript example2.R
    #give example consistent with farnam
Resource allocation commands starts with #sbatch.  SLURM will read the lines starts with # only. The first line is the shebang line specifies the interpreter. Remains unchanged until you are using bash commands and we have almost nothing to do with this interpreter. We should give a name of the job so that we can follow the progress of the job searching by the name in the list of the jobs running in the cluster. For this job, SLURM will allocate 1 CPU with 1 mb memory (RAM) for 1 hour.

Tasks describes to do load *Python 3.5.1* and *R 3.3.1* first then run codes in example1.py and example2.R.

Usually those jobs are created in a text file and stored in *.sbatch* extension. To submit the job in cluster use command.

    sbatch example.sbatch

While defining a job in cluster we should synchronize the tasks and resource request to run the task. Our objective is to allocate resources and define tasks so that we can perform multiple tasks in parallel to get output within a reasonable time. 

Second, Suppose, in the above example, the *python* code will run and retain a data set which will be used as input for *R* script. In this situation, we need a little plan for where the data will be stored and how *R* will locate the data. This kind of issues can be handled in this part. We will discuss these issues while discussing the resource allocation.

We will first describe some simpler jobs and will show gradually how we can design some complected jobs to get faster output. At first we will create a job and run that using SLURM

Next: [Creating a simple job](https://github.com/yushuf/BiostatComputing/blob/master/simple_processing.md)


