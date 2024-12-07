User Guide
==========

How to submit a job on DANTE
----------------------------

DANTE use Slurm Scheduler. You can find Slurm documentation on
their `website <https://slurm.schedmd.com/documentation.html>`__.

**Recommendations**

'''For a correct usage of cluster''' thank you to respect our
recommendations.

- for a long (> 24h00) parallel job: not more than 80 cores and 60Gb of
  memory.

- no limit for short (<5h00) parallel jobs. Nevertheless, for the jobs
  to 8h00-12h00, it is preferable to execute these jobs at the end of
  the day to run overnight.

- Each user of the queue should not take more than 40% of the available
  resources in this queue.

These rules ensure that a maximum of users can work on the cluster,
however they can be flexible due to the system load (memory, cpu, etc.)
on the cluster.

If you have an exceptional request please contact us
via `OSticket <https://supportapc.in2p3.fr/>`__.

**Job submission**

You can save the following example to a file (e.g. run.sh). Comment the
two cp commands that are just for illustratory purpose (lines 46 and 55)
and change the SBATCH directives where applicable. You can then run the
script by typing:

$ sbatch run.sh

Please note that all values that you define with SBATCH directives are
hard values. When you, for example, ask for 6000 MB of memory
(--mem=6000MB) and your job uses more than that, the job will be
automatically killed by the manager.

1. #!/bin/bash

2.  

3. ##############################

4. # Job blueprint #

5. ##############################

6.  

7. # Give your job a name, so you can recognize it in the queue overview

8. #SBATCH --job-name=example

9.  

10. # Define, how many nodes you need. Here, we ask for 1 node.

11. # Each node has 16 or 20 CPU cores.

12. #SBATCH --nodes=1

13. # You can further define the number of tasks with --ntasks-per-\*

14. # See "man sbatch" for details. e.g. --ntasks=4 will ask for 4 cpus.

15.  

16. # Define, how long the job will run in real time. This is a hard cap
meaning

17. # that if the job runs longer than what is written here, it will be

18. # force-stopped by the server. If you make the expected time too
long, it will

19. # take longer for the job to start. Here, we say the job will take 5
minutes.

20. # d-hh:mm:ss

21. #SBATCH --time=0-00:05:00

22.  

23. # Define the partition on which the job shall run. May be omitted.

24. #SBATCH --partition quiet

25.  

26. # How much memory you need.

27. # --mem will define memory per node and

28. # --mem-per-cpu will define memory per CPU/core. Choose one of
those.

29. #SBATCH --mem-per-cpu=1500MB

30. ##SBATCH --mem=5GB # this one is not in effect, due to the double
hash

31.  

32. # Turn on mail notification. There are many possible self-explaining
values:

33. # NONE, BEGIN, END, FAIL, ALL (including all aforementioned)

34. # For more values, check "man sbatch"

35. #SBATCH --mail-user=user@apc.in2p3.fr

36. #SBATCH --mail-type=END,FAIL

37.  

38. # The output file for your job.

39. #SBATCH --output=test-srun.out

40.  

41. ##SBATCH --requeue

42. # Specifies that the job will be requeued after a node failure.

43. # The default is that the job will not be requeued.

44.  

45. # You may not place any commands before the last SBATCH directive

46.  

47. # Define and create a unique scratch directory for this job

48. SCRATCH_DIRECTORY=/scratch/${USER}/${SLURM_JOBID}

49. mkdir -p ${SCRATCH_DIRECTORY}

50. cd ${SCRATCH_DIRECTORY}

51.  

52. # You can copy everything you need to the scratch directory

53. # ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from

54. cp ${SLURM_SUBMIT_DIR}/myfiles\*.txt ${SCRATCH_DIRECTORY}

55.  

56. # This is where the actual work is done. In this case, the script
only waits.

57. # The time command is optional, but it may give you a hint on how
long the

58. # command worked

59. time sleep 10

60. #sleep 10

61.  

62. # After the job is done we copy our output back to $SLURM_SUBMIT_DIR

63. cp ${SCRATCH_DIRECTORY}/my_output ${SLURM_SUBMIT_DIR}

