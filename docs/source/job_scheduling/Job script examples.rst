**Job Script Examples**
--------------------------

**Basic examples**

**General blueprint for a jobscript**

You can save the following example to a file (e.g. testrun.sh) on FEDGEN
HPC. Comment the two cp commands that are just for illustratory purpose
(lines 46 and 55) and change the SBATCH directives where applicable. You
can then run the script by typing:

.. code-block:: python

  $ sbatch testrun.sh

Please note that all values that you define with SBATCH directives are
hard values. When you, for example, ask for 4000 MB of memory
(--mem=4000MB) and your job uses more than that, the job will be
automatically killed by the manager.

.. code-block:: python

        #!/bin/bash -l
    
    ##############################
    #       Job blueprint        #
    ##############################
    
    # Give your job a name, so you can recognize it in the queue overview
    #SBATCH --job-name=example
    
    # Define, how many nodes you need. Here, we ask for 1 node.
    # Each node has 16 or 20 CPU cores.
    #SBATCH --nodes=1
    # You can further define the number of tasks with --ntasks-per-*
    # See "man sbatch" for details. e.g. --ntasks=4 will ask for 4 cpus.
    
    # Define, how long the job will run in real time. This is a hard cap meaning
    # that if the job runs longer than what is written here, it will be
    # force-stopped by the server. If you make the expected time too long, it will
    # take longer for the job to start. Here, we say the job will take 5 minutes.
    #              d-hh:mm:ss
    #SBATCH --time=0-00:05:00
    
    # Define the partition on which the job shall run. May be omitted.
    #SBATCH --partition debug
    
    # How much memory you need.
    # --mem will define memory per node and
    # --mem-per-cpu will define memory per CPU/core. Choose one of those.
    #SBATCH --mem-per-cpu=1500MB
    ##SBATCH --mem=5GB    # this one is not in effect, due to the double hash
    
    # Turn on mail notification. There are many possible self-explaining values:
    # NONE, BEGIN, END, FAIL, ALL (including all aforementioned)
    # For more values, check "man sbatch"
    #SBATCH --mail-type=END,FAIL
    
    # You may not place any commands before the last SBATCH directive
    
    # Define and create a unique scratch directory for this job
    SCRATCH_DIRECTORY=/fedgenscratch/work/${USER}/${SLURM_JOBID}.allot.hpc.fedgen.net
    mkdir -p ${SCRATCH_DIRECTORY}
    cd ${SCRATCH_DIRECTORY}
    
    # You can copy everything you need to the scratch directory
    # ${SLURM_SUBMIT_DIR} points to the path where this script was submitted from
    cp ${SLURM_SUBMIT_DIR}/myfiles*.txt ${SCRATCH_DIRECTORY}
    
    # This is where the actual work is done. In this case, the script only waits.
    # The time command is optional, but it may give you a hint on how long the
    # command worked
    time sleep 10
    #sleep 10
    
    # After the job is done we copy our output back to $SLURM_SUBMIT_DIR
    cp ${SCRATCH_DIRECTORY}/my_output ${SLURM_SUBMIT_DIR}
    
    # In addition to the copied files, you will also find a file called
    # slurm-1234.out in the submit directory. This file will contain all output that
    # was produced during runtime, i.e. stdout and stderr.
    
    # After everything is saved to the home directory, delete the work directory to
    # save space on /fedgenscratch/work/
    cd ${SLURM_SUBMIT_DIR}
    rm -rf ${SCRATCH_DIRECTORY}
    
    # Finish the script
    exit 0


**Running many sequential jobs in parallel using job arrays**

In this example we wish to run many similar sequential jobs in parallel
using job arrays. We take Python as an example but this does not matter
for the job arrays:

*#!/usr/bin/env python*

**import** **time**

print('start at ' + time.strftime('%H:%M:%S'))

print('sleep for 10 seconds ...')

time.sleep(10)

print('stop at ' + time.strftime('%H:%M:%S'))

Save this to a file called “test.py” and try it out:

$ python test.py

start at 15:23:48

sleep for 10 seconds ...

stop at 15:23:58

Good. Now we would like to run this script 16 times at the same time.
For this we use the following script:

*#!/bin/bash -l*

*#####################*

*# job-array example #*

*#####################*

