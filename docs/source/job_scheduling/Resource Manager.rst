**Resource Manager**

Resource sharing on a high-performance cluster dedicated to scientific
computing is organized by a piece of software called a *resource
manager* or *job scheduler*. Indeed, users do not run software on those
cluster like they would do on a workstation, rather, they must submit
jobs, which are run unattended, by the job scheduler at the time, and on
the resources, decided by its algorithm.

`SLURM <https://slurm.schedmd.com/>`__ (Simple Linux utility Resource
manager) is the workload manager and job scheduler for FEDGEN HPC
Cluster. Slurm Workload Manager is an open source, fault-tolerant, and
highly scalable cluster management and job scheduling system for large
and small Linux clusters. It is used by many of the world’s
supercomputers and computer clusters.

The main function of SLURM is to allocate resources within the cluster
to jobs as requested by users. Resources managed by SLURM include:

- Nodes: A Node is a computing instance with one or more cores, memory
  and local storage.

- Cores: A complete isolated set of registers, Arithmetic Logic Units
  and queues to execute a program.

- Threads: Within each physical core, the operating system is able to
  address two virtual cores to increase the number of independent
  instruction sets processed.

- Memory: Program execution space

- Accelerators such as GPUs can be managed by SLURM

- License guide: SLURM also assists with license management by assigning
  available licenses to jobs at the time of scheduling. If the relevant
  license is not available, the job will not be executed and will remain
  in the pending state.Machine learning and deep learning models can be
  trained in HPC with Tensorflow, PyTorch, Dask or other distributed
  computing library.

Read the official `Quick Start User
Guide <https://slurm.schedmd.com/quickstart.html>`__ for an overview of
the architecture, commands and examples.

**Glossary of concepts and terms related to Slurm**

**Nodes**

   A node is (commonly) the largest part of the cluster running a single
   operating system image, and hence capable of supporting a shared
   memory program. Nodes are connected with each other through an
   interconnect (usually Infiniband or Ethernet), and communication
   between nodes is done via message passing.

   **CPU**

   The CPU is the Computer Processing Unit. Each node in a cluster can
   have one or multiple CPUs and each CPU has multiple cores capable of
   executing compute instructions.

   **Core**

   A core is the smallest processor of the CPU. It can execute a
   single *thread* of instructions.

   **Partition**

   Groups of nodes with limits and access controls, basically the
   equivalent of a queue in Torque. A node can be part of multiple
   partitions.

   **Job**

   A resource allocation request.

   **Job step**

   A set of (possibly parallel) tasks within a job. A job can consist of
   just a single job step or can contain multiple job steps which may
   use all or just a part of the resource allocation of a job and can
   run sequentially or in parallel (or a mix of that). The job script
   itself is a special job step, called the batch job step, but
   additional job steps can be created (e.g., for running a parallel MPI
   application).

   **Task**

   A task is executed within a job step and essentially corresponds to a
   Linux process: a single- or multithreaded process, or a single rank
   within a MPI process. Specifying the number of tasks one wants to run
   simultaneously and the number of cores per task is a very convenient
   way to request resources to Slurm as afterwards starting a MPI or
   hybrid MPI/OpenMP program using the srun command is very easy.