64.  

65. # In addition to the copied files, you will also find a file called

66. # slurm-1234.out in the submit directory. This file will contain all
output that

67. # was produced during runtime, i.e. stdout and stderr.

68.  

69. # After everything is saved to the home directory, delete the work
directory to

70. # save space on /scratch

71. cd ${SLURM_SUBMIT_DIR}

72. rm -rf ${SCRATCH_DIRECTORY}

73.  

74. # Finish the script

75. exit 0

76.  

**Running many sequential jobs in parallel using job arrays**

In this example we wish to run many similar sequential jobs in parallel
using job arrays. We take Python as an example but this does not matter
for the job arrays:

.. code-block:: language

  #!/usr/bin/env python
  import time
  print('start at ' + time.strftime('%H:%M:%S'))
  print('sleep for 10 seconds ...')
  time.sleep(10)
  print('stop at ' + time.strftime('%H:%M:%S'))

Save this to a file called “test.py” and try it out:

$ python test.py

start at 15:23:48

sleep **for** 10 seconds ...

stop at 15:23:58

Good. Now we would like to run this script 16 times at the same time.
For this we use the following script:

**#!/bin/bash**

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

SCRATCH_DIRECTORY=/scratch/${USER}/job-array-example/${SLURM_JOBID}

mkdir -p ${SCRATCH_DIRECTORY}

cd ${SCRATCH_DIRECTORY}

cp ${SLURM_SUBMIT_DIR}/test.py ${SCRATCH_DIRECTORY}

*# each job will see a different ${SLURM_ARRAY_TASK_ID}*

echo "now processing task id:: " ${SLURM_ARRAY_TASK_ID}

python test.py > output\_${SLURM_ARRAY_TASK_ID}.txt

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp output\_${SLURM_ARRAY_TASK_ID}.txt ${SLURM_SUBMIT_DIR}

*# we step out of the scratch directory and remove it*

cd ${SLURM_SUBMIT_DIR}

rm -rf ${SCRATCH_DIRECTORY}

*# happy end*

exit 0

Submit the script and after a short while you should see 16 output files
in your submit directory:

$ ls -l output\*.txt

-rw\ *------- 1 user user 60 Oct 14 14:44 output_1.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_10.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_11.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_12.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_13.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_14.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_15.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_16.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_2.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_3.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_4.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_5.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_6.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_7.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_8.txt*

-rw\ *------- 1 user user 60 Oct 14 14:44 output_9.txt*

**Interactive submission**

One has the possibility to use the interactive job submission via
the srun command, which allows to:

- ease the debugging

- perform interactive computations on '''declared''' and '''reserved'''
  resources

Example :

srun --nodes=1 --ntasks-per-node=1 --time=01:00:00 --pty bash -i

**Job informations**

List all current jobs for a user:

squeue -u <username>

List all running jobs for a user:

squeue -u <username> -t RUNNING

List all pending jobs for a user:

squeue -u <username> -t PENDING

List all current jobs in the shared partition for a user:

squeue -u <username> -p shared

List detailed information for a job (useful for troubleshooting):

scontrol show jobid -dd <jobid>

List status info for a currently running job:

sstat --format=AveCPU,AvePages,AveRSS,AveVMSize,JobID -j <jobid>
--allsteps

Once your job has completed, you can get additional information that was
not available during the run. This includes run time, memory used, etc.
To get statistics on completed jobs by jobID:

sacct -j <jobid> --format=JobID,JobName,MaxRSS,Elapsed

To view the same information for all jobs of a user:

sacct -u <username> --format=JobID,JobName,MaxRSS,Elapsed

**OpenMP and MPI**

You can download the examples given here to a file (e.g. run.sh) and
start it with:

$ sbatch run.sh

**Example for an OpenMP job**

**#!/bin/bash**

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

SCRATCH_DIRECTORY=/scratch/${USER}/example/${SLURM_JOBID}

mkdir -p ${SCRATCH_DIRECTORY}

cd ${SCRATCH_DIRECTORY}

*# we copy everything we need to the scratch directory*

