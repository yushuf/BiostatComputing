# Simple processing #

Let us play with the following R code

    r_example <- function(){
     x <- lapply(1:10, function(i) rnorm(1, mean = 0, sd = 1))
     round( unlist(x), 4)
    }
    x <- lapply(1:100, function(i) r_example())
    as.data.frame(do.call(rbind, x))
The function *r_example()* will produce an instance of ten independent normally distributed random variables. The code returns a data frame of 100 instances. By default R runs with A single processor (as most of the programs do). Let us think run the code with multiple processor in cluster. Let us try the code in cluster with as single processor first.

We can run that in login node through interactive session. But if the task is large may be killed in login node.

Let us create a job. Assume the above file stored as example1.r. Here we will keep track on job processing time which is not comparable to the CPU time.

    #!/bin/sh
    #SBATCH --job-name=example1#Job name
    #SBATCH --mail-type=ALL # Mail events
    #SBATCH --mail-user=yushuf@ufl.edu #Where to send mail
    #SBATCH --ntasks=1 # #of CPU requested
    #SBATCH --mem-per-cpu=1gb # Memory per CPU requested
    #SBATCH -t 00:10:00 # requested time: 0 hours 10 minutes 0 seconds
    #SBATCH --error=example1_%j.txt # Error files if error. %j is job number
    #SBATCH --output=res.out

    module load R/3.4.3
    Rscript r_example.r
Keep the first line as usual. Job name, mail and mail user are important to get notification from the HPC about the progress or any other events like error and so on. All these are important when we have multiple jobs running in the same time in cluster and we want to monitor their status.

We requested 1 CPU with memory 1GB for 10 minutes. The job has only one task to perform. Results will be written in the res.out a data frame of 100 instances.

    sacct -j 18656245 --format="JobID, Elapsed, AllocCPUS,CPUTime,NNodes,ntasks"

    JobID      Elapsed    AllocCPUS  CPUTime  NNodes   NTasks
    18656245   00:00:02     1		 00:00:02   1 		    1

sacct provides information about the job. It took two seconds to perform the the task. It is clear that the job utilized the whole resources that we had requested.

Change the option --ntask=5  then run the job. multiple processor never improved the run time. Only one processor worked and the rest were idle. Let us try to utilize the 4 extra processor in parallel with the following modified R code

    sacct -j 18656716 --format="JobID, Elapsed, AllocCPUS, CPUTime, NNodes, ntasks"
     JobID      Elapsed   AllocCPUS  CPUTime  NNodes   NTasks
    18656716.ba+ 00:00:03   4       00:00:12   1        1
CPU time =  Elapsed time X Number of CPU. Same time elapsed as for single CPU and rest of the CPUs were idle to perform one task. Allocated memory 1GB per processor.

 Summary
We can define a single task to run on a single processor. A task is defined based on how we get the output. Though we repeated a function within a single process 100 times, this is a single task performed by a single processor.

Next we will utilize 20 processor to get the same result. Our intention is to generate a 5 instances in each processor.
