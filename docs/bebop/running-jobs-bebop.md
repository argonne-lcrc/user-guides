# Running Jobs on Bebop

> **NOTE:** 
> On July 1, 2024, Bebop was rebuilt from CentOS 7 to Rocky Linux 8 as CentOS has reached End of Life. This rebuild includes a full Operating System change, a new software tree and a transition from Slurm to PBS Professional as the system job scheduler. Please see our [Bebop Rebuild FAQ](../bebop-rebuild-faq) for more details.

## Quickstart

Presented below are fundamental commands essential for day-to-day use by most LCRC users on Bebop. Comprehensive guides are available in other sections linked within our documentation.

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

Bebop's job scheduling system is characterized by:

- Uses [**PBS Pro**](../running-jobs-at-lcrc/pbs-pro.md)
- Uses the [**`sbank`**](../allocation-management/sbank-allocation-accounting-system.md) accounting system
- Allocations are calculated in [**node hours**](../allocation-management/allocations.md#node-hours-improv-and-bebop-clusters)

## Queues

Bebop currently enforces the following limits on publicly available queues:

- **32 Running Jobs** per user.
- **100 Queued Jobs** per user.
- **40 Running Nodes** per user.
- **60 Running Nodes** per project.
- **3 Days (72 Hours)** Maximum Walltime.
- **1 Hour** Default Walltime if not specified.
- **bdwall** (Broadwell Compute Nodes) is the default queue.

Use the -q option with qsub to select a queue.

| Bebop Queue Name | Description                        | Number of Nodes | CPU Type                | Cores Per Node | Memory Per Node | Local Scratch Disk |
|----------------------|------------------------------------|-----------------|-------------------------|----------------|-----------------|--------------------|
| bdwall               | All Broadwell Nodes                | 672             | Intel Xeon E5-2695v4    | 36             | 128GB DDR4      | 15 GB or 4 TB      |
| backfill             | All Broadwell Nodes                | 672             | Intel Xeon E5-2695v4    | 36             | 128GB DDR4      | 15 GB or 4 TB      |

### Special Request for Large Local Scratch Disk

The bdwall queue also has 64 nodes with a 4TB local scratch disk. You can request these directly by adding bigdata=true to your PBS select statement. For example:

```bash
#PBS -l select=1:ncpus=36:mpiprocs=36:bigdata=true
```

### Backfill Queue

The backfill queue is used to improve the overall efficiency of the cluster by utilizing idle resources that would otherwise remain unused. **Only jobs that ran out of hours may use the backfill queue, and the maximum wall-time is 4 hours.** Users can submit jobs to the backfill queue by specifying it as the target queue in the PBS select statement. For example:

```
#PBS -q backfill
```

An example for interactive jobs:

```
qsub -q backfill -l select=1:ncpus=36:mpiprocs=36 -l walltime=15:00 -A support -I
```


## Job Submission Examples

### [Example `qsub` Job Submission](https://docs.lcrc.anl.gov/running-jobs-at-lcrc/pbs-pro#resource-selection-and-job-placement)

### [Example Interactive Job Submission](https://docs.lcrc.anl.gov/running-jobs-at-lcrc/pbs-pro#submitting-an-interactive-job)