*#SBATCH --job-name=example*

*# 16 jobs will run in this array at the same time*

*#SBATCH --array=1-16*

*# run for five minutes*

*# d-hh:mm:ss*

*#SBATCH --time=0-00:05:00*

*# 500MB memory per core*

*# this is a hard limit*

*#SBATCH --mem-per-cpu=500MB*

*# you may not place bash commands before the last SBATCH directive*

*# define and create a unique scratch directory*

SCRATCH_DIRECTORY=/fedgenscratch/work/*${*\ USER\ *}*/job-array-example/*${*\ SLURM_JOBID\ *}*

mkdir -p *${*\ SCRATCH_DIRECTORY\ *}*

cd *${*\ SCRATCH_DIRECTORY\ *}*

cp *${*\ SLURM_SUBMIT_DIR\ *}*/test.py *${*\ SCRATCH_DIRECTORY\ *}*

*# each job will see a different ${SLURM_ARRAY_TASK_ID}*

echo "now processing task id:: " *${*\ SLURM_ARRAY_TASK_ID\ *}*

python test.py > output\_\ *${*\ SLURM_ARRAY_TASK_ID\ *}*.txt

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp output\_\ *${*\ SLURM_ARRAY_TASK_ID\ *}*.txt
*${*\ SLURM_SUBMIT_DIR\ *}*

*# we step out of the scratch directory and remove it*

cd *${*\ SLURM_SUBMIT_DIR\ *}*

rm -rf *${*\ SCRATCH_DIRECTORY\ *}*

*# happy end*

exit 0

Submit the script and after a short while you should see 16 output files
in your submit directory:

$ ls -l output\*.txt

-rw------- 1 user user 60 Oct 14 14:44 output_1.txt

-rw------- 1 user user 60 Oct 14 14:44 output_10.txt

-rw------- 1 user user 60 Oct 14 14:44 output_11.txt

-rw------- 1 user user 60 Oct 14 14:44 output_12.txt

-rw------- 1 user user 60 Oct 14 14:44 output_13.txt

-rw------- 1 user user 60 Oct 14 14:44 output_14.txt

-rw------- 1 user user 60 Oct 14 14:44 output_15.txt

-rw------- 1 user user 60 Oct 14 14:44 output_16.txt

-rw------- 1 user user 60 Oct 14 14:44 output_2.txt

-rw------- 1 user user 60 Oct 14 14:44 output_3.txt

-rw------- 1 user user 60 Oct 14 14:44 output_4.txt

-rw------- 1 user user 60 Oct 14 14:44 output_5.txt

-rw------- 1 user user 60 Oct 14 14:44 output_6.txt

-rw------- 1 user user 60 Oct 14 14:44 output_7.txt

-rw------- 1 user user 60 Oct 14 14:44 output_8.txt

-rw------- 1 user user 60 Oct 14 14:44 output_9.txt

**Packaging smaller parallel jobs into one large parallel job**

There are several ways to package smaller parallel jobs into one large
parallel job. The preferred way is to use Job Arrays. Browse the web for
many examples on how to do it. Here we want to present a more pedestrian
alternative which can give a lot of flexibility.

In this example we imagine that we wish to run 5 MPI jobs at the same
time, each using 4 tasks, thus totalling to 20 tasks. Once they finish,
we wish to do a post-processing step and then resubmit another set of 5
jobs with 4 tasks each:

*#!/bin/bash*

*#SBATCH --job-name=example*

*#SBATCH --ntasks=20*

*#SBATCH --time=0-00:05:00*

*#SBATCH --mem-per-cpu=500MB*

cd *${*\ SLURM_SUBMIT_DIR\ *}*

*# first set of parallel runs*

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

wait

*# here a post-processing step*

*# ...*

*# another set of parallel runs*

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

mpirun -n 4 ./my-binary &

wait

exit 0

The wait commands are important here - the run script will only continue
once all commands started with & have completed.

**Example on how to allocate entire memory on one node**

*#!/bin/bash -l*

*###################################################*

*# Example for a job that consumes a lot of memory #*

*###################################################*

*#SBATCH --job-name=example*

*# we ask for 1 node*

*#SBATCH --nodes=1*

*# run for five minutes*

*# d-hh:mm:ss*

