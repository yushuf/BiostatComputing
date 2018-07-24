# Simple parallel processing using bash loop #

We can run the process multiple times in different processors using the bash loop. The trick is to submit a job multiple times using a loop and the outcome file will be named by the job number. After that we will accumulate the results from all outcome data files to get the desired data. Let us modify the previous *R* code.

    File Name: r_example2.r

    r_example <- function(){
    x <- lapply(1:10, function(i) rnorm(1, mean = 0, sd = 1))
    round( unlist(x), 4)
    }
    r_example()

File name: example2.sbatch

    #!/bin/bash
    #SBATCH --job-name=example1#Job name
    #SBATCH --mail-type=ALL # Mail events
    #SBATCH --mail-user=yushuf@ufl.edu #Where to send mail
    #SBATCH --ntasks=1 # Cannot exceed the maximum allocation
    #SBATCH --mem-per-cpu=1gb # Per processor memory
    #SBATCH -t 00:10:00 # Wall time 0 hours 10 minutes 0 seconds
    #SBATCH --error=example2_%j.txt # Error files if error. %j is job number
    #SBATCH --output=res%j.out

    module load R/3.4.3
    Rscript r_example2.r

The output file names of each of the job is *res* followed by the job number *%j*. For a single job, we have requested 1 CPU with 1 GB memory for 10 minutes. In line 3 and 4, we requested to mail all updated regarding the job by email notification. This is an useful option but not required. The *error* option in 8 is helpful to monitor the errors or warnings which will be sotred in the file named *example2* followed by job number with extexsion *.txt*. If there were not error, this file will remain blank. Let us run the code 100 times

    for i in {1..100}; do sbatch example2.sbatch; done

this bash command will provide 100 res*.out files. Let us append them in a single file.

for example in this case the combined output looks like

     cat res*.out
    [1] 2.3194 0.4334 2.2647 -0.3612 -2.1461 1.6223 -1.3583 0.7752 -0.2327
    [10] 0.3758
    [1] 0.7189 -0.4648 -2.3699 -0.4917 0.1180 -1.0486 -0.1197 0.6385 -2.4213
    [10] 0.4233 ...


To get this as a data frame or matrix for further analysis, we can use regular expression in bash commands to remove the *R* index [1] and [10] and taking each single output in a single line and append the. Here is an option to do that.

    for file in res*.out
    do
    awk -F" " '{print $2,$3, $4, $5, $6, $7, $8, $9}' $file > t_"$file"
    echo $(cat t_"$file" )> t_"$file"
    done
    cat t_*.out <- dat.out

Now the data is ready to import in *R* for further analysis. It will be a tab delimited data file without any variable name.


    [ms3628@farnam1 hpc]$cat dat.out
    -0.5870 1.9116 0.8395 -0.4478 -0.1295 -0.6501 -0.4614 0.4746 -0.4019
    -0.5654 1.6447 -0.0111 1.1727 1.1582 0.2432 0.1597 0.3563 0.1778
    ...
## Summary ##

We can use multiple CPU from command line or by looping the same job multiple times. The good thing for this process is, you don't need to wait for a Number of CPU free at a time. Whenever a CPU is free, you will be allocated that for processing a job.

The output requires post processing to use them for further analysis. Bash commands are useful to handle them.

The problem with this method is that in each job the cluster is doing the same things repeatedly such as allocating resources, loading the software and running the R code from the very first line. We can make it smart with Massage Passing Interface (MPI)
