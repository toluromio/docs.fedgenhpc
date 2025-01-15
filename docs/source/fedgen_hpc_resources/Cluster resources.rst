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


+-----------------------+----+----------+---+----+-----+---------+--------+---------+
| **Cluster Type** 	  	| ** | **Pr     | * | ** | **D | **GPUs  | **Node | **      |
|  					      	    | No | ocessors | * | Me | isk | (Nu     | Name** | Model** |
|  						          | de | (Archite | C | mo | Siz | mber)** |        |         |
|  					          	| C  | cture)** | o | ry | e** |         |        |         |
|  						          | ou |          | r | ** |     |         |        |         |
|     				        	| nt |          | e |    |     |         |        |         |
|     					        | ** |          | s |    |     |         |        |         |
|     					        |    |          | * |    |     |         |        |         |
|     					        |    |          | * |    |     |         |        |         |
+-----------------------+----+----------+---+----+-----+---------+--------+---------+
| Fedgendc1 			      | 3  | Intel    | 8 | 5  | 412 |         | gi     | Sup     |
|  						          |    | Xeon     | 0 | 12 |  GB |         | ga-[00 | erMicro |
|  						          |    | 8358     |   | GB |     |         | 1-003] |         |
|     					        |    |          |   |    |     |         |        |         |
|     					        |    | Hypert   |   |    |     |         |        |         |
|     				        	|    | hreading |   |    |     |         |        |         |
|     					        |    | Enabled  |   |    |     |         |        |         |
|     					        |    |          |   |    |     |         |        |         |
|     					        |    | (Cascade |   |    |     |         |        |         |
|     					        |    | Lake)    |   |    |     |         |        |         |
+-----------------------+----+----------+---+----+-----+---------+--------+---------+
|    					          | 3  | Intel    | 2 |    | 412 | Nvidia  | te     | Sup     |
|     					        |    | Xeon     | 4 | 64 |  GB | RTX2070 | rx-[00 | erMicro |
|     					        |    | 8358     |   | GB |     |         | 4-006] |         |
|     					        |    |          |   |    |     | 4192 MB |        |         |
|     					        |    | (Ivy     |   |    |     |  memory |        |         |
|     					        |    | Bridge)  |   |    |     | (1)     |        |         |
+-----------------------+----+----------+---+----+-----+---------+--------+---------+
|     					        | 1  | Intel    | 4 | 2  | 412 | Nvidia  | terv   | DELL    |
|     					        |    | Xeon     | 4 | 00 |  GB | V100    | -[007] |         |
|     					        |    | 8358     |   | 5  |     | SXM     |        |         |
|     					        |    |          |   | GB |     | with    |        |         |
|     					        |    | (Sky     |   |    |     | 4       |        |         |
|     					        |    | Lake)    |   |    |     | 1920 MB |        |         |
|     					        |    |          |   |    |     |  memory |        |         |
|     					        |    |          |   |    |     | (1)     |        |         |
+-----------------------+----+----------+---+----+-----+---------+--------+---------+
|    				          	| 1  | Intel    | 1 | 1  | 1   | Nvidia  | tera   | ASUS    |
|     					        |    | Xeon     | 2 | 02 | .92 | A100    | -[008] |         |
|     					        |    | 8358     | 8 | 4  |  TB | SXM     |        |         |
|     				        	|    |          |   | GB |     | with    |        |         |
|     					        |    | (Cascade |   |    |     | 8       |        |         |
|     					        |    | Lake)    |   |    |     | 1920 MB |        |         |
|     					        |    |          |   |    |     |  memory |        |         |
|     					        |    |          |   |    |     | (3)     |        |         |
+-----------------------+----+----------+---+----+-----+---------+--------+---------+


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
