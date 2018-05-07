# Message Passing Interface (MPI) #
MPI is a standardized architecture to perform parallel computing. This is easy to implement with the module openMPI.

MPI read the function and input and pass them to multiple processors to perform parallel and then retain the output serially again.

Define the job as follows

    #!/bin/bash
    #SBATCH --job-name=example3#Job name
    #SBATCH --mail-type=ALL # Mail events
    #SBATCH --mail-user= #Where to send mail
    #SBATCH --ntasks=10 # Cannot exceed the maximum allocation
    #SBATCH --mem-per-cpu=1gb # Per processor memory
    #SBATCH --time=00:10:00 # Wall time 0 hours 10 minutes 0 seconds
    #SBATCH --error=example1_%j.txt # Error files if error. %j is job
    #SBATCH --output=res.out
    
    module load OpenMPI/2.1.1-GCC-6.4.0-2.28 R/3.4.1-foss-2016b
    srun Rscript r_example2.r
Requested 10 CPUs for 10 minutes with 1GB memory. The difference with previous job is loading *openMPI* and the `srun` command. To run openMPI, the module *GCC* will be loaded automatically. The command`srun` created 10 copies of the task to the processors and returned the results in the output file res.out. The result would look like this

    $ cat res.out
     [1] 1.8874 0.2208 0.9983 0.0737 -0.6017 -0.4450 0.9205 0.8273 -1.1002
    [10] -0.6830
     [1] -0.2565 2.3278 -0.5796 -0.0532 -0.8855 -0.6632 -0.3592 1.0792 0.0356
    [10] -1.0024
    ...
As usual, you might requires some post processing to use the output further.

    $ sacct -j 7007932 --format="JobID, Elapsed, AllocCPUS,CPUTime,NNodes,ntasks"
     JobID    Elapsed   AllocCPUS  CPUTime    NNodes   NTasks
    7007932.0 00:00:01   10         00:00:10   3          10


We performed 10 tasks with 10 CPUs with shortest time elapsed.


One thing we should mention, the `--ntasks` option will allocate 1 CPU per task no matter where it is. May be the CPUs are in different nodes. 

Since only one CPU allocated for each of the task, if a task contains a processing that you intended to run parallel into multiple processor, MPI would not help. In that case we need a CPU with multiple cores which is sometimes referred as multithreaded program.


Next: Shared memory (OpenMP)