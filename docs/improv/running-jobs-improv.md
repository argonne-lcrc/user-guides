# Running Jobs on Improv

## Job Scheduling System

Improv's job scheduling system is characterized by:

- Uses [**PBS Pro**](../running-jobs-at-lcrc/pbs-pro-clusters.md)
- Uses the [**`sbank`**](../allocation-management/sbank-allocation-accounting-system.md) accounting system 
- Allocations are calculated in [**node hours**](../allocation-management/allocations.md#node-hours-improv-cluster)

## Queues

Use the `-q` option with `qsub` to select a queue. The default queue is `compute`. We allow up to 15 jobs per user to run at the same time while 100 total jobs can be queued to run.

| Improv Queue Name | Description | Number of Nodes | CPU Type | Cores Per Node | Memory Per Node | Local Scratch Disk | Max Walltime |
|-------------------|-------------|-----------------|----------|----------------|-----------------|--------------------|--------------|
| compute | Standard Compute Nodes | 737 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 960GB | 72 Hours (3 Days) |
| bigmem | Large Memory Compute Nodes | 12 | 2x AMD EPYC 7713 64-Core Processor | 128 | 1TB DDR4 | 6TB | 72 Hours (3 Days) |
| bigdata | Large Scratch Compute Nodes | 68 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 6TB | 72 Hours (3 Days) |
| debug | Reduced Walltime Compute Nodes | 8 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 960GB | 1 Hour |

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

* PBS Pro on Improv is currently **not** configured to allow sharing nodes. When have a node allocated to you, you receive the ENTIRE node. Ensure that you use all of your allocated nodes' resources unless you have a reason not to.
