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

+-----+----+---------+----+-----+----+------------+---------+-------+
| **C | ** | **Pro   | *  | *   | ** | **GPUs     | **Node  | **Mo  |
| lus | No | cessors | *C | *Me | Di | (Number)** | Name**  | del** |
| ter | de | (A      | or | mor | sk |            |         |       |
| Typ | C  | rchitec | es | y** | Si |            |         |       |
| e** | ou | ture)** | ** |     | ze |            |         |       |
|     | nt |         |    |     | ** |            |         |       |
|     | ** |         |    |     |    |            |         |       |
+-----+----+---------+----+-----+----+------------+---------+-------+
| Fed | 3  | Intel   | 80 | 51  | 41 |            | giga-[0 | Super |
| gen |    | Xeon    |    | 2GB | 2  |            | 01-003] | Micro |
| dc1 |    | 8358    |    |     | GB |            |         |       |
|     |    |         |    |     |    |            |         |       |
|     |    | Hyperth |    |     |    |            |         |       |
|     |    | reading |    |     |    |            |         |       |
|     |    | Enabled |    |     |    |            |         |       |
|     |    |         |    |     |    |            |         |       |
|     |    | (       |    |     |    |            |         |       |
|     |    | Cascade |    |     |    |            |         |       |
|     |    | Lake)   |    |     |    |            |         |       |
+-----+----+---------+----+-----+----+------------+---------+-------+
|     | 3  | Intel   | 24 |  6  | 41 | Nvidia     | terx-[0 | Super |
|     |    | Xeon    |    | 4GB | 2  | RTX2070    | 04-006] | Micro |
|     |    | 8358    |    |     | GB |            |         |       |
|     |    | (Ivy    |    |     |    | 4192       |         |       |
|     |    | Bridge) |    |     |    |  MB memory |         |       |
|     |    |         |    |     |    | (1)        |         |       |
+-----+----+---------+----+-----+----+------------+---------+-------+
|     | 1  | Intel   | 44 | 2   | 41 | Nvidia     | ter     | DELL  |
|     |    | Xeon    |    | 005 | 2  | V100 SXM   | v-[007] |       |
|     |    | 8358    |    |  GB | GB | with       |         |       |
|     |    | (Sky    |    |     |    | 41920      |         |       |
|     |    | Lake)   |    |     |    |  MB memory |         |       |
|     |    |         |    |     |    | (1)        |         |       |
+-----+----+---------+----+-----+----+------------+---------+-------+
|     | 1  | Intel   | 1  | 1   | 1  | Nvidia     | ter     | ASUS  |
|     |    | Xeon    | 28 | 024 | .9 | A100 SXM   | a-[008] |       |
|     |    | 8358    |    |  GB | 2  | with       |         |       |
|     |    | (       |    |     | TB | 81920      |         |       |
|     |    | Cascade |    |     |    |  MB memory |         |       |
|     |    | Lake)   |    |     |    | (3)        |         |       |
+-----+----+---------+----+-----+----+------------+---------+-------+

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
