# Interactive sessions #

Interactive sessions are important to prepare the running environment and testing the code that runs in that environment. For example, we need a particular version of R with some specific libraries to run an R code, We may install the necessary software or libraries in HPC through interactive sessions. In interactive sessions, we can load or run any commands or codes interactively as we do in our Linux personal computers.

In cluster, each program such as *R* or Python are named a module. First we have to load the required program in the work space before running a code. For example, let us think, we need to load *R 3.4.3* the command

    module load R/3.4.1-foss-2016b

To load a module you must provide the module names as they stored in cluster. The module names after *"/"* are quite unpredictable. To get the right name and version you need to search with keywords. For example, to find the versions of *R* installed in cluster

    module spider R

## Installation ##
Suppose we need a version of software that is not installed in the cluster. We can request to the HPC support unit to install the specific program or we can install the program in our own work spaces. This is quite common in *R* or *Python* users to install the recent version of libraries. In that cases we can create your own collection of libraries to a designated folder in our work spaces. Here is a sample *R* library installation commands from the login node.

    [ms3628@farnam1 ~]$ module spider R
    [ms3628@farnam1 ~]$ module load R/3.4.1-foss-2016b
    [ms3628@farnam1 ~]$ mkdir myrlib
    [ms3628@farnam1 ~]$ chmod o+rwx myrlib
    [ms3628@farnam1 ~]$ R
    > install.packages("multiwayvcov", lib = "/ysm-gpfs/home/ms3628/myrlib")
    > ##Check you library paths
    > .libPaths()
    [1] "/gpfs/ysm/apps/software/R/3.3.1-foss-2016a/lib64/R/library"
    > ##Set your library to the library path and check##
    > .libPaths("/gpfs/ysm/home/ms3628/myrlib")
    > .libPaths()
    [1] "/gpfs/ysm/home/ms3628/myrlib"
    [2] "/gpfs/ysm/apps/software/R/3.4.1-foss-2016b/lib64/R/library"
    > ##Now you are all set to use the library as you do in your code with
    your personal computer##
    > q() # quite from R
Commands starts with [ are the bash commands and > are the r commands. A library "myrlib" has been created and an R library "multiwayvcov" installed into that folder. Finally we added the "myrlib" location to the library path of R so that while running, R search the library in my specified location.

## Writing a code from login node ##
Up to this point, we showed how we can command to do something in cluster. But we can write a series of commands in a file, store them and can run from the command prompt. Nano is the text editor available in cluster. Not so friendly like the other text editors in personal computers but okay.

Let us write a code to find the cluster adjusted  variance covariance matrix for a linear model estimators to check whether our library is working perfectly.

Start the text editor with a file name

    [ms3628@farnam1 hpc]$ nano vcov.r
and write the R code and save with *ctrl + x*

    #GNU nano 2.3.1 File: vcov.r
    library(multiwayvcov)
    x <- rnorm(100, 0,1)
    y <- 1 + .5*x + rnorm(100, 0, 1)
    clu <- rbinom(100, 1, .5)
    m1 <- lm(y ~ x)
    # Cluster by clu
    vcov_clu <- cluster.vcov(m1, clu)
    m1
    vcov_clu
Now run the file *vcov.r* from the terminal. Assuming R is already loaded into the work space. the *Rscript* command runs the codes written in the file *vcov.r*

    [ms3628@farnam1 hpc]$ Rscript vcov.r
    Call:
    lm(formula = y ~ x)
    Coefficients:
    (Intercept) x
     1.1369 0.6766

    (Intercept)  x
    (Intercept) 0.008703953 -0.003818426
      x        -0.003818426 0.001675144
working!!!

Once the working environment and code is ready, we can look for creating a job to run the process multiple times. First we will see what consists of a job and how SLURM and Bash works to create a job for HPC.
