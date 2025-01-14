**Managing & Monitoring jobs**
---------------------------------

The lifecycle of a job can be managed with as little as three different
commands:

1. Submit the job with sbatch <script_name>.

2. Check the job status with squeue. (to limit the display to only your
   jobs use squeue -u <user_name>.)

3. (optional) Delete the job with scancel <job_id>.

You can also hold the start of a job:

**scontrol hold <job_id>**

   Put a hold on the job. A job on hold will not start or block other
   jobs from starting until you release the hold.

   **scontrol release <job_id>**

   Release the hold on a job.

**Job status descriptions in squeue**

When you run squeue (probably limiting the output
with squeue -u <user_name>), you will get a list of all jobs currently
running or waiting to start. Most of the columns should be
self-explaining, but the *ST* and *NODELIST (REASON)* columns can be
confusing.

*ST* stands for *state*. The most important states are listed below. For
a more comprehensive list, check the `squeue help page section Job State
Codes <https://slurm.schedmd.com/squeue.html#lbAG>`__.

**R**

   The job is running

   **PD**

   The job is pending (i.e. waiting to run)

   **CG**

   The job is completing, meaning that it will be finished soon

The column *NODELIST (REASON)* will show you a list of computing nodes
the job is running on if the job is actually running. If the job is
pending, the column will give you a reason why it still pending. The
most important reasons are listed below. For a more comprehensive list,
check the `squeue help page section Job Reason
Codes <https://slurm.schedmd.com/squeue.html#lbAF>`__.

**Priority**

   There is another pending job with higher priority

   **Resources**

   The job has the highest priority, but is waiting for some running job
   to finish.

   **QOS*Limit**

   This should only happen if you run your job with --qos=devel. In
   developer mode you may only have one single job in the queue.

   **launch failed requeued held**

   Job launch failed for some reason. This is normally due to a faulty
   node. Please contact us
   via `support@metacenter.no <mailto:support%40metacenter.no>`__ stating
   the problem, your user name, and the jobid(s).

   **Dependency**

   Job cannot start before some other job is finished. This should only
   happen if you started the job with --dependency=...

   **DependencyNeverSatisfied**

   Same as *Dependency*, but that other job failed. You must cancel the
   job with scancel JOBID.

**Monitoring your jobs**

**SLURM commands**

To monitor your jobs, you can use of of those commands. For details run
them with the *-*-help option:

scontrol show jobid -dd <jobid> lists detailed information for a job
(useful for troubleshooting).

sacct -j <jobid> --format=JobID,JobName,MaxRSS,Elapsed will give you
statistics on completed jobs by jobID. Once your job has completed, you
can get additional information that was not available during the run.
This includes run time, memory used, etc.

From our monitoring tool Ganglia, you can watch live status information
on FEDGEN HPC:

- `Load situation <http://stallo-adm.uit.no/ganglia/>`__

- `Job
  queue <http://stallo-login2.uit.no/slurmbrowser/html/squeue.html>`__

**CPU load and memory consumption of your job**

FEDGEN HPC has only limited resources and usually a high demand.
Therefore using the available resources as efficient as possible is
paramount to have short queueing times and getting most out of your
quota.

**Accessing slurmbrowser**

In order to find out the CPU load and memory consumption of your running
jobs or jobs which have finished less than 48 hours ago, please use
the `job
browser <http://stallo-login2.uit.no/slurmbrowser/html/squeue.html>`__ (accessible
from within the UNINETT network).

For users from outside of UNINETT, you have to setup a ssh tunnel to
FEDGEN HPC with:

ssh -L8080:localhost:80 FEDGEN HPC-login2.uit.no

After that you can access slurmbrowser
at http://localhost:8080/slurmbrowser/html/squeue.html

**Detecting inefficient jobs**

You can filter for a slurm job ID, account name or user name with the
search bar in the upper left corner.

For single- or multinode jobs the AvgNodeLoad is an important indicator
if your jobs runs efficiently, at least with respect to CPU usage. If
you use the whole node, the average node load should be close to number
of CPU cores of that node (so 16 or 20 on FEDGEN HPC). In some cases it
is totally acceptable to have a low load if you for instance need a lot
of memory but in general either CPU or memory load should be high.
Otherwise you are wasting your quota and experience probably longer than
necessary queuing times.

If you detect inefficient jobs you either look for ways to improve the
resource usage of your job or ask for less resources in your SLURM
script.

**Understanding your job status**

When you look at the job queue through the `job
browser <http://stallo-login2.uit.no/slurmbrowser/html/squeue.html>`__,
or you use the squeue command, you will see that the queue is divided in
3 parts: Active jobs, Idle jobs, and Blocked jobs.

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
combination of resources that FEDGEN HPC can not provide.

Please contact the support staff, if you don’t understand why your job
has a hold or deferred state.

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

Monitoring and managing your
job(s)\ `# <https://docs.hpc.ugent.be/running_batch_jobs/#monitoring-and-managing-your-jobs>`__

Using the job ID that qsub returned, there are various ways to monitor
the status of your job. In the following commands, replace 12345 with
the job ID qsub returned.

qstat 12345

To show on which compute nodes your job is running, at least, when it is
running:

qstat -n 12345

To remove a job from the queue so that it will not run, or to stop a job
that is already running.

qdel 12345

When you have submitted several jobs (or you just forgot about the job
ID), you can retrieve the status of all your jobs that are submitted and
are not yet finished using:

$ qstat

:

Job ID Name User Time Use S Queue

----------- ------- --------- -------- - -----

123456 .... mpi vsc40000 0 Q short
