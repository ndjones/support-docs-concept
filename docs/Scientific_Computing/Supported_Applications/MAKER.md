Local Customisations {#local-customisations dir="auto"}
====================

Since the MAKER control file *maker\_exe.ctl* is just an annoyance in an
environment module based system we have patched MAKER to make that
optional. If it is absent then the defaults will be used directly. 

Parallelism {#parallelism dir="auto"}
===========

MAKER can be used with MPI, though due to a complicated interaction
between Infiniband libraries and MAKER\'s use of forking it can\'t be
used across multiple nodes. So we recommend running large MAKER jobs
with up to 36 tasks on one node (ie: one full regular node), eg:

    #SBATCH --nodes=1
    #SBATCH --ntasks-per-node=36
    #SBATCH --mem-per-cpu=1500

    module load MAKER/2.31.9-gimkl-2020a
    srun maker -q

Resources
=========

MAKER creates many files in its output, sometimes hundreds of thousands.
 There is a risk that you exhaust your quota of inodes, so:

-   Don\'t run too many MAKER jobs simultaneously.
-   Delete unneeded output files promptly after MAKER finishes.  You can
    of course use `nn_archive_files` or `tar` to archive them first.

 