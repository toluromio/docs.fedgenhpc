FEDGEN HPC Layout
--------------------

The FEDGEN HPC comprises of three different kinds of nodes: the login
node, the compute/GPU nodes, and the storage nodes. In a typical
workflow, users primarily access FEDGEN HPC by connecting to the login
node, where they compile and test code before submitting jobs to
the `SLURM
queue <https://fedgenhpc.readthedocs.io/en/latest/SLURM_overview/>`__
which automatically assigns a compute node(s) to the submitted job. This
HPC layout is in the diagram below.

|FEDGEN HPC Flow|

Login Node

The Login node is the only node directly accessible over the internet.
Users connect to the login from their personal computers
using ssh before accessing other parts of the FEDGEN HPC Cluster.

Compute Nodes

Compute nodes perform computations submitted to the cluster

Storage Nodes

Storage nodes shared filesystem to the entire cluster

A cluster typically hosts multiple types of nodes:

- *management* nodes: these computers run the management services like
  Slurm, but also the user identities database, the monitoring and
  reporting applications, etc. ;

- *login* nodes: these are the computers users login into through SSH to
  install software and submit jobs ;

- *storage* nodes: these computers host user files in possibly multiple
  filesystems ; and

- *compute* nodes: these are the computers where jobs run and where the
  computations actually take place. They are often organised
  into **partitions**

Alongside these most common types, a cluster can also comprise

- *visualization* nodes: computers dedicated to data visualization with
  specific hardware ;

- *I/O* nodes: computers dedicated to data ingress and outgress to and
  from the cluster.

The typical usage therefore involves connecting to a login node, making
sure data are stored on the right filesystems and software are
available, and then submitting jobs. The jobs are dispatched to the
compute nodes by the resource manager based on the adequation between
the resources resquested by the job and those offered by the compute
nodes.

**Warning**

On a cluster, you do not run the computations on the login node.
Computations belong on the compute nodes, when, and where decided by the
scheduler.

Inside a compute node, the computation is carried on by
the **processors**. A processor looks like this:

|image1|

A typical compute node has one, two, or four **sockets** on the
motherboard, that each can host a processor. The processors are made of
multiple **cores**, that can be though of as independent computing
units, each one being in turn possibly “divided” into two **hardware
threads**.

Hardware threads are a technology designed to hide the latencies of the
memory and feed the compute units fast enough to keep them busy all the
time. It can be activated or deactivated depending on the cluster.
Depending on the workload, it can benefit or slightly harm the
computations times.

Compute nodes also provide volatile working **memory, or RAM**, where
data are stored when they are being processed or produced, before they
are enventually saved to permanent storage (disks).

Compute nodes can also offer **accelerators**, such as GPUs, which are
pieces of hardware where heavy computations can be offloaded. GPUs are
often much more powerfull than regular processors, but do not have the
same flexibility and require specific programming skills.

.. |FEDGEN HPC Flow| image:: media/FEDGEN_HPC_Layout489.png
   :width: 5.89167in
   :height: 3.69167in
.. |image1| image:: media/image2.jpeg
   :width: 2.86458in
   :height: 2.58333in
