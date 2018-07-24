# Multithreading #

Suppose you have a task that includes a combination of multiple smaller tasks with different functions and the smaller tasks may be dependent on each other. What we can run the code in a CPU which has multiple cores and we wished to run each of the smaller tasks into different cores of the CPU parallel. This kind of task is simple to define in python but writing multithreaded code is difficult in *R* and sometimes not safe. But we can utilized the multiple cores in a single CPU to run the same function with different input using using some palatalization commands like `mclapply`

To give a flavor of how multithreading works, let us rewrite the *R* code in a way that search for multiple processor to perform a single task.

    require(parallel)
    r_example <- function(){
      x <- mclapply(1:5, function(i) rnorm(2, mean = 0, sd = 5), mc.cores = 5)
      return(unlist(x))
    }
    r_example()

We are using `parallel` package for parallel processing in `R`. We are interested to generate 10 observation from normal random variable. We have 5 task to do (`1:5`). `mclaaply` distributes the whole task into 5 processors and each processors will generate two numbers in parallel and will return a list of length 5. Note that, SLURM cannot return list type output to the output files.

In this situation, we need a CPU which has at least 5 cores.Let us request a CPU with 5 cores as follows;

    #!/bin/bash
    #SBATCH --job-name=example1#Job name
    #SBATCH --mail-type=ALL # Mail events
    #SBATCH --mail-user=yushuf@ufl.edu #Where to send mail
    #SBATCH --ntasks=1
    #SBATCH --cpus-per-task=5
    #SBATCH --mem-per-cpu=1gb # Per processor memory
    #SBATCH -t 00:10:00 # Wall time 0 hours 10 minutes 0 seconds
    #SBATCH --error=example1_%j.txt # Error files if error. %j is job number
    #SBATCH --output=mthreading_%j.txt

    module load R/3.4.3
    Rscript r_example2.r


`--ntasks` tells, I have requested 1 CPU with 5 cores `--cpus-per-task=5` option. Tasks cannot be split across several compute nodes, so requesting several CPUs with 5 cores (`--cpus-per-task=5`). 
`--cpus-per-task` option will ensure all CPUs are allocated on the same compute node. By contrast, requesting the same amount of CPUs with the --`ntasks` option may lead to several CPUs being allocated on several, distinct compute nodes.

To check multicore is helping, In R code, increase the repetition from 1:5 to 1:500. Change `mc.cores= 1` and `10` use the command after submitting the batch for `mc.core` 1 and 10. 

    sacct -j 21061470 --format="JobID, Elapsed, AllocCPUS,CPUTime,NNodes,ntasks"
    # 21061470 is the job id
See the difference in elapsed time.

A task cannot be split into different CPU. So to perform a task in multithreading, Requesting more than one CPU does not make sense. But we can repeat the whole threading into multiple CPU using the Open MPI which is discussed in **hybrid processing**. 

