# Running Jobs on Swing

## Overview

Swing's job scheduling system is characterized by:

- Uses [**Slurm**](../running-jobs-at-lcrc/slurm-clusters.md)
- Uses the legacy [**`lcrc-sbank`**](../allocation-management/lcrc-sbank-allocation-accounting-system.md) accounting system
- Allocations are calculated in [**GPU hours**](../allocation-management/allocations.md#gpu-hours-swing-cluster)

## Partition Limits

Swing currently enforces the following limits on publicly available partitions:

- **4** Running Jobs per user.
- **10** Queued Jobs per user.
- **1 Days (24 Hours)** Maximum Walltime.
- **1 Hour** Default Walltime if not specified.
- **16 GPUs (2 full nodes)** Max in use at one time.
- **gpu** is the default partition.

## Slurm Partitions

Swing has two partitions, the default being named gpu. By default, you will be allocated 1/8th of the node resources per GPU.

Nodes allow for multiple jobs from multiple users up until the resources are fully consumed (8 jobs with 1 GPU each per node, 1 job with 8 GPU per node, and everything in between).

You MUST request at least 1 GPU to run a job otherwise you will see the following error:

```
srun: error: Please request at least 1 GPU in the partition 'gpu'
srun: error: e.g '#SBATCH --gres=gpu:1')
srun: error: Unable to allocate resources: Invalid generic resource (gres) specification
```

| Partition Name | Number of Nodes | GPUs Per Node | GPU Memory Per Node | CPUs Per Node | DDR4 Memory Per Node | Local Scratch Disk | Operating System |
| -------------- | --------------- | ------------- | ------------------- | ------------- | -------------------- | ------------------ | ---------------- |
| gpu | 5 | 8x NVIDIA A100 40GB | 320GB | 2x AMD EPYC 7742 64-Core Processor (128 Total Cores) | 1TB | 14TB | Ubuntu 20.04.2 LTS |
| gpu-large | 1 | 8x NVIDIA A100 80GB | 640GB | 2x AMD EPYC 7742 64-Core Processor (128 Total Cores) | 2TB | 28TB | Ubuntu 20.04.2 LTS |

## Submission Examples

### Example `sbatch` Job Submission Script

Here is an example Slurm submission script called `gpu-app-script.sh` that requests available GPUs for the job.

```bash
#!/bin/bash

#SBATCH --job-name=gpu-test
#SBATCH --account=<my_lcrc_project_name>
#SBATCH --nodes=2
#SBATCH --gres=gpu:8
#SBATCH --time=00:05:00

srun ./your-gpu-application
```

You can then submit the script with `sbatch gpu-app-script.sh`.

### Example Interactive Job Submission

To run an interactive job in a computing environment using Slurm, you have two main options:

1. **Direct Node Session with srun:**
    - Use `srun --gres=gpu:1 --pty bash` to get a session on a node with 1 GPU. This session ends once you exit the node.

2. **Resource Allocation with salloc:**
    - Use `salloc -N 2 --gres=gpu:4 -t 00:30:00` to allocate resources for a job. This allocates 2 nodes with 4 GPUs each for 30 minutes. The job number will be provided in the output.
    - To see the list of allocated nodes and Slurm settings, use `printenv | grep SLURM`.
    - After allocation, use `srun` to run your job, or SSH directly to the nodes in your allocation to execute jobs.