*#SBATCH --time=0-00:05:00*

*# total memory for this job*

*# this is a hard limit*

*# note that if you ask for more than one CPU has, your account gets*

*# charged for the other (idle) CPUs as well*

*#SBATCH --mem=31000MB*

*# turn on all mail notification*

*#SBATCH --mail-type=ALL*

*# you may not place bash commands before the last SBATCH directive*

*# define and create a unique scratch directory*

SCRATCH_DIRECTORY=/fedgenscratch/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

mkdir -p *${*\ SCRATCH_DIRECTORY\ *}*

cd *${*\ SCRATCH_DIRECTORY\ *}*

*# we copy everything we need to the scratch directory*

*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*

cp *${*\ SLURM_SUBMIT_DIR\ *}*/my_binary.x *${*\ SCRATCH_DIRECTORY\ *}*

*# we execute the job and time it*

time ./my_binary.x > my_output

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp *${*\ SCRATCH_DIRECTORY\ *}*/my_output *${*\ SLURM_SUBMIT_DIR\ *}*

*# we step out of the scratch directory and remove it*

cd *${*\ SLURM_SUBMIT_DIR\ *}*

rm -rf *${*\ SCRATCH_DIRECTORY\ *}*

*# happy end*

exit 0

**How to recover files before a job times out**

Possibly you would like to clean up the work directory or recover files
for restart in case a job times out. In this example we ask Slurm to
send a signal to our script 120 seconds before it times out to give us a
chance to perform clean-up actions.

*#!/bin/bash -l*

*# job name*

*#SBATCH --job-name=example*

*# replace this by your account*

*#SBATCH --account=...*

*# one core only*

*#SBATCH --ntasks=1*

*# we give this job 4 minutes*

*#SBATCH --time=0-00:04:00*

*# asks SLURM to send the USR1 signal 120 seconds before end of the time
limit*

*#SBATCH --signal=B:USR1@120*

*# define the handler function*

*# note that this is not executed here, but rather*

*# when the associated signal is sent*

your_cleanup_function()

{

echo "function your_cleanup_function called at **$(**\ date\ **)**"

*# do whatever cleanup you want here*

}

*# call your_cleanup_function once we receive USR1 signal*

trap 'your_cleanup_function' USR1

echo "starting calculation at **$(**\ date\ **)**"

*# the calculation "computes" (in this case sleeps) for 1000 seconds*

*# but we asked slurm only for 240 seconds so it will not finish*

*# the "&" after the compute step and "wait" are important*

sleep 1000 &

wait

**OpenMP and MPI**

You can download the examples given here to a file (e.g. smpijob.sh) and
start it with:

$ sbatch mpijob.sh

**Example for an OpenMP job**

*#!/bin/bash -l*

*#############################*

*# example for an OpenMP job #*

*#############################*

*#SBATCH --job-name=example*

*# we ask for 1 task with 20 cores*

*#SBATCH --nodes=1*

*#SBATCH --ntasks-per-node=1*

*#SBATCH --cpus-per-task=20*

*# exclusive makes all memory available*

*#SBATCH --exclusive*

*# run for five minutes*

*# d-hh:mm:ss*

*#SBATCH --time=0-00:05:00*

*# turn on all mail notification*

*#SBATCH --mail-type=ALL*

*# you may not place bash commands before the last SBATCH directive*

*# define and create a unique scratch directory*

SCRATCH_DIRECTORY=/fedgenscratch/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

mkdir -p *${*\ SCRATCH_DIRECTORY\ *}*

cd *${*\ SCRATCH_DIRECTORY\ *}*

*# we copy everything we need to the scratch directory*

*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*

cp *${*\ SLURM_SUBMIT_DIR\ *}*/my_binary.x *${*\ SCRATCH_DIRECTORY\ *}*

*# we set OMP_NUM_THREADS to the number of available cores*

export OMP_NUM_THREADS=\ *${*\ SLURM_CPUS_PER_TASK\ *}*

*# we execute the job and time it*

time ./my_binary.x > my_output

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp *${*\ SCRATCH_DIRECTORY\ *}*/my_output *${*\ SLURM_SUBMIT_DIR\ *}*

*# we step out of the scratch directory and remove it*

cd *${*\ SLURM_SUBMIT_DIR\ *}*

