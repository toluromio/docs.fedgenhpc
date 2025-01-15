FEDGEN HPC Layout
--------------------

The FEDGEN HPC comprises of three different kinds of nodes: the login
node, the compute, accelerator nodes, the management nodes and the storage nodes. In a typical
workflow, users primarily access FEDGEN HPC by connecting to the login
node, where they compile and test code before submitting jobs to
the `SLURM
queue <../job_scheduling/SLURM%20Workload%20ManagerMAIN.rst>`__
which automatically assigns a compute node(s) to the submitted job. This
HPC layout is in the diagram below.

|FEDGEN HPC Flow|

Login Node
===========
The Login node is the only node directly accessible over the internet.
Users connect to the login from their personal computers
using ssh before accessing other parts of the FEDGEN HPC Cluster. Users primarily initiate their jobs
from this node

Compute Nodes
===============
Compute nodes perform the actual computations submitted to the cluster.
They are often organised groupings called **partitions**.
A typical compute node has one, two, or four **sockets** on the
motherboard, that each can host a processor. The processors are made of
multiple **cores**, that can be thought of as independent computing
units, each core can in some cases be enabled to handle two threads simultaneously.


Accelerator Nodes
=================
These are special purpose compute nodes equipped with Graphics Processing Units (GPU) for computation
intesive tasks. The GPU usually offloads certain computations off the CPU for tasks not ideal for it.
ideal 

Storage Nodes
==============
Dedicated nodes for cluster Storage shared filesystem across the entire cluster

Management Nodes
================
these computers run the management services like Slurm, but also the user directory, the monitoring and
reporting applications, etc. ;


.. |FEDGEN HPC Flow| image:: media/FEDGEN_HPC_Layout489.png
   :width: 5.89167in
   :height: 3.69167in
.. |image1| image:: media/image2.jpeg
   :width: 2.86458in
   :height: 2.58333in
