[OpenMP](https://en.wikipedia.org/wiki/OpenMP) is an application
programming interface that lets you write parallel programs on shared
memory platforms. In a parallel section, OpenMP code can create multiple
threads that run on separate cores, executing their shares of the total
workload concurrently. OpenMP is suited for the Mahuika and Māui HPCs as
each platform has 36, respectively 40 physical cores per node.  Each
physical core can handle up to two threads in parallel using
[Hyperthreading](https://support.nesi.org.nz/hc/en-gb/articles/360000568236).
Therefore you can run up to 72 threads on Mahuika and 80 threads on
Māui. 

The environment variable that controls the number of threads is
OMP\_NUM\_THREADS, e.g.,

    export OMP_NUM_THREADS=16

allows OpenMP code to fork 16 threads. To make sure that resources
requested from SLURM and used by your program are consistent, is usually
a good idea to set

    #SBATCH --cpus-per-task=16
    [...]
    export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

in your SLURM script - although this can sometimes be more complicated,
e.g., with [TensorFlow on
CPUs](https://support.nesi.org.nz/hc/en-gb/articles/360000997675).

On Mahuika, you will be charged for the number of physical cores that
you requested - the second logical core on a physical core is free,
although SLURM always reports both cores. On Māui you will be charged
for full nodes.

In order to achieve good and consistent parallel scaling, additional
settings may be required. This is particularly true on Mahuika whose
nodes are shared between different SLURM jobs. Following are some
settings that can help improve scaling and/or make your timings more
consistent, additional information can be found in our article [Thread
Placement and Thread
Affinity](https://support.nesi.org.nz/hc/en-gb/articles/360000995575).

1\. \--hint=nomultithread. Set this in conjunction with srun or sbatch
to tell SLURM that you don\'t want to use hyperthreads. Your program
will only be presented with physical cores. Inversely,
\--hint=multithread will request two threads per physical core. If
\--hint is not set, SLURM will currently assume \--hint=multithread by
default.

2\. OMP\_PROC\_BIND. Set this to \"true\" to pin the threads down during
program execution. By default, threads may migrate from one core to
another, depending on the load on the node. In an HPC setting, it is
generally advisable to pin the threads to avoid delays caused by thread
migration.

3\. OMP\_PLACES. Set this to \"cores\" if you want to pin the threads to
physical cores, or to \"threads\" if you want to use hyperthreading. 

The effect of each setting is illustrated below. In this experiment we
measured the execution time twice of the finite difference
code [upwindCxx -numCells 256 -numSteps
10.](https://github.com/pletzer/fidibench) The code was built with the
gimpi/2018b toolchain on Mahuika.

+-----------------+-----------------+-----------------+-----------------+
| Number of       | \--hint not     | \--hint=[no]{   | \--h            |
| physical cores  | used,           | .wysiwyg-color- | int=multithread |
|                 | OMP\_PROC\_BIND | red}multithread |                 |
|                 | and OMP\_PLACES |                 | OMP\_           |
|                 | unset           | OMP\_           | PROC\_BIND=true |
|                 |                 | PROC\_BIND=true |                 |
|                 |                 |                 |  OMP\_PLACES    |
|                 |                 |  OMP\_PLAC      | =[threads]{.wys |
|                 |                 | ES=[cores]{.wys | iwyg-color-red} |
|                 |                 | iwyg-color-red} |                 |
+-----------------+-----------------+-----------------+-----------------+
| 1               | 1m43s, 1m42s    | 1m42s, 1m42s    | 1m30s, 1m30s    |
+-----------------+-----------------+-----------------+-----------------+
| 2               | 1m30s, 1m31s    | 1m03, 55s       | 56s, 56s        |
+-----------------+-----------------+-----------------+-----------------+
| 4               | **[58s,         | **[45s,         | **[27s,         |
|                 | 1m27s]{.wysiw   | 41s]{.wysiwyg-  | 2               |
|                 | yg-color-red}** | color-blue90}** | 8s]{.wysiwyg-co |
|                 |                 |                 | lor-green110}** |
+-----------------+-----------------+-----------------+-----------------+
| 8               | 24s, 27s        | 18s, 17s        | 16s, 16s        |
+-----------------+-----------------+-----------------+-----------------+

 
-

Results
-------

In the default case, \--hint was not used and the environment variables
OMP\_PROC\_BIND and OMP\_PLACES were not set. Significant variations of
execution times are sometimes observed due to the random placement of
threads, which may or may not share a physical core. 

The third column shows the settings for the case with no multithreading.
The fourth column places 2 threads per physical cores (i.e.
OMP\_NUM\_THREADS = 2 times the number of physical cores) and this often
gives the best performance.

 Bottom line
------------

Be explicit by using \--hint and setting OMP\_PROC\_BIND and
OMP\_PLACES. In many cases we expect one of the following to work best:

  -------------------- ----------------------- ---------------------- ---------------------
  1 thread per core    \--hint=nomultithread   OMP\_PROC\_BIND=true   OMP\_PLACES=cores
  2 threads per core   \--hint=multithread     OMP\_PROC\_BIND=true   OMP\_PLACES=threads
  -------------------- ----------------------- ---------------------- ---------------------

Let us know if you find other settings to work better for you.