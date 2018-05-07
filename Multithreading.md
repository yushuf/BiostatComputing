# Multithreading #
Not finished
You can also create a job in the following way

    #!/bin/bash 
    #SBATCH --job-name=example1#Job name 
    #SBATCH --mail-type=ALL # Mail events 
    #SBATCH --mail-user=yushuf@ufl.edu #Where to send mail 
    #SBATCH --ntasks=1
    #SBATCH --cpus-per-task=4
    #SBATCH --mem-per-cpu=1gb # Per processor memory 
    #SBATCH -t 00:10:00 # Wall time 0 hours 10 minutes 0 seconds 
    #SBATCH --error=example1_%j.txt # Error files if error. %j is job number 
    #SBATCH --output=res%j.out 
    
    module load R/3.4.3 
    Rscript r_example2.r



while CPUs, for the multithreaded programs, are requested with the --cpus-per-task option. Tasks cannot be split across several compute nodes, so requesting several CPUs with the --cpus-per-task option will ensure all CPUs are allocated on the same compute node. By contrast, requesting the same amount of CPUs with the --ntasks option may lead to several CPUs being allocated on several, distinct compute nodes.

Next: [Job Array](https://github.com/yushuf/BiostatComputing/blob/master/Array.md)
