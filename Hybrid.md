# Hybrid Processing #

Let us try the r code used in multithreading 

    # r_example2.r
    require(parallel)

    r_example <- function(){
    x <- mclapply(1:5, function(i) rnorm(1, mean = 0, sd = 1), mc.cores = 5)
    return( unlist(x))
    }
    
    r_example()


The R code generates 5 numbers using rnorm function in parallel using 5 cores. We want to repeat the process 10 times 5 different CPUs so that we get 50 realizations. 

We have to use all three of the `--ntasks`, `--cpus-per-task`, and `--nodes` flags.`--ntasks` equals the number of MPI tasks (here 10), `--cpus-per-task` equals the number of `OMP_NUM_THREADS` (5 cores) and `--nodes` is the number of nodes required to fit `--ntasks`. Here 2 to 10 would work. As we mentioned, larger resource allocation sometimes take time due to the queue to get the requested resource free. Also sometimes the additional resource remains unused. 

    
    #!/bin/bash
    #SBATCH --job-name=hybrid
    #SBATCH --output=res.txt
    #SBATCH --ntasks=10
    #SBATCH --cpus-per-task=5
    #SBATCH --nodes=4
    #SBATCH --time=20:00
    
    export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
    module load R/3.4.1-foss-2016b
    mpirun Rscript r_example2.r

    
the code saved as `hybrid.sbatch`, export `OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK` command set the number of threads using the environment variable `OMP_NUM_THREADS` in each CPU. Submit the job returns the results in `res.txt`
 
    [ms3628@farnam1 hpc]$ cat hydrid_job.txt
    
    The following have been reloaded with a version change:
      1) PCRE/8.39-foss-2016b => PCRE/8.38-foss-2016b
    
    Loading required package: parallel
    Loading required package: parallel
    [1] -1.8673061 -1.2322739  0.2023437  0.6581562 -0.8485164
    [1] -0.18707090 -0.94632844 -0.40259718  1.41495897 -0.07529816
    Loading required package: parallel
    Loading required package: parallel
    [1] 1.0504148 0.5803681 1.3206354 1.0307997 1.5246292
    [1] -0.3186228 -1.1512348 -0.6106463 -0.9628663  1.6056388
    Loading required package: parallel
    Loading required package: parallel
    Loading required package: parallel
    Loading required package: parallel
    [1]  0.3588247  1.3256367  0.6304067 -1.4264329  0.5035663
    [1] -0.1045564  1.3202804 -1.1610167 -0.1794284  0.4917891
    [1]  1.74036240  0.04694376 -0.98490901 -0.87968579 -0.54265604
    [1] -0.46143383  0.48995693  0.86201929  0.01010628 -0.67084898
    
Post `Bash` processing is required to use the data for further analysis. 

Always check your submission as follows

    sacct -j 21062310 --format="JobID, Elapsed,  AllocCPUS,CPUTime,NNodes,ntasks"

       JobID       Elapsed  AllocCPUS CPUTime    NNodes    NTasks
    ------------ ---------- ---------- ---------- -------- --------
    21062310       00:00:05  40        00:03:20      4
    21062310.ba+   00:00:05  5         00:00:25      1       1
    21062310.ex+   00:00:05  40        00:03:20      4       4
    21062310.0     00:00:02  15        00:00:30      3       3
    