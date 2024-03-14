# Running Jobs on Bebop

## Overview

Bebop's job scheduling system is characterized by:

- Uses [**Slurm**](../running-jobs-at-lcrc/slurm-clusters.md)
- Uses the legacy [**`lcrc-sbank`**](../allocation-management/lcrc-sbank-allocation-accounting-system.md) accounting system
- Allocations are calculated in [**core hours**](../allocation-management/allocations.md#core-hours-bebop-cluster)

## Partition Limits

Bebop currently enforces the following limits on publicly available partitions:

- **32 Running Jobs** per user.
- **100 Queued Jobs** per user.
- **3 Days (72 Hours)** Maximum Walltime on Broadwell Nodes. (bdws is 1 hour)
- **7 Days (168 Hours)** Maximum Walltime on KNL Nodes. (knls is 4 hours)
- **1 Hour** Default Walltime if not specified.
- **bdwall** (Broadwell Compute Nodes) is the default partition.

## Available Partitions

| Bebop Partition Name | Description                        | Number of Nodes | CPU Type                | Cores Per Node | Memory Per Node | Local Scratch Disk |
|----------------------|------------------------------------|-----------------|-------------------------|----------------|-----------------|--------------------|
| bdwall               | All Broadwell Nodes                | 664             | Intel Xeon E5-2695v4    | 36             | 128GB DDR4      | 15 GB or 4 TB      |
| bdw                  | Broadwell Nodes with 15 GB / scratch | 600           | Intel Xeon E5-2695v4    | 36             | 128GB DDR4      | 15 GB              |
| bdwd                 | Broadwell Nodes with 4 TB / scratch  | 64            | Intel Xeon E5-2695v4    | 36             | 128GB DDR4      | 4 TB               |
| bdws                 | Broadwell Shared Nodes (Oversubscription / Non-Exclusive) | 8                 | Intel Xeon E5-2695v4    | 36             | 128GB DDR4      | 15 GB              |
| knlall               | All Knights Landing Nodes            | 348           | Intel Xeon Phi 7230     | 64             | 96GB DDR4/16GB MCDRAM | 15 GB or 4 TB  |
| knl                  | Knights Landing Nodes with 15GB /scratch | 284         | Intel Xeon Phi 7230     | 64             | 96GB DDR4/16GB MCDRAM | 15 GB          |
| knld                 | Knights Landing Nodes with 4TB /scratch | 64          | Intel Xeon Phi 7230     | 64             | 96GB DDR4/16GB MCDRAM | 4 TB           |
| knls                 | Knights Landing Shared Nodes (Oversubscription / Non-Exclusive) | 4               | Intel Xeon Phi 7230     | 64             | 96GB DDR4/16GB MCDRAM | 15 GB          |
| knl-preemptable      | All Knights Landing Nodes with restrictions including preemption. [Click here for more details](#). | 348       | Intel Xeon Phi 7230     | 64             | 96GB DDR4/16GB MCDRAM | 15 GB or 4 TB  |

## Submission Examples

### [Example `sbatch` Job Submission (Simple)](../running-jobs-at-lcrc/slurm-clusters.md#example-sbatch-job-submission-simple)

### [Example `sbatch` Job Submission (MPI)](../running-jobs-at-lcrc/slurm-clusters.md#example-sbatch-job-submission-mpi)

### [Example Interactive Job Submission](../running-jobs-at-lcrc/slurm-clusters.md#example-interactive-job-submission)

### Example Knights Landing (KNL) `sbatch` Job Submission (MPI)

>**Attention KNL Users:**
>Please note that if you wish to use a different set of modes for KNL other than Quadrant & Cache, you'll need to request a reservation that will require approval. You can request your reservation via [this form](https://your-reservation-link.com).

Depending on the amount of KNL nodes and the changes to be made, this could take a decent amount of time. Because this is using the resources, this time will also be charged against your core hour usage including the time it takes to complete the job.

```bash
#!/bin/bash

# SBATCH --job-name=<my_job_name>
# SBATCH --account=<my_lcrc_project_name>
# SBATCH --partition=knlall
# SBATCH --constraint knl,quad,cache # Other modes require a reservation.
# SBATCH --nodes=2
# SBATCH --ntasks-per-node=64
# SBATCH --output=<my_job_name>.out
# SBATCH --error=<my_job_name>.error
# SBATCH --mail-user=<your email address> # Optional if you require email
# SBATCH --mail-type=ALL # Optional if you require email
# SBATCH --time=01:00:00

# Setup My Environment
module load intel-parallel-studio/cluster.2018.4-xtm134f
export I_MPI_FABRICS=shm:tmi

# Run My Program
srun -n 128 ./helloworld
```

This will run across two KNL nodes, using the Quadrant mode, and Cache MCDRAM mode.
The default setting is quad,cache.

A table of available settings, along with more detailed information about Slurm’s KNL support is available [here](https://slurm.schedmd.com/intel_knl.html).

You can then submit this job from a Bebop login nodes using:

`sbatch myjob.sh`

Please refer to the [sbatch](https://slurm.schedmd.com/sbatch.html) webpage for a list of full options including environment variables.

## KNL Preemptable Queue

LCRC has set up a preemptable queue on the KNL partition (named as knl-preemptable). This covers all of the same nodes in the knlall partition. The preemptable queue is largely targeted towards users who need to run jobs but do not have sufficient time available in their projects or wish to stretch their allocations further. As the name implies, these jobs are preemptable immediately by other normal jobs if the partition (knlall) is full. The main advantage to users is that preemptable jobs are charged at 0.2 (20%) the normal core-hours.

The user has to have an active project with some remaining time to be able to submit jobs to the preemptable queue.

The rules for submitting jobs to the preemptable queue are as follows:

Maximum job size: 6 nodes
Maximum time: 24 hours
Maximum number of running jobs per project: 1

A single user can submit multiple jobs to the preemptable queue if they have more than one project. If a user submits two preemptable jobs in any given project, only one of the jobs will run while the other is queued.

As an example of the core-hour savings, a job run for 24 hours on 6 KNL nodes would be charged:
6x64x24x0.585 = 5391 core-hours (0.585 is the scaling factor for jobs run on the knl partition) whereas in the preemptable queue, the core-hours charged would be:
6x64x24x0.2 = 1843 core-hours (0.2 is the additional scaling factor for preemptable jobs).

It is important that the user have checkpointing available and enabled so intermediate solutions are being saved at regular intervals as the job is running. If a job is preempted, the job can be automatically requeued so it can start again as new resources become available if the requeue flag is included during job submission. It is also important that users make appropriate changes to their scripts and input files to have the requeued job start from the latest saved solution. It is recommended that users periodically clean up their project spaces to delete intermediate solution files they might not require after the preemptable job is completed.

Users wishing to submit jobs to the preemptable queue should include the line
`#SBATCH –partition=knl-preemptable`
in their batch script.

It is recommended that users include the following lines in their batch-scripts if they desire the job to be requeued and to be notified if their jobs have been preempted and requeued. This will enable them to make necessary changes to the input file to restart the jobs from the last saved solution (if need be).

```bash
#SBATCH –requeue
#SBATCH –mail-user=<your email address>
#SBATCH –mail-type=REQUEUE
```