rm -rf *${*\ SCRATCH_DIRECTORY\ *}*

*# happy end*

exit 0

**Example for a MPI job**

*#!/bin/bash -l*

*##########################*

*# example for an MPI job #*

*##########################*

*#SBATCH --job-name=example*

*# 80 MPI tasks in total*

*# FEDGEN HPC has 16 or 20 cores/node and therefore we take*

*# a number that is divisible by both*

*#SBATCH --ntasks=80*

*# run for five minutes*

*# d-hh:mm:ss*

*#SBATCH --time=0-00:05:00*

*# 500MB memory per core*

*# this is a hard limit*

*#SBATCH --mem-per-cpu=500MB*

*# turn on all mail notification*

*#SBATCH --mail-type=ALL*

*# you may not place bash commands before the last SBATCH directive*

*# define and create a unique scratch directory*

SCRATCH_DIRECTORY=/fedgenscratch/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

mkdir -p *${*\ SCRATCH_DIRECTORY\ *}*

cd *${*\ SCRATCH_DIRECTORY\ *}*

*# we copy everything we need to the scratch directory*

*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*

cp *${*\ SLURM_SUBMIT_DIR\ *}*/my_binary.x *${*\ SCRATCH_DIRECTORY\ *}*

*# we execute the job and time it*

time mpirun -np $SLURM_NTASKS ./my_binary.x > my_output

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp *${*\ SCRATCH_DIRECTORY\ *}*/my_output *${*\ SLURM_SUBMIT_DIR\ *}*

*# we step out of the scratch directory and remove it*

cd *${*\ SLURM_SUBMIT_DIR\ *}*

rm -rf *${*\ SCRATCH_DIRECTORY\ *}*

*# happy end*

exit 0

**Example for a hybrid MPI/OpenMP job**

*#!/bin/bash -l*

*#######################################*

*# example for a hybrid MPI OpenMP job #*

*#######################################*

*#SBATCH --job-name=example*

*# we ask for 4 MPI tasks with 10 cores each*

*#SBATCH --nodes=2*

*#SBATCH --ntasks-per-node=2*

*#SBATCH --cpus-per-task=10*

*# run for five minutes*

*# d-hh:mm:ss*

*#SBATCH --time=0-00:05:00*

*# 500MB memory per core*

*# this is a hard limit*

*#SBATCH --mem-per-cpu=500MB*

*# turn on all mail notification*

*#SBATCH --mail-type=ALL*

*# you may not place bash commands before the last SBATCH directive*

*# define and create a unique scratch directory*

SCRATCH_DIRECTORY=/fedgenscratch/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

mkdir -p *${*\ SCRATCH_DIRECTORY\ *}*

cd *${*\ SCRATCH_DIRECTORY\ *}*

*# we copy everything we need to the scratch directory*

*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*

cp *${*\ SLURM_SUBMIT_DIR\ *}*/my_binary.x *${*\ SCRATCH_DIRECTORY\ *}*

*# we set OMP_NUM_THREADS to the number cpu cores per MPI task*

export OMP_NUM_THREADS=\ *${*\ SLURM_CPUS_PER_TASK\ *}*

*# we execute the job and time it*

time mpirun -np $SLURM_NTASKS ./my_binary.x > my_output

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp *${*\ SCRATCH_DIRECTORY\ *}*/my_output *${*\ SLURM_SUBMIT_DIR\ *}*

*# we step out of the scratch directory and remove it*

cd *${*\ SLURM_SUBMIT_DIR\ *}*

rm -rf *${*\ SCRATCH_DIRECTORY\ *}*

*# happy end*

exit 0

If you want to start more than one MPI rank per node you can
use --ntasks-per-node in combination with --nodes:

*#SBATCH --nodes=4 --ntasks-per-node=2 --cpus-per-task=8*

This will start 2 MPI tasks each on 4 nodes, where each task can use up
to 8 threads

**Message passing example (MPI)**

#!/bin/bash

#

#SBATCH --job-name=test_mpi

#SBATCH --output=res_mpi.txt

#

#SBATCH --ntasks=4

#SBATCH --time=10:00

#SBATCH --mem-per-cpu=100

module load OpenMPI

srun hello.mpi

