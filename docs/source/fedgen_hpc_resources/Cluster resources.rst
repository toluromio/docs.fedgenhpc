Cluster Resources
----------------------

FEDGEN Datacenter HPC resources includes Compute, GPUs and Storage
Resources which are being increased on an annual basis. The FEDGEN HPC
cluster has very similar hardware with specific processors in variety of
configuration in memory capacity or number of CPUs. All the nodes share
resources over networks for storage and cluster communication

**Resource Manager**

FEDGEN HPC Uses SLURM Resource and Queue Manager Software to manage
Cluster Resources. Jobs submitted to SLURM job queue can run on any
possible nodes dependent on job or cluster constraints. Users only have
to specify `resource
requirements <job_scheduling/Scheduling Jobs.rst>`__ and
SLURM scheduler will assign the job to an appropriate node or set of
nodes.

**Operating Software**

FEDGEN HPC Cluster currently runs on *Ubuntu 22.04* .

**Node Info**

+-----+----+-----------+---+---+------+---------+--------+----------+
| **C | ** | **P       | * | * | **   | **GPUs  | **Node | *        |
| lus | No | rocessors | * | * | Disk | (Nu     | Name** | *Model** |
| ter | de | (Archit   | C | M | Si   | mber)** |        |          |
| Typ | C  | ecture)** | o | e | ze** |         |        |          |
| e** | ou |           | r | m |      |         |        |          |
|     | nt |           | e | o |      |         |        |          |
|     | ** |           | s | r |      |         |        |          |
|     |    |           | * | y |      |         |        |          |
|     |    |           | * | * |      |         |        |          |
|     |    |           |   | * |      |         |        |          |
+-----+----+-----------+---+---+------+---------+--------+----------+
| Fed | 3  | Intel     | 8 | 5 | 41   |         | gi     | Su       |
| gen |    | Xeon 8358 | 0 | 1 | 2 GB |         | ga-[00 | perMicro |
| dc1 |    |           |   | 2 |      |         | 1-003] |          |
|     |    | Hyper     |   | G |      |         |        |          |
|     |    | threading |   | B |      |         |        |          |
|     |    | Enabled   |   |   |      |         |        |          |
|     |    |           |   |   |      |         |        |          |
|     |    | (Cascade  |   |   |      |         |        |          |
|     |    | Lake)     |   |   |      |         |        |          |
+-----+----+-----------+---+---+------+---------+--------+----------+
|     | 3  | Intel     | 2 |   | 41   | Nvidia  | te     | Su       |
|     |    | Xeon 8358 | 4 | 6 | 2 GB | RTX2070 | rx-[00 | perMicro |
|     |    |           |   | 4 |      |         | 4-006] |          |
|     |    | (Ivy      |   | G |      | 4192 MB |        |          |
|     |    | Bridge)   |   | B |      |  memory |        |          |
|     |    |           |   |   |      | (1)     |        |          |
+-----+----+-----------+---+---+------+---------+--------+----------+
|     | 1  | Intel     | 4 | 2 | 41   | Nvidia  | terv   | DELL     |
|     |    | Xeon 8358 | 4 | 0 | 2 GB | V100    | -[007] |          |
|     |    |           |   | 0 |      | SXM     |        |          |
|     |    | (Sky      |   | 5 |      | with    |        |          |
|     |    | Lake)     |   |   |      | 4       |        |          |
|     |    |           |   | G |      | 1920 MB |        |          |
|     |    |           |   | B |      |  memory |        |          |
|     |    |           |   |   |      | (1)     |        |          |
+-----+----+-----------+---+---+------+---------+--------+----------+
|     | 1  | Intel     | 1 | 1 | 1.9  | Nvidia  | tera   | ASUS     |
|     |    | Xeon 8358 | 2 | 0 | 2 TB | A100    | -[008] |          |
|     |    |           | 8 | 2 |      | SXM     |        |          |
|     |    | (Cascade  |   | 4 |      | with    |        |          |
|     |    | Lake)     |   |   |      | 8       |        |          |
|     |    |           |   | G |      | 1920 MB |        |          |
|     |    |           |   | B |      |  memory |        |          |
|     |    |           |   |   |      | (3)     |        |          |
+-----+----+-----------+---+---+------+---------+--------+----------+


**Networks**

All nodes are connected to a 10Gb Ethernet network for Cluster
Communications as well as a **10Gbps** to storage node

**Storage**

- Local Scratch space ~ 412GB High Speed SSD Drive (Available on each
  Node)

- User Home Directory ~ 4TB on network file system (NFS) installed on
  the head node

- Shared Scratch space ~ 14 TB network file system (NFS) provisioned
  from storage node

For more details
see `Storage <Storage.rst>`__ section.
