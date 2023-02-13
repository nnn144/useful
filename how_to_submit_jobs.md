# How to submit and run jobs on AI.Panther and Blueshark
***Note 1:*** *For information on using installed packages on AI.Panther and Blueshark, please see [this KB article](https://help.fit.edu/TDClient/39/Portal/KB/ArticleDet?ID=2105&SIDs=271) for more information on using environment modules.*

## 1. Introduction
To submit jobs on AI.Panther and Blueshark, the scheduler, Slurm Workload Manager, must be used to run the job. This differs from how you'd normally run a command, as you need to prepare a submission script and optionally make your code MPI capable.

All software must be run using the Slurm scheduler. These clusters are shared resources; therefore, if a user runs code on the login server, it will cause login and performance issues for others. Users who run on the login node will have their code terminated, and continued disregard will result in the user being blocked from using these clusters.

## 2. Connecting to the AI.Panther and Blueshark
To access the AI.Panther and Blueshark HPC clusters, an SSH client must be installed on your device and used to connect to the HPC clusters.

The AI.Panther hostname for SSH is `ai-panther.fit.edu`

The Blueshark hostname for SSH is `blueshark.fit.edu`

The clusters support using your TRACKS username and password. Please be aware that your TRACKS username is not your email address. It is only the part before the '@' symbol.

## 3. Resource Management
The clusters use the Slurm Workload Manager to manage available resources and to distribute jobs to free compute nodes. Slurm also provides a queuing system; if not enough resources are available, it will hold your job until it can run it. 

### 3.1 Slurm Example Submission Script
In order to submit a job to Slurm, a job submission script must be created. An example submission script is provided below:
***
```bash
#!/bin/bash

#SBATCH --job-name TestJob
#SBATCH --nodes 2
#SBATCH --ntasks 2
#SBATCH --mem=50MB
#SBATCH --time=00:15:00
#SBATCH --partition=short
#SBATCH --error=testjob.%J.err 
#SBATCH --output=testjob.%J.out

module load mpich

echo "Starting at date"
echo "Running on hosts: $SLURM_NODELIST"
echo "Running on $SLURM_NNODES nodes."
echo "Running on $SLURM_NPROCS processors."
echo "Current working directory is pwd"
```
***

The only options you absolutely need are:
  - `--job-name`   — a unique name for your job. This can be set to anything.
  - `--nodes`      — the number of nodes to request.
  - `--ntasks`     — the number of tasks in total across all nodes.
  - `--mem`        — amount of memory to request on each node. This is a hard limit and you will run into out-of-memory errors if you fail to provide the correct amount.
  - `--partition`  — the partition for your job. Valid partitions can be found by using sinfo.

If you need MPICH to run your jobs, set it to load at login using:   
```
module initadd mpich
```
or by placing   
```
module load mpich
```
in your submission script like above. Many more options are available for Slurm's submission scripts, and more information available by visiting: https://slurm.schedmd.com/sbatch.html.

### 3.2 Submitting a Job
Slurm has its own set of commands for job management. To submit your submission script, use 
```
sbatch script.sh
```
Some other commands you may want to use are listed below:
 - `squeue`    — lists the jobs that are currently running for everyone.
 - `sinfo`     — show node status.
 - `scancel`   — cancel a currently running job. 
 - `sstat`     — show statistics for a job.

For a more in-depth look into Slurm and its respective commands, check out the Slurm quick start guide.

### 3.3 Optimizing your Submission Script
Slurm will attempt to run your job wherever it can place it, however, this is hugely dependent on how your submission script specifies its resources. Thus, if you can reduce your submission script requirements, your job has a much higher chance of being scheduled faster.

### 3.4 Memory Requirements
**--mem** is most often used to specify the amount of memory your job will take per node. However, this is largely dependent on how many tasks you can fit in a node or the number of nodes you'll require. If you don't specify the number of nodes you need, slurm won't balance out the tasks, often leading to out-of-memory errors on nodes where more jobs were placed than expected. Another issue arises when the cluster is under heavy use. Small pockets of resources are scattered through the cluster, and won't be easy to acquire when your job needs a fixed amount of memory per node. 

To prevent this, we can use **--mem-per-cpu** instead. If each task only requires a certain amount of memory, you can specify this amount instead. This way, the scheduler can better allocate resources -- if tasks require more memory than what's available on a node, they'll be split, and if there are pockets of resources a single task can fit in, it will allocate that spot.

### 3.5 Partitions
Slurm's partitions can be considered job queues, each of which has an assortment of constraints such as job size limit, job time limit, users permitted to use it, etc. Jobs that only need a short amount of time to run, but a large number of processors, will have their jobs categorized differently than jobs that may need to run for days and require fewer processors. In addition, partitions can be used to group together nodes that have general hardware that others don't (ex, GPU partition has GPUs in its nodes).

To set a partition, use:
```
#sbatch --partition=[partition]
```
in your submission script, or specify it on the command line using `--partition`.

#### 3.5.1 AI.Panther Partitions
These partitions exist on AI.Panther:
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Partition Name</th>
    <th class="tg-0pky">Max Compute Time</th>
    <th class="tg-0pky">Max Nodes</th>
    <th class="tg-0pky">Allowed Groups</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">short</td>
    <td class="tg-0pky">45 minutes</td>
    <td class="tg-0pky">16</td>
    <td class="tg-0pky">AI.Panther users</td>
  </tr>
  <tr>
    <td class="tg-0pky">med</td>
    <td class="tg-0pky">4 hours</td>
    <td class="tg-0pky">16</td>
    <td class="tg-0pky">AI.Panther users</td>
  </tr>
  <tr>
    <td class="tg-0pky">long</td>
    <td class="tg-0pky">7 days</td>
    <td class="tg-0pky">16</td>
    <td class="tg-0pky">AI.Panther users</td>
  </tr>
  <tr>
    <td class="tg-0pky">eternity</td>
    <td class="tg-0pky">infinite</td>
    <td class="tg-0pky">16</td>
    <td class="tg-0pky">AI.Panther users</td>
  </tr>
  <tr>
    <td class="tg-0pky">gpu1</td>
    <td class="tg-0pky">Infinite</td>
    <td class="tg-0pky">4</td>
    <td class="tg-0pky">gpu1 users</td>
  </tr>
  <tr>
    <td class="tg-0pky">gpu2</td>
    <td class="tg-0pky">infinite</td>
    <td class="tg-0pky">4</td>
    <td class="tg-0pky">gpu2 users</td>
  </tr>
</tbody>
</table>

**NOTE**: gpu1 are 4 nodes with 4 A100 GPUs with SXM4 (NVLink) interconnects. This partition is setup for jobs requiring a high amount of bandwidth between GPUs compared to the A100 PCIe nodes. Additional information about comparing the differences: https://infohub.delltechnologies.com/p/accelerating-hpc-workloads-with-nvidia-a100-nvlink-on-dell-poweredge-xe8545/

**NOTE**: gpu2 are 4 nodes with 4 A100 GPUs with PCIe interconnects.

#### 3.5.2 Blueshark Partitions
These partitions exist on Blueshark:
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-w2s2{background-color:#F5F5F5;border-color:inherit;color:#2B2B2B;text-align:left;vertical-align:top}
.tg .tg-svct{background-color:#FFF;border-color:inherit;color:#2B2B2B;text-align:left;vertical-align:middle}
.tg .tg-s0zn{background-color:#FFF;border-color:inherit;color:#2B2B2B;text-align:left;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-svct">Partition Name</th>
    <th class="tg-svct">Max Compute Time</th>
    <th class="tg-svct">Max Nodes</th>
    <th class="tg-svct">Allowed Groups</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-w2s2"><span style="color:#2B2B2B;background-color:#F5F5F5">short</span></td>
    <td class="tg-svct">45 minutes</td>
    <td class="tg-svct">N/A</td>
    <td class="tg-s0zn">blueshark users</td>
  </tr>
  <tr>
    <td class="tg-w2s2"><span style="color:#2B2B2B;background-color:#F5F5F5">med</span></td>
    <td class="tg-svct">4 hours</td>
    <td class="tg-svct">N/A</td>
    <td class="tg-s0zn">blueshark users</td>
  </tr>
  <tr>
    <td class="tg-w2s2"><span style="color:#2B2B2B;background-color:#F5F5F5">long</span></td>
    <td class="tg-svct">7 days</td>
    <td class="tg-svct">N/A</td>
    <td class="tg-s0zn">blueshark users</td>
  </tr>
  <tr>
    <td class="tg-w2s2"><span style="color:#2B2B2B;background-color:#F5F5F5">eternity</span></td>
    <td class="tg-svct">infinite</td>
    <td class="tg-svct">20</td>
    <td class="tg-svct">blueshark users</td>
  </tr>
  <tr>
    <td class="tg-w2s2"><span style="color:#2B2B2B;background-color:#F5F5F5">class</span></td>
    <td class="tg-svct">10 minutes</td>
    <td class="tg-svct">6</td>
    <td class="tg-svct">Parallel Programming class</td>
  </tr>
  <tr>
    <td class="tg-w2s2"><span style="color:#2B2B2B;background-color:#F5F5F5">gpu</span></td>
    <td class="tg-svct">infinite</td>
    <td class="tg-svct">10</td>
    <td class="tg-svct">blueshark GPU users</td>
  </tr>
</tbody>
</table>

### 3.6 Running GPU Jobs
Running GPU jobs is very similar to running regular jobs. An extra parameter has to be passed (`--gres`) and the partition must be set to gpu.
```
#sbatch --partition=gpu
```
will set your partition.
```
#sbatch --gres=gpu:[#] 
```
will set the number of GPUs you want **per node**. Note that this differs from `ntask`, specified earlier. `--gres` will request n number of GPUs from each node. Thus, if you request 4 nodes with `--gres=gpu:2`, you will have 4 nodes * 2 GPU/node = 8 GPUs in total. This option will not exceed 4, as we only have 4 GPUs per node.

Also, GPUs can be selected based on whether or not they support GPUDirect technology. Each GPU node has 2 standard GPUs and 2 GPUDirect enabled GPUs. To select between the two, you can use: 
```
#sbatch --gres=gpu:[type]:[#] 
```
where `[type]` is either gpudirect or standard.