*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*

cp ${SLURM_SUBMIT_DIR}/my_binary.x ${SCRATCH_DIRECTORY}

*# we set OMP_NUM_THREADS to the number of available cores*

export OMP_NUM_THREADS=${SLURM_CPUS_PER_TASK}

*# we execute the job and time it*

time ./my_binary.x > my_output

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp ${SCRATCH_DIRECTORY}/my_output ${SLURM_SUBMIT_DIR}

*# we step out of the scratch directory and remove it*

cd ${SLURM_SUBMIT_DIR}

rm -rf ${SCRATCH_DIRECTORY}

*# happy end*

exit 0

**Example for a MPI job**

**#!/bin/bash**

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

SCRATCH_DIRECTORY=/scratch/${USER}/example/${SLURM_JOBID}

mkdir -p ${SCRATCH_DIRECTORY}

cd ${SCRATCH_DIRECTORY}

*# we copy everything we need to the scratch directory*

*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*

cp ${SLURM_SUBMIT_DIR}/my_binary.x ${SCRATCH_DIRECTORY}

*# we execute the job and time it*

time mpirun -np $SLURM_NTASKS ./my_binary.x > my_output

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp ${SCRATCH_DIRECTORY}/my_output ${SLURM_SUBMIT_DIR}

*# we step out of the scratch directory and remove it*

cd ${SLURM_SUBMIT_DIR}

rm -rf ${SCRATCH_DIRECTORY}

*# happy end*

exit 0

**Example for a hybrid MPI/OpenMP job**

**#!/bin/bash**

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

SCRATCH_DIRECTORY=/scratch/${USER}/example/${SLURM_JOBID}

mkdir -p ${SCRATCH_DIRECTORY}

cd ${SCRATCH_DIRECTORY}

*# we copy everything we need to the scratch directory*

*# ${SLURM_SUBMIT_DIR} points to the path where this script was
submitted from*

cp ${SLURM_SUBMIT_DIR}/my_binary.x ${SCRATCH_DIRECTORY}

*# we set OMP_NUM_THREADS to the number cpu cores per MPI task*

export OMP_NUM_THREADS=${SLURM_CPUS_PER_TASK}

*# we execute the job and time it*

time mpirun -np $SLURM_NTASKS ./my_binary.x > my_output

*# after the job is done we copy our output back to $SLURM_SUBMIT_DIR*

cp ${SCRATCH_DIRECTORY}/my_output ${SLURM_SUBMIT_DIR}

*# we step out of the scratch directory and remove it*

cd ${SLURM_SUBMIT_DIR}

rm -rf ${SCRATCH_DIRECTORY}

*# happy end*

exit 0

If you want to start more than one MPI rank per node you can
use --ntasks-per-node in combination with --nodes:

*#SBATCH --nodes=4 --ntasks-per-node=2 --cpus-per-task=8*

This will start 2 MPI tasks each on 4 nodes, where each task can use up
to 8 threads.

**Job status**

When you use the squeue command, you will see that the queue is divided
in 3 parts: Active jobs, Idle jobs, and Blocked jobs.

Active jobs are the jobs that are running at the moment. Idle jobs are
next in line to start running, when the needed resources become
available. Each user can by default have only one job in the Idle Jobs
queue.

Blocked jobs are all other jobs. Their state can be *Idle*, *Hold*,
or *Deferred*. *Idle* means that they are waiting to get to the Idle
queue. They will eventually start when the resources become available.
The jobs with the *Hold* state have been put on hold either by the
system, or by the user. F.e. if you have one job in the Idle queue, that
is not very important to you, and it is blocking other, more urgent,
jobs from starting, you might want to put that one job on hold. Jobs on
hold will not start until the hold is released. *Deferred* jobs will not
start. In most cases, the job is deferred because it is asking for a
combination of resources that we can not provide.

Please contact the support staff, if you don’t understand why your job
has a hold or deferred state.

To monitor your jobs, you can use of of those commands. For details run
them with the --help option:

scontrol show jobid -dd <jobid> lists detailed information for a job
(useful for troubleshooting).