Request four cores on the cluster for 10 minutes, using 100 MB of RAM
per core. Assuming hello.mpi was compiled with MPI support, srun will
create four instances of it, on the nodes allocated by Slurm.

You can try the above example by downloading the example `hello world
program from
Wikipedia <https://en.wikipedia.org/wiki/Message_Passing_Interface#Example_program>`__ (name
it for instance wiki_mpi_example.c), and compiling it with

module load OpenMPI

mpicc wiki_mpi_example.c -o hello.mpi

The res_mpi.txt file should contain something like

We have 4 processors

Hello 1! Processor 1 reporting for duty

Hello 2! Processor 2 reporting for duty

Hello 3! Processor 3 reporting for duty

**Shared memory example (OpenMP)**

#!/bin/bash

#

#SBATCH --job-name=test_omp

#SBATCH --output=res_omp.txt

#

#SBATCH --ntasks=1

#SBATCH --cpus-per-task=4

#SBATCH --time=10:00

#SBATCH --mem-per-cpu=100

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

srun ./hello.omp

The job will be run in an allocation where four cores have been reserved
on the same compute node.

You can try it by using the `hello world program from
Wikipedia <https://en.wikipedia.org/wiki/Openmp#C>`__ (name it for
instance wiki_omp_example.c) and compiling it with

gcc -fopenmp wiki_omp_example.c -o hello.omp

The res_omp.txt file should contain something like

Hello World from thread 0

Hello World from thread 3

Hello World from thread 1

Hello World from thread 2

There are 4 threads

**Embarrassingly parallel workload example (job array)**

This setup is useful for problems based on **random draws** (e.g.
Monte-Carlo simulations). In such cases, you can have four programs
drawing 1000 random samples and combining their output afterwards (with
another program) you get the equivalent of drawing 4000 samples.

Another typical use of this setting is **parameter sweep**. In this case
the same computation is carried on several times by a given code,
differing only in the initial value of some high-level parameter for
each run. An example could be the optimisation of an integer-valued
parameter through range scanning in a **job array**:

#!/bin/bash

#

#SBATCH --job-name=test_emb_arr

#SBATCH --output=res_emb_arr.txt

#

#SBATCH --ntasks=1

#SBATCH --time=10:00

#SBATCH --mem-per-cpu=100

#

#SBATCH --array=1-8

srun ./my_program.exe $SLURM_ARRAY_TASK_ID

In that configuration, the command my_program.exe will be run eight
times, creating eight distinct jobs, each time with a different argument
passed with the environment variable defined by
slurm **SLURM_ARRAY_TASK_ID** ranging from 1 to 8, as specified by
the --array parameter.

The same idea can be used to process **several data files**. To
different instances of the program we must pass a different file to
read, based upon the value set in the $SLURM\_\* environment variable.
For instance, assuming there are exactly eight files in /path/to/data we
can create the following script:

#!/bin/bash

#

#SBATCH --job-name=test_emb_arr

#SBATCH --output=res_emb_arr.txt

#

#SBATCH --ntasks=1

#SBATCH --time=10:00

#SBATCH --mem-per-cpu=100

#

#SBATCH --array=0-7

FILES=(/path/to/data/\*)

srun ./my_program.exe ${FILES[$SLURM_ARRAY_TASK_ID]}

In this case, eight jobs will be submitted, each with a different
filename given as an argument to my_program.exe defined in the
array FILES[]. As the FILES[] Bash array is zero-indexed, the Slurm job
array IDs must also start at 0 so the argument is --array=0-7. One pain
point is that the number of files in the directory must match the number
of jobs in the array.

Note that the same recipe can be used with a numerical argument that is
not simply an integer sequence, by defining a Bash
array ARGS[] containing the desired values:

ARGS=(0.05 0.25 0.5 1 2 5 100)

srun ./my_program.exe ${ARGS[$SLURM_ARRAY_TASK_ID]}

Here again, the Slurm job array numbering must start at 0 to make sure
all items in the ARGS[] Bash array are processed.

**Warning**

If the running time of your program is small, say ten minutes or less,
creating a job array will incur a lot of overhead and you should
consider *packing* your jobs.

**Packed jobs example**

By default, the srun command in a submission script inherits all
non-GRES resource allocated in the job, but with the --exact parameter,
you can split the resource and allocate them to multiple steps in
parallel.

