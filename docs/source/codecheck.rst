Job script examples
===================

Basic examples
---------------

**General blueprint for a jobscript**

You can save the following example to a file (e.g. run.sh) on Stallo.
Comment the two cp commands that are just for illustratory purpose
(lines 46 and 55) and change the SBATCH directives where applicable. You
can then run the script by typing:

$ sbatch run.sh

Please note that all values that you define with SBATCH directives are
hard values. When you, for example, ask for 6000 MB of memory
(--mem=6000MB) and your job uses more than that, the job will be
automatically killed by the manager.

.. codeblock

*#!/bin/bash -l*
*##############################*
*# Job blueprint #*
*##############################*
*# Give your job a name, so you can recognize it in the queue overview*
*#SBATCH --job-name=example*
*# Define, how many nodes you need. Here, we ask for 1 node.*
*# Each node has 16 or 20 CPU cores.*
*#SBATCH --nodes=1*
*# You can further define the number of tasks with --ntasks-per-\**
*# See "man sbatch" for details. e.g. --ntasks=4 will ask for 4 cpus.*
*# Define, how long the job will run in real time. This is a hard cap
meaning*
*# that if the job runs longer than what is written here, it will be*
*# force-stopped by the server. If you make the expected time too long,
it will*
*# take longer for the job to start. Here, we say the job will take 5
minutes.*
*# d-hh:mm:ss*
*#SBATCH --time=0-00:05:00*
*# Define the partition on which the job shall run. May be omitted.*
*#SBATCH --partition normal*
*# How much memory you need.*
*# --mem will define memory per node and*
*# --mem-per-cpu will define memory per CPU/core. Choose one of those.*
*#SBATCH --mem-per-cpu=1500MB*
*##SBATCH --mem=5GB # this one is not in effect, due to the double hash*
*# Turn on mail notification. There are many possible self-explaining
values:*
*# NONE, BEGIN, END, FAIL, ALL (including all aforementioned)*
*# For more values, check "man sbatch"*
*#SBATCH --mail-type=END,FAIL*
*# You may not place any commands before the last SBATCH directive*
*# Define and create a unique scratch directory for this job*
SCRATCH_DIRECTORY=/global/work/*${*\ USER\ *}*/*${*\ SLURM_JOBID\ *}*.stallo-adm.uit.no
mkdir -p *${*\ SCRATCH_DIRECTORY\ *}*
cd *${*\ SCRATCH_DIRECTORY\ *}*
*# You can copy everything you need to the scratch directory*
*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*
cp *${*\ SLURM_SUBMIT_DIR\ *}*/myfiles\*.txt
*${*\ SCRATCH_DIRECTORY\ *}*
*# This is where the actual work is done. In this case, the script only
waits.*

*# The time command is optional, but it may give you a hint on how long
the*

*# command worked*
time sleep 10
*#sleep 10*
*# After the job is done we copy our output back to $SLURM_SUBMIT_DIR*
cp *${*\ SCRATCH_DIRECTORY\ *}*/my_output *${*\ SLURM_SUBMIT_DIR\ *}*

*# In addition to the copied files, you will also find a file called*

*# slurm-1234.out in the submit directory. This file will contain all
output that*

*# was produced during runtime, i.e. stdout and stderr.*

*# After everything is saved to the home directory, delete the work
directory to*

*# save space on /global/work*

cd *${*\ SLURM_SUBMIT_DIR\ *}*

rm -rf *${*\ SCRATCH_DIRECTORY\ *}*

*# Finish the script*

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

SCRATCH_DIRECTORY=/global/work/*${*\ USER\ *}*/job-array-example/*${*\ SLURM_JOBID\ *}*

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

SCRATCH_DIRECTORY=/global/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

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

You can download the examples given here to a file (e.g. run.sh) and
start it with:

$ sbatch run.sh

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

SCRATCH_DIRECTORY=/global/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

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

*# Stallo has 16 or 20 cores/node and therefore we take*

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

SCRATCH_DIRECTORY=/global/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

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

SCRATCH_DIRECTORY=/global/work/*${*\ USER\ *}*/example/*${*\ SLURM_JOBID\ *}*

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
