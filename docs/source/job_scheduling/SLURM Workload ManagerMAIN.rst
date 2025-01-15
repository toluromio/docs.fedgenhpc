**SLURM Workload Manager**
----------------------------

**Slurm Commands**

Here’s a list of some commonly used user commands. See Slurm `man
pages <https://slurm.schedmd.com/man_index.html>`__ for a complete list
of commands or download
the `command summary PDF <https://usp-hpc.readthedocs.io/en/latest/_downloads/ed645709b8b700878a7e0385574b6c60/summary.pdf>`__.
Note that all Slurm commands start with **‘s’**.

+------------------------+---------------------------------------------+
| **Command**            | **Description**                             |
+========================+=============================================+
| sbatch <slurm_script>  | Submit a job script for later execution.    |
+------------------------+---------------------------------------------+
| scancel <jobid>        | Cancel a pending or running job or job step |
+------------------------+---------------------------------------------+
| srun                   | Parallel job launcher (Slurm analog of      |
|                        | mpirun)                                     |
+------------------------+---------------------------------------------+
| squeue                 | Show all jobs in the queue                  |
+------------------------+---------------------------------------------+
| squeue -u <username>   | Show jobs in the queue for a specific user  |
+------------------------+---------------------------------------------+
| squeue –start          | Report the expected start time for pending  |
|                        | jobs                                        |
+------------------------+---------------------------------------------+
| squeue -j <jobid>      | Show the nodes allocated to a running job   |
+------------------------+---------------------------------------------+
| scontrol show config   | View default parameter settings             |
+------------------------+---------------------------------------------+
| sinfo                  | Show cluster status                         |
+------------------------+---------------------------------------------+

**Gathering cluster information**

Slurm offers the sinfo command to get an overview of the resources
offered by the cluster. By default, sinfo lists the partitions that are
available. A **partition** is a set of **compute nodes** (computers
dedicated to workload computation) grouped logically based on either
physical properties of the hardware or job scheduling policies. Typical
examples include partitions dedicated to debugging where only small and
short jobs can be scheduled, or partitions dedicated to visualization
with nodes equipped with specific graphic cards.

.. code-block:: python
    # sinfo

    PARTITION AVAIL TIMELIMIT NODES STATE NODELIST
    batch up infinite 2 alloc giga[08-09]
    batch up infinite 6 idle node[10-16]
    debug\* up 30:00 8 idle node[01-07]


In the above example, we see two partitions, named *batch* and *debug*.
The latter is the default partition as it is marked with an asterisk.
All nodes of the debug partition are idle, while two of the batch
partition are being used. The nodes in this example are
named giga001 to giga016.

The sinfo command also lists the time limit (column TIMELIMIT) to which
jobs are subject. On every cluster, jobs are limited to a maximum run
time, to allow job rotation and let every user a chance to see their job
being started. Generally, the larger the cluster, the smaller the
maximum allowed time.

The command sinfo can output the information in a node-oriented fashion,
with the argument -N.

.. code-block:: ruby

    # sinfo -N -l
    NODELIST NODES PARTITION STATE CPUS S:C:T MEMORY TMP_DISK WEIGHT
    AVAIL_FE REASON
    node[01-02] 2 debug\* idle 32 2:8:2 3448 38536 16 Intel (null)
    node[03,05-07] 4 debug\* idle 32 2:8:2 3384 38536 16 Intel (null)
    node03 1 debug\* down 32 2:8:2 3394 38536 16 Intel "Disk replacement"
    node[08-09] 2 batch allocated 32 2:8:2 246 82306 16 AMD (null)
    node[10-16] 7 batch idle 32 2:8:2 246 82306 16 AMD (null)


With the -l argument, more information about the nodes is provided,
among which the number of “CPUs” (CPUS), which is the number of
processing units that the jobs can use. It should generally correspond
to the number of sockets (S) times number of cores per socket (C) times
number of hardware threads per core (T in the S:C:T column) but can be
lower in the case some CPUs are reserved for system use.

The other columns report the volatile working memory (RAM – MEMORY), the
size of the local temporary disk (also called *local scratch
space* – TMP_DISK), and the node “weight” (an internal parameter
specifying preferences in nodes for allocations when there are multiple
possibilities). The last but one column (AVAIL_FE) show
so-called **features** of the nodes, that are set by the administrator,
and can refer to a processor vendor or family, a specific network
equipment, or any desirable feature of the node, that can be used to
choose one node type to another.

The last column, (REASON), if not null, describes the reason why a node
would not be available.

**Note**

You can actually specify precisely what information you would
like sinfo to output by using its --format argument. For more details,
have a look at the command manpage with man sinfo.

**Gathering job information**

The squeue command shows the list of jobs which are currently running
(they are in the *RUNNING* **state**, noted as ‘R’) or waiting for
resources (noted as ‘PD’, short for *PENDING*).

# squeue

JOBID PARTITION NAME USER ST TIME NODES NODELIST(REASON)

12345 debug job1 dave R 0:21 4 node[09-12]

12346 debug job2 dave PD 0:00 8 (Resources)

12348 debug job3 ed PD 0:00 4 (Priority)