As an example, the following job submission script will ask Slurm for 8
CPUs, then it will run the myprog program 1000 times with arguments
passed from 1 to 1000. But with the -N1 -n1 -c1 --exact option, it will
control that at any point in time only 8 instances are effectively
running, each being allocated one CPU. You can at this point decide to
allocate several CPUs or tasks by adapting the corresponding parameters.

#! /bin/bash

#

#SBATCH --ntasks=8

for i in {1..1000}

do

srun -N1 -n1 -c1 --exact ./myprog $i &

done

wait

The for-loop can be replaced with GNU parallel if installed on your
system:

parallel -P $SLURM_NTASKS srun -N1 -c1 -n1 --exact ./myprog :::
{1..1000}

Similarly, many files can be processed with one job submission script.
The following script will run myprog for every file in /path/to/data,
but maximum 8 at a time, and using one CPU per task.

#! /bin/bash

#

#SBATCH --ntasks=8

for file in /path/to/data/\*

do

srun -N1 -n1 -c1 --exact ./myprog $file &

done

wait

Here again the for-loop can be replaced with another command, xargs:

find /path/to/data -print0 \| xargs -0 -n1 -P $SLURM_NTASKS srun -n1
--exclusive ./myprog

**Master/worker program example**

#!/bin/bash

#

#SBATCH --job-name=test_ms

#SBATCH --output=res_ms.txt

#

#SBATCH --ntasks=4

#SBATCH --time=10:00

#SBATCH --mem-per-cpu=100

srun --multi-prog multi.conf

With file multi.conf being, for example, as follows

0 echo I am the Master

1-3 echo I am worker %t

The above instructs Slurm to create four tasks (or processes), one
running echo 'I am the Master', and the other 3
running echo I am worker %t. The %t placeholder will be replaced with
the task id. This is typically used in a **producer/consumer** setup
where one program (the master) create computing tasks for the other
program (the workers) to perform.

Upon completion of the above job, file res_ms.txt will contain

I am worker 2

I am worker 3

I am worker 1

I am the Master

though not necessarily in the same order.

**Hybrid jobs**

You can mix multi-processing (MPI) and multi-threading (OpenMP) in the
same job, simply like this:

#! /bin/bash

#

#SBATCH --ntasks=8

#SBATCH --cpus-per-task=4

module load OpenMPI

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

srun ./myprog

or even a job array of hybrid jobs:

#! /bin/bash

#

#SBATCH --array=1-10

#SBATCH --ntasks=8

#SBATCH --cpus-per-task=4

module load OpenMPI

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

srun ./myprog $SLURM_ARRAY_TASK_ID

**GPU jobs**

If you want to claim a GPU for your job, you need to specify the
GRES `Generic Resource
Scheduling <https://slurm.schedmd.com/gres.html>`__ parameter in your
job script. Please note that GPUs are only available in a specific
partition whose name depends on the cluster.

#SBATCH --partition=PostP

#SBATCH --gres=gpu:1

A sample job file requesting a node with a GPU could look like this:

#!/bin/bash

#SBATCH --job-name=example

#SBATCH --ntasks=1

#SBATCH --time=1:00:00

#SBATCH --mem-per-cpu=1000

#SBATCH --partition=gpu

#SBATCH --gres=gpu:1

module load CUDA

srun ./my_cuda_program

**Settings for OpenMP and MPI jobs**

**Single node jobs**

For applications that are not optimized for HPC (high performance
computing) systems like simple python or R scripts and a lot of software
which is optimized for desktop PCs.

**Simple applications and scripts**

Many simple tools and scripts are not parallelized at all and therefore
won’t profit from more than one CPU core.

+-----------------+----------------------------------------------------+
| **Parameter**   | **Function**                                       |
+=================+====================================================+
| –nodes=1        | Start a unparallized job on only one node          |
+-----------------+----------------------------------------------------+
| –nt             | For OpenMP, only one task is necessary             |
| asks-per-node=1 |                                                    |
+-----------------+----------------------------------------------------+
| –               | Just one CPU core will be used.                    |
| cpus-per-task=1 |                                                    |
+-----------------+----------------------------------------------------+
| –mem=<MB>       | Memory (RAM) for the job. Number followed by unit  |
|                 | prefix, e.g. 16G                                   |
+-----------------+----------------------------------------------------+

