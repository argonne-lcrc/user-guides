# Running Jobs on Swing

## Partition Limits

Swing currently enforces the following limits on publicly available partitions:

- **4** Running Jobs per user.
- **10** Queued Jobs per user.
- **1 Days (24 Hours)** Maximum Walltime.
- **1 Hour** Default Walltime if not specified.
- **16 GPUs (2 full nodes)** Max in use at one time.
- **gpu** is the default partition.

## PBS Partitions

Swing has two partitions, the default being named gpu. By default, you will be allocated 1/8th of the node resources per GPU.

Nodes allow for multiple jobs from multiple users up until the resources are fully consumed (8 jobs with 1 GPU each per node, 1 job with 8 GPU per node, and everything in between).

You **MUST** request at least 1 GPU to run a job. Additionally, you may only request the following number of GPUs per node:

- 1 GPU
- 2 GPUs
- 4 GPUs
- 8 GPUs

| Partition Name | Number of Nodes | GPUs Per Node | GPU Memory Per Node | CPUs Per Node | DDR4 Memory Per Node | Local Scratch Disk | Operating System |
| -------------- | --------------- | ------------- | ------------------- | ------------- | -------------------- | ------------------ | ---------------- |
| gpu | 5 | 8x NVIDIA A100 40GB | 320GB | 2x AMD EPYC 7742 64-Core Processor (128 Total Cores) | 1TB | 14TB | Ubuntu 20.04.2 LTS |
| gpu-large | 1 | 8x NVIDIA A100 80GB | 640GB | 2x AMD EPYC 7742 64-Core Processor (128 Total Cores) | 2TB | 28TB | Ubuntu 20.04.2 LTS |

## Submission Examples

### Example `qsub` Job Submission Script

Here is an example PBS submission script called `gpu-app-script.sh` that requests a single GPU for the job.

```bash
#!/bin/bash -l
#PBS -N gpu-test
#PBS -A support
#PBS -l select=1:ngpus=1
#PBS -o logout.txt
#PBS -j oe
#PBS -l walltime=04:00:00

cd $PBS_O_WORKDIR
echo Working directory is $PBS_O_WORKDIR
cat /dev/null > logout.txt

module purge
module load nvhpc tensorflow-gpu/2.4.1

printf "All Nodes in Job: $SLURM_JOB_NODELIST\n"
printf "CUDA_VISIBLE_DEVICES: $CUDA_VISIBLE_DEVICES\n\n"

nvidia-smi
time python tf-test.py
sleep 60
exit 0
```

You can then submit the script with `qsub gpu-app-script.sh`.

### Example Interactive Job Submission

To run an interactive job in a computing environment using PBS, you can do the following:

```bash
qsub -l select=1:ngpus=1 -l walltime=15:00 -q gpu -A support -k doe -I
```

This command requests 1 node for a period of 15 hour in the gpu queue. After waiting in the queue for a node to become available, a shell prompt on a compute node will appear. You may then start building applications and testing gpu affinity scripts on the compute node.