The above output shows that one job is running, whose name is *job1* and
whose **jobid** is 12345. The jobid is a unique identifier that is used
by many Slurm commands when actions must be taken about one particular
job. For instance, to cancel job *job1*, you would use scancel 12345.
Time is the time the job has been running until now. Node is the number
of nodes which are allocated to the job, while the Nodelist column lists
the nodes which have been allocated for running jobs. For pending jobs,
that column gives the reason why the job is pending. In the example, job
12346 is pending because requested resources (CPUs, or other) are not
available in sufficient amounts, while job 12348 is waiting for job
12346, whose priority is higher, to run.

SLURM Parameter

`SLURM <https://slurm.schedmd.com/>`__ supports a multitude of different
parameters. This enables you to effectively tailor your script to your
need when using FEDGEN HPC .

The following parameters can be used as command line parameters
with sbatch and srun or in job script, see `job script
examples <https://scihpc.ir/docs/jobs/examples/>`__. To use these
parameters in a job script, start a newline with #SBTACH directive
followed by the parameter. Replace <....> with the value you want,
e.g. --job-name=test-job. The following tables shows the commonly used
ones.

**Basic Parameters**

+----------------------+-----------------------------------------------+
| **Parameter**        | **Function**                                  |
+======================+===============================================+
| --j                  | Job name to be displayed by for example       |
| ob-name=<name> or -J | the squeue command                            |
| <name>               |                                               |
+----------------------+-----------------------------------------------+
| -                    | Path to the file where the job output is      |
| -output=<path> or -o | written to                                    |
| <name>               |                                               |
+----------------------+-----------------------------------------------+
| --error=<path> or -e | Path to the file where the job error is       |
| <name>               | written to                                    |
+----------------------+-----------------------------------------------+
| --mail-type=<type>   | Turn on mail notification; type can be one of |
|                      | BEGIN, END, FAIL, REQUEUE or ALL              |
+----------------------+-----------------------------------------------+
| --mail-              | Email address to send notifications to        |
| user=<email_address> |                                               |
+----------------------+-----------------------------------------------+

**Requesting Resources parameters**

+----------------------+-----------------------------------------------+
| **Parameter**        | **Function**                                  |
+======================+===============================================+
| --time=<d-hh:mm:ss>  | Time limit for job. Job will be killed by     |
|                      | SLURM after time has run out. Format          |
|                      | days-hours:minutes:seconds                    |
+----------------------+-----------------------------------------------+
| --nod                | Number of nodes. Multiple nodes are only      |
| es=<num_nodes> or -N | useful for jobs with distributed-memory (e.g. |
|                      | MPI).                                         |
+----------------------+-----------------------------------------------+
| --mem=<MB>           | Memory (RAM) per node. Number followed by     |
|                      | unit prefix K|M|G|T, e.g. 16G                 |
+----------------------+-----------------------------------------------+
| --mem-per-cpu=<MB>   | Memory (RAM) per requested CPU core. This     |
|                      | option with the value of 512 M is set as the  |
|                      | default for all partitions.                   |
+----------------------+-----------------------------------------------+
| --ntas               | Number of processes. Useful for MPI jobs.     |
| ks=<num_procs> or -n |                                               |
+----------------------+-----------------------------------------------+
| --ntasks-            | Number of processes per node. Useful for MPI  |
| per-node=<num_procs> | jobs. Maximum number is node dependent        |
|                      | (number of cores)                             |
+----------------------+-----------------------------------------------+
| --cpus-per-task      | CPU cores per task. For OpenMP (i.e. shared   |
| =<num_threads> or -c | memory) or hybrid OpenMP/MPI use one. Should  |
|                      | be equal to the number of threads.            |
+----------------------+-----------------------------------------------+
| --exclusive          | Job will not share nodes with other running   |
|                      | jobs. You will be charged for the complete    |
|                      | nodes even if you asked for less.             |
+----------------------+-----------------------------------------------+

Accounting parameters

See
also `partitions <https://scihpc.ir/docs/jobs/slurm/#partitions-queues>`__.

+-----------------------+----------------------------------------------+
| **Parameter**         | **Function**                                 |
+=======================+==============================================+
| --account=<name>      | Project (not user) account the job should be |
|                       | charged to.                                  |
+-----------------------+----------------------------------------------+
| --p                   | Partition/queue in which o run the job.      |
| artition=<name> or -p |                                              |
+-----------------------+----------------------------------------------+
| --qos=<...>           | The quality of service requested; can        |
|                       | be *low*, *normal* or *high*                 |
+-----------------------+----------------------------------------------+

Advanced Job Control parameters

+--------------------+-------------------------------------------------+
| **Parameter**      | **Function**                                    |
+====================+=================================================+
| --array=<indexes>  | Submit a collection of similar jobs,            |
|                    | e.g. --array=1-10. (sbatch command only). See   |
|                    | official `SLURM                                 |
|                    | documentation                                   |
|                    |  <https://slurm.schedmd.com/job_array.html>`__. |
+--------------------+-------------------------------------------------+
| --depend           | Wait with the start of the job until specified  |
| ency=<state:jobid> | dependencies have been satisfied.               |
|                    | E.g. --dependency=afterok:123456                |
+--------------------+-------------------------------------------------+