If you are unsure if your application can benefit from more cores try a
higher number and observe the load of your job. If it stays at
approximately one there is no need to ask for more than one.

**OpenMP applications**

OpenMP (Open Multi-Processing) is a multiprocessing library is often
used for programs on shared memory systems. Shared memory describes
systems which share the memory between all processing units (CPU cores),
so that each process can access all data on that system.

+-----------------------+----------------------------------------------+
| **Parameter**         | **Function**                                 |
+=======================+==============================================+
| –nodes=1              | Start a parallel job for a shared memory     |
|                       | system on only one node                      |
+-----------------------+----------------------------------------------+
| –ntasks-per-node=1    | For OpenMP, only one task is necessary       |
+-----------------------+----------------------------------------------+
| –cpus-p               | Number of threads (CPU cores) to use         |
| er-task=<num_threads> |                                              |
+-----------------------+----------------------------------------------+
| –mem=<MB>             | Memory (RAM) for the job. Number followed by |
|                       | unit prefix, e.g. 16G                        |
+-----------------------+----------------------------------------------+

**Multiple node jobs (MPI)**

Depending on the frequency and bandwidth demand of your setup, you can
either just start a number of MPI tasks or request whole nodes. While
using whole nodes guarantees that a low latency and high bandwidth it
usually results in a longer queuing time compared to cluster wide job.
With the latter the SLURM manager can distribute your task across all
nodes of stallo and utilize otherwise unused cores on nodes which for
example run a 16 core job on a 20 core node. This usually results in
shorter queuing times but slower inter-process connection speeds.

We strongly advice all users to ask for a given set of cores when
submitting multi-core jobs. To make sure that you utilize full nodes,
you should ask for sets that adds up to both 16 and 20 (80, 160 etc) due
to the hardware specifics of Stallo i.e. submit the job
with --ntasks=80 **if** your application scales to this number of tasks.

This will make the best use of the resources and give the most
predictable execution times. If your job requires more than the default
available memory per core (32 GB/node gives 2 GB/core for 16 core nodes
and 1.6GB/core for 20 core nodes) you should adjust this need with the
following command: #SBATCH --mem-per-cpu=4GB When doing this, the batch
system will automatically allocate 8 cores or less per node.

**To use whole nodes**

+----------------+-----------------------------------------------------+
| **Parameter**  | **Function**                                        |
+================+=====================================================+
| –nod           | Start a parallel job for a distributed memory       |
| es=<num_nodes> | system on several nodes                             |
+----------------+-----------------------------------------------------+
| –ntasks-per-no | Number of (MPI) processes per node. Maximum number  |
| de=<num_procs> | depends nodes (16 or 20 on Stallo)                  |
+----------------+-----------------------------------------------------+
| –c             | Use one CPU core per task.                          |
| pus-per-task=1 |                                                     |
+----------------+-----------------------------------------------------+
| –exclusive     | Job will not share nodes with other running jobs.   |
|                | You don’t need to specify memory as you will get    |
|                | all available on the node.                          |
+----------------+-----------------------------------------------------+

**To distribute your job**

+-----------------+----------------------------------------------------+
| **Parameter**   | **Function**                                       |
+=================+====================================================+
| –nta            | Number of (MPI) processes in total. Equals to the  |
| sks=<num_procs> | number of cores                                    |
+-----------------+----------------------------------------------------+
| –m              | Memory (RAM) per requested CPU core. Number        |
| em-per-cpu=<MB> | followed by unit prefix, e.g. 2G                   |
+-----------------+----------------------------------------------------+

**Scalability**

You should run a few tests to see what is the best fit between
minimizing runtime and maximizing your allocated cpu-quota. That is you
should not ask for more cpus for a job than you really can utilize
efficiently. Try to run your job on 1, 2, 4, 8, 16, etc., cores to see
when the runtime for your job starts tailing off. When you start to see
less than 30% improvement in runtime when doubling the cpu-counts you
should probably not go any further. Recommendations to a few of the most
used applications can be found in `Application
guides <https://hpc-uit.readthedocs.io/en/latest/applications/sw_guides.html#sw-guides>`__.
