# Running Jobs on Bebop

## Overview

> [!IMPORTANT]
> On July 1, 2024, Bebop was rebuilt from CentOS 7 to Rocky Linux 8 as CentOS has reached End of Life. This rebuild includes a full Operating System change, a new software tree and a transition from Slurm to PBS Professional as the system job scheduler. Please see our [Bebop Rebuild FAQ](../bebop-rebuild-faq.md) for more details.

Bebop's job scheduling system is characterized by:

- Uses [**PBS Pro**](../running-jobs-at-lcrc/pbs-pro-clusters.md)
- Uses the [**`sbank`**](../allocation-management/sbank-allocation-accounting-system.md) accounting system
- Allocations are calculated in [**node hours**](../allocation-management/allocations.md#node-hours-improv-and-bebop-clusters)

## Partition Limits

Bebop currently enforces the following limits on publicly available partitions:

- **32 Running Jobs** per user.
- **100 Queued Jobs** per user.
- **3 Days (72 Hours)** Maximum Walltime on Broadwell Nodes. (bdws is 1 hour)
- **1 Hour** Default Walltime if not specified.
- **bdwall** (Broadwell Compute Nodes) is the default partition.

## Available Partitions

| Bebop Partition Name | Description                        | Number of Nodes | CPU Type                | Cores Per Node | Memory Per Node | Local Scratch Disk |
|----------------------|------------------------------------|-----------------|-------------------------|----------------|-----------------|--------------------|
| bdwall               | All Broadwell Nodes                | 672             | Intel Xeon E5-2695v4    | 36             | 128GB DDR4      | 15 GB or 4 TB      |

## Special Request for Large Local Scratch Disk

The bdwall queue also has 64 nodes with a 4TB local scratch disk. You can request these directly by adding bigdata=true to your PBS select statement. For example:

```bash
#PBS -l select=1:ncpus=36:mpiprocs=36:bigdata=true
```

## Submission Examples

### [Example `qsub` Job Submission](../running-jobs-at-lcrc/pbs-pro-clusters.md#resource-selection-and-job-placement)

### [Example Interactive Job Submission](../running-jobs-at-lcrc/pbs-pro-clusters.md#submitting-an-interactive-job)