sacct -j <jobid> --format=JobID,JobName,MaxRSS,Elapsed will give you
statistics on completed jobs by jobID. Once your job has completed, you
can get additional information that was not available during the run.
This includes run time, memory used, etc.

**Summary of used resources**

Slurm will append a summary of used resources to the slurm-xxx.out file.
The fields are:

- Task and CPU usage stats

- AllocCPUS: Number of allocated CPUs

- NTasks: Total number of tasks in a job or step.

- MinCPU: Minimum CPU time of all tasks in job (system + user).

- MinCPUTask: The task ID where the mincpu occurred.

- AveCPU: Average CPU time of all tasks in job (system + user)

- Elapsed: The jobs elapsed time in format [DD-[HH:]]MM:SS.

- ExitCode: The exit code returned by the job script. Following the
  colon is the signal that caused the process to terminate if it was
  terminated by a signal.

- Memory usage stats

- MaxRSS: Maximum resident set size of all tasks in job.

- MaxRSSTask: The task ID where the maxrss occurred.

- AveRSS: Average resident set size of all tasks in job.

- MaxPages: Maximum number of page faults of all tasks in job.

- MaxPagesTask: The task ID where the maxpages occurred.

- AvePages: Average number of page faults of all tasks in job.

- Disk usage stats

- MaxDiskRead: Maximum number of bytes read by all tasks in job.

- MaxDiskReadTask: The task ID where the maxdiskread occurred.

- AveDiskRead: Average number of bytes read by all tasks in job.

- MaxDiskWrite: Maximum number of bytes written by all tasks in job.

- MaxDiskWriteTask: The task ID where the maxdiskwrite occurred.

- AveDiskWrite: Average number of bytes written by all tasks in job.

**Cluster informations**

Here are different commands that display information about the status of
the cluster :

- Summary usage of the cluster : sinfo, sinfo -Nel

- Available resources : sinfo -o %P,%D,%c,%m,%e,%C

- Free nodes on a partition : sinfo -t idle -p quiet -h -o %N

More informations on the `official
documentation <https://slurm.schedmd.com/sinfo.html>`__.

**TORQUE TO SLUM**

**Commands**

+------------------+--------------------+------------------------------+
|                  | **Torque**         | **Slurm**                    |
+==================+====================+==============================+
| Job submission   | qsub [script_file] | sbatch [script_file]         |
+------------------+--------------------+------------------------------+
| Job deletion     | qdel [job_id]      | scancel [job_id]             |
+------------------+--------------------+------------------------------+
| Job status (by   | qstat [job_id]     | squeue [job_id]              |
| job)             |                    |                              |
+------------------+--------------------+------------------------------+
| Job status (by   | qstat -u           | squeue -u [user_name]        |
| user)            | [user_name]        |                              |
+------------------+--------------------+------------------------------+
| Job hold         | qhold [job_id]     | scontrol hold [job_id]       |
+------------------+--------------------+------------------------------+
| Job release      | qrls [job_id]      | scontrol release [job_id]    |
+------------------+--------------------+------------------------------+
| Queue list       | qstat -Q           | sqeue                        |
+------------------+--------------------+------------------------------+
| Node list        | pbsnodes -l        | sinfo -N OR scontrol show    |
|                  |                    | nodes                        |
+------------------+--------------------+------------------------------+
| Cluster status   | qstat -a           | sinfo                        |
+------------------+--------------------+------------------------------+

**Script directive**

+----------------------+------------------------+---------------------+
|                      | **Torque**             | **Slurm**           |
+======================+========================+=====================+
| Script directive     | #PBS                   | #SBATCH             |
+----------------------+------------------------+---------------------+
| Queue                | -q [queue]             | -p [queue]          |
+----------------------+------------------------+---------------------+
| Node Count           | -l nodes=[count]       | -N [min[-max]]      |
+----------------------+------------------------+---------------------+

` Previous <https://si-apc.pages.in2p3.fr/dante-cluster/technical/>`__\ `Next  <https://si-apc.pages.in2p3.fr/dante-cluster/account/>`__
