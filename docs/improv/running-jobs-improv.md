# Running Jobs on Improv

## Quickstart

Presented below are fundamental commands essential for day-to-day use by most LCRC users on Improv. Comprehensive guides are available in other sections linked within our documentation.

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

Improv's job scheduling system is characterized by:

- Uses [**PBS Pro**](../running-jobs-at-lcrc/pbs-pro.md)
- Uses the [**`sbank`**](../allocation-management/sbank-allocation-accounting-system.md) accounting system
- Allocations are calculated in [**node hours**](../allocation-management/allocations.md#node-hours-improv-and-bebop-clusters)

## Queues

Improv currently enforces the following limits on publicly available queues:

- **15 Running Jobs** per user.
- **100 Queued Jobs** per user.
- **40 Running Nodes** per user.
- **60 Running Nodes** per project.
- **3 Days (72 Hours)** Maximum Walltime.
- **1 Hour** Default Walltime if not specified.
- **compute** (Standard Compute Nodes) is the default queue.

Use the `-q` option with `qsub` to select a queue.

| Improv Queue Name | Description | Number of Nodes | CPU Type | Cores Per Node | Memory Per Node | Local Scratch Disk | Max Walltime |
|-------------------|-------------|-----------------|----------|----------------|-----------------|--------------------|--------------|
| compute | Standard Compute Nodes | 805 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 960GB (6TB bigdata Nodes) | 72 Hours (3 Days) |
| bigmem | Large Memory Compute Nodes | 12 | 2x AMD EPYC 7713 64-Core Processor | 128 | 1TB DDR4 | 6TB | 72 Hours (3 Days) |
| debug | Reduced Walltime Compute Nodes | 8 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 960GB | 1 Hour |
| backfill | Idle Compute Nodes | 825 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB or 1TB DDR4 | 960GB or 6TB | 4 Hours |

The compute queue also has 68 nodes with a 6TB local NVMe scratch disk. You can request these directly by adding `bigdata=true` to your PBS select statement. For example:
```
#PBS -l select=8:ncpus=128:mpiprocs=128:bigdata=true
```

### Backfill Queue

The backfill queue is used to improve the overall efficiency of the cluster by utilizing idle resources that would otherwise remain unused. Only jobs that ran out of hours may use the backfill queue. Users can submit jobs to the backfill queue by specifying it as the target queue in the PBS select statement. For example:

```
#PBS -q backfill
```

An example for interactive jobs:

```
qsub -q backfill -l select=1:ncpus=128:mpiprocs=128 -l walltime=15:00 -A support -I
```

## Running MPI Applications

OpenMPI is the recommended MPI for use on Improv.

Here are some parameters that might be useful:

`-np <processes>`: This specifies the number of processes to launch. It should match the number of processors or cores you've requested in your job script. **If you plan to use all the cores on each node you are allocated, this is not necessary in a submission script. The `#PBS -l select=<# of nodes>:ncpus=128:mpiprocs=128` script header takes care of this.**

`--display-map`: This displays a map showing where MPI processes are running.

`--report-bindings`: This reports how MPI processes are bound to cores or sockets.

For the most accurate and detailed information, please refer to the [OpenMPI documentation](https://docs.open-mpi.org).

```bash
#!/bin/bash -l
# UG Section 2.5, page UG-24 Job Submission Options
# Note: Command line switches will override values in this file.

# ----------------- PBS Directives ----------------- #
# These options are mandatory in LCRC; qsub will fail without them.

# select: number of nodes. Adjust these values as per your job's requirement. In the below example, 4 nodes are requested.
# ncpus: number of cores per node. (use 128 unless you have a reason otherwise)
# mpiprocs: number of MPI processes (use the same value as ncpus unless you have a reason otherwise ).
# walltime: a limit on the total time from the start to the completion of a job
#PBS -A <project_name>
#PBS -l select=4:ncpus=128:mpiprocs=128
#PBS -l walltime=HH:MM:SS

# Queue for the job submission
#PBS -q <queue>

# Job name (first 15 characters are displayed in qstat)
#PBS -N <job_name>

# Output options: 'oe' to merge stdout/stderr into stdout, 'eo' for stderr, 'n' to not merge.
#PBS -j n

# Email notifications: 'b' at begin, 'e' at end, 'a' on abort. Remove 'n' for no emails.
#PBS -m be
#PBS -M <your_email_address>

# --------------- Script Execution Part --------------- #
# The following is an example setup for an MPI job.

echo "Working directory: $PBS_O_WORKDIR"
cd $PBS_O_WORKDIR

echo "Job ID: $PBS_JOBID"
echo "Running on host: $(hostname)"
echo "Running on nodes: $(cat $PBS_NODEFILE)"

# Loading GCC and MPI modules, and verifying the environment.
module load gcc openmpi
module list
which mpirun

# Run the MPI program
mpirun ./hello_mpi
```

**Important things to note:**

- PBS Pro on Improv is currently **not** configured to allow sharing nodes. When have a node allocated to you, you receive the ENTIRE node. Ensure that you use all of your allocated nodes' resources unless you have a reason not to.
- More information about running jobs on PBS can be found on our [Running Jobs on PBS Clusters](https://docs.lcrc.anl.gov/running-jobs-at-lcrc/pbs-pro) page.

## Job Submission Examples

### [Example `qsub` Job Submission](https://docs.lcrc.anl.gov/running-jobs-at-lcrc/pbs-pro#resource-selection-and-job-placement)

### [Example Interactive Job Submission](https://docs.lcrc.anl.gov/running-jobs-at-lcrc/pbs-pro#submitting-an-interactive-job)
