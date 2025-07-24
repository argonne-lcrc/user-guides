# Running Jobs on Swing

## Quickstart

Presented below are fundamental commands essential for day-to-day use by most LCRC users on Swing. Comprehensive guides are available in other sections linked within our documentation.

Check your Current Allocation Balance(s):
```
sbank-list-allocations -p <project_name>
```

Check your Filesystem Quota(s):
```
lcrc-quota
```

Submit a Batch Job:
```
qsub -A <project> <your job script>
```

List All Jobs:
```
qstat
```

Delete a Job:
```
qdel <jobid>
```

## Job Scheduling System

Swing's job scheduling system is characterized by:

- Uses [**PBS Pro**](../running-jobs-at-lcrc/pbs-pro.md)
- Uses the [**`sbank`**](../allocation-management/sbank-allocation-accounting-system.md) accounting system
- Allocations are calculated in [**node hours**](../allocation-management/allocations.md#swing-cluster)

## Queues

Swing currently enforces the following limits on publicly available queues:

- **4** Running Jobs per user.
- **10** Queued Jobs per user.
- **1 Days (24 Hours)** Maximum Walltime.
- **1 Hour** Default Walltime if not specified.
- **16 GPUs (2 full nodes)** Max in use at one time.
- **gpu** is the default partition.

Use the -q option with qsub to select a queue.

You will be allocated 1/8th of the node resources per GPU. Nodes allow for multiple jobs from multiple users up until the resources are fully consumed (8 jobs with 1 GPU each per node, 1 job with 8 GPU per node, and everything in between).

You **MUST** request at least 1 GPU to run a job. Additionally, you may only request the following number of GPUs per node:

- 1 GPU
- 2 GPUs
- 4 GPUs
- 8 GPUs

| Partition Name | Number of Nodes | GPUs Per Node | GPU Memory Per Node | CPUs Per Node | DDR4 Memory Per Node | Local Scratch Disk | Operating System |
| -------------- | --------------- | ------------- | ------------------- | ------------- | -------------------- | ------------------ | ---------------- |
| gpu | 5 | 8x NVIDIA A100 40GB | 320GB | 2x AMD EPYC 7742 64-Core Processor (128 Total Cores) | 1TB | 14TB | Ubuntu 22.04.5 LTS |
| gpu-large | 1 | 8x NVIDIA A100 80GB | 640GB | 2x AMD EPYC 7742 64-Core Processor (128 Total Cores) | 2TB | 28TB | Ubuntu 22.04.5 LTS |
| backfill | 6 | 8x NVIDIA A100 40GB/80GB | 320GB/640GB | 2x AMD EPYC 7742 64-Core Processor (128 Total Cores) | 1TB/2TB | 14TB/28TB | Ubuntu 22.04.5 LTS |

### Backfill Queue

The backfill queue is used to improve the overall efficiency of the cluster by utilizing idle resources that would otherwise remain unused. Only jobs that ran out of hours may use the backfill queue. Users can submit jobs to the backfill queue by specifying it as the target queue in the PBS select statement. For example:

```
#PBS -q backfill
```

An example for interactive jobs:

```
qsub -q backfill -l select=1:ngpus=1 -l walltime=15:00 -A support -I
```

## Job Submission Examples

### Example `qsub` Job Submission Script

Here is an example PBS submission script called `gpu-app-script.sh` that requests a single GPU for the job.

```bash
#!/bin/bash -l
#PBS -N gpu-test
#PBS -A support
#PBS -l select=1:ngpus=1
#PBS -j oe
#PBS -l walltime=04:00:00

cd $PBS_O_WORKDIR
echo Working directory is $PBS_O_WORKDIR

module purge
module load nvhpc

printf "CUDA_VISIBLE_DEVICES: $CUDA_VISIBLE_DEVICES\n\n"

nvidia-smi
exit 0
```

You can then submit the script with `qsub gpu-app-script.sh`.

### Example Interactive Job Submission

To run an interactive job in a computing environment using PBS, you can do the following:

```bash
qsub -I -l select=1:ngpus=1 -l walltime=01:00:00 -q gpu -A <project_name>
```

This command requests 1 node and 1 gpu for a period of 1 hour in the gpu queue. After waiting in the queue for a node to become available, a shell prompt on a compute node will appear. You may then start building applications and testing gpu affinity scripts on the compute node.
