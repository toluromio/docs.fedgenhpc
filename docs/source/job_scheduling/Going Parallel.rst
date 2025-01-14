**Going parallel**
--------------------

There are several ways a **parallel job**, one that leverages multiple
compute units (CPUs) at the same time, can be created.

**Warning**

The type of job that can be created depends on the capabilities of the
software being used. Parallel computing requires specific programming
techniques, which, if not used, lead to simply the same computation
being performed multiple times, with no actual benefit.

If the resource request fit with the capabilities of the program, then
Slurm will be able to allocate every parallel sequence of computing
instructions, called a **thread** in programming, to a different CPU in
a precise one-to-one mapping. This process is called “binding”, or
“pinning”. If there are more threads created by the program than there
are CPUs available, the operating system will iteratively start and stop
threads to allocate CPU time to every thread, in a procedure called
“context switching”. That procedure consumes resource in itself and
incurs significant overhead to be considered undesirable for
high-performance computing.

**Single-node parallelism**

In the Linux operating system, running a program will initiate
a **process**, that is a running copy of the program in the main memory.
A process initially consists of one single thread, but it can clone
itself into multiple threads that all share the same memory space
(*shared-memory programming*) and can perform computations in parallel,
but it can also spawn (“fork” in Linux terms) other processes, whose
memory spaces are independent and within which it can communicate
internally.

Multithreaded programms can be written using
the `pthreads <https://en.wikipedia.org/wiki/Pthreads>`__ system call,
but most multithreaded scientific software is written
with `OpenMP <https://en.wikipedia.org/wiki/OpenMP>`__. OpenMP allows
creating *Single program, Multiple data* programs, or SPMD. The same
program is run multiple times, but each instance is identified by
a *thread ID*, or *rank* that can be used to differentiate the behavior
of each instance, for instance working on a separate subset of the data.
Optimised linear algebrae of signal processing libraries use OpenMP a
lot for parallelism.

**Note**

Even if the software you write is purely sequential and does not use
OpenMP for instance, if it links to optimized scientific libraries (as
is the case for interpreted languages like Python, R or Julia, they will
use OpenMP behind the scenes so you should request at least 4 or 8 CPUs.

**Multi-node parallelism**

**Multi-node parallelism assumes distributed memory programming with
either**

- no communication between the processes on the different nodes ; or

- *message passing* through the network ; or

- communication through a common file-system.

The first case, called **embarrassingly parallel**, simply relies on
multiple instances of the same program being started with either - a
different *environment*, and possibly - different arguments.

The second case is typical of high-performance computing, and requires
specific large-bandwith low-latency, network hardware. Most message
passing scientific software
use `MPI <https://en.wikipedia.org/wiki/Message_Passing_Interface>`__, a
library that takes care of instantiating multiple instances of the same
program on different nodes, and allow them to send and receive messages
through the network. This is another example of SPMD programming.

The last case is mostly encountered with Master/Worker setups (a
specific case of *multiple program multiple data*) where one program
(master) is designed to prepare and distribute work, while another
(worker) is designed to perform the work. Only one master is run, but
there can be as many workers instances as needed.

**Important**

The above taxonomy must be take with a grain of salt. Some Master/Worker
programs use the network rather than the disk for communication, or
use inter-process *communication*. In that later case, they can only do
single-node parallelism. As for Message Passing, it can be done all in a
single node. Also shared memory programming can be done on multiple
nodes if specific libraries are used.

**Running MPI jobs**

There are two available MPI implementations on FEDGEN HPC:

- OpenMPI provided by the foss module, e.g. module load foss/2019b

- Intel MPI provided by the intel module, e.g. module load intel/2019b

There are several ways of launching an MPI application within a SLURM
allocation, e.g. srun, mpirun, mpiexec and mpiexec.hydra. Unfortunately,
the best way to launch your program depends on the MPI implementation
(and possibly your application), and choosing the wrong command can
severly affect the efficiency of your parallel run. Our recommendation
is the following:

**Intel MPI**

With Intel MPI, We recommend using srun:

$ srun --mpi=pmi2 ./my_application

**OpenMPI**

With OpenMPI, we recommend using mpirun:

$ mpirun ./my_application
