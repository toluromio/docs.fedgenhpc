Cluster Resources
----------------------

FEDGEN Datacenter HPC resources includes Compute, GPUs and Storage
Resources which are being increased on an annual basis. The FEDGEN HPC
cluster has very similar hardware with specific processors in variety of
configuration in memory capacity or number of CPUs. All the nodes share
resources over networks for data storage and cluster communication.

**Resource Manager**
=====================

FEDGEN HPC Uses SLURM Resource and Queue Manager Software to manage
Cluster Resources. Jobs submitted to SLURM job queue can run on any
possible nodes dependent on job or cluster constraints. Users only have
to specify `resource
requirements <job_scheduling/Scheduling Jobs.rst>`__ and
SLURM scheduler will assign the job to an appropriate node or set of
nodes.

**Operating Software**
==========================

FEDGEN HPC Cluster currently runs on *Ubuntu 22.04* .


**Node Info (Compute/GPU)**
============================

+--------------+------------+---------------------------+-------+---------+-----------+------------------------------------------+----------------+------------+
| Cluster Type | Node Count | Processors (Architecture) | Cores | Memory  | Disk Size | GPUs (Number)                            | Node Name      | Model      |
+==============+============+===========================+=======+=========+===========+==========================================+================+============+
| Fedgendc1    | 3          | Intel Xeon Gold 5218R     | 80    | 512 GB  | 900 GB    |                                          | giga-[001-003] | SuperMicro |
|              |            | Hyperthreading Enabled    |       |         |           |                                          |                |            |
|              |            | (Cascade Lake SP)         |       |         |           |                                          |                |            |
+--------------+------------+---------------------------+-------+---------+-----------+------------------------------------------+----------------+------------+
|              | 3          | Intel Xeon Bronze 3408U   | 8     |  16 GB  | 500 GB    | Nvidia A2000                             | terx-[004-006] | SuperMicro |
|              |            | (Sapphire Rapids)         |       |         |           | 12 GB memory (1)                         |                |            |
+--------------+------------+---------------------------+-------+---------+-----------+------------------------------------------+----------------+------------+
|              | 1          | Intel Silver 4210R        | 20    | 512 GB  | 1000 GB   | Nvidia A100 SXM with 40 GB Memory (1)    | terv-[007]     | ASUS       |
|              |            | (Cascade Lake)            |       |         |           |                                          |                |            |
+--------------+------------+---------------------------+-------+---------+-----------+------------------------------------------+----------------+------------+
|              | 1          | Intel Xeon Platinum 8358  | 128   | 1024 GB | 1.92 TB   | Nvidia A100 SXM with 81920 MB memory (3) | tera-[008]     | SuperMicro |
|              |            | (Cascade Lake)            |       |         |           |                                          |                |            |
+--------------+------------+---------------------------+-------+---------+-----------+------------------------------------------+----------------+------------+

**Networks (Interconnect)**
=============================

All nodes are connected to a **10Gb Ethernet network** for Cluster
Communications as well as a **10Gbps** to storage node

**Storage**
===============

- Local Scratch space ~ High Speed SSD Drive in RAID 1 (Available on each Node) Size is dependent on node

- User Home Directory ~ 4TB on network file system (NFS) available on the login node, the compute nodes and the GPU nodes.

- Shared Scratch space ~ 14 TB network file system (NFS) provisioned from storage node

For more details
see `Storage <Storage.rst>`__ section.
