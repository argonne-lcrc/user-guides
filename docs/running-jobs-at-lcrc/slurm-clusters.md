# Running Jobs on Slurm Clusters

This document complements the information provided on the [Swing](../swing/getting-started-swing.md) cluster page, forming a comprehensive resource for running jobs effectively. Both pages should be referenced for a complete understanding.

## Job Submission Commands

The 3 most common tools you will use to submit jobs are `sbatch`, `srun` and `salloc`.

You can reference the table below for a simple, quick cheat sheet on a few examples about jobs in Slurm:

| Slurm Command | Description |
| --- | --- |
| `sbatch <job_script>` | Submit `<job_script>` to the Scheduler |
| `srun <options>` | Run Parallel Jobs |
| `salloc <options>` | Request an Interactive Job |
| `squeue` | View Job Information |
| `scancel <job_id>` | Delete a Job |

### Example `sbatch` Job Submission (Simple)

Here you'll find a couple of very simple submission scripts to get you started that you can use with `sbatch` to submit your job. For this example, the script can be named `myjob.sh`:

```bash
#!/bin/bash

#SBATCH --job-name=<my_job_name>
#SBATCH --account=<my_lcrc_project_name>
#SBATCH --partition=bdwall
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=36
#SBATCH --output=<my_job_name>.out
#SBATCH --error=<my_job_name>.error
#SBATCH --mail-user=<your email address> # Optional if you require email
#SBATCH --mail-type=ALL                  # Optional if you require email
#SBATCH --time=01:00:00

# Run My Program
srun /bin/hostname
```

### Example `sbatch` Job Submission (MPI)

```bash
#!/bin/bash

# SBATCH --job-name=<my_job_name>
# SBATCH --account=<my_lcrc_project_name>
# SBATCH --partition=bdwall
# SBATCH --nodes=2
# SBATCH --ntasks-per-node=36
# SBATCH --output=<my_job_name>.out
# SBATCH --error=<my_job_name>.error
# SBATCH --mail-user=<your_email_address> # Optional if you require email
# SBATCH --mail-type=ALL # Optional if you require email
# SBATCH --time=01:00:00

# Setup My Environment
module load intel-parallel-studio/cluster.2018.4-xtm134f
export I_MPI_FABRICS=shm:tmi

# Run My Program
srun -n 72 ./helloworld
```

NOTE: `I_MPI_FABRICS=shm:tmi` – Use shared memory (shm) for communication within a single host, and the tag matching interface (tmi) (Omni-Path optimized) for host to host communication.

### Example Interactive Job Submission

There are a couple of ways to run an interactive job on Bebop.

First, you can just get a session on a node by using the srun command in the following way:

`srun --pty -p <partition> -t <walltime> /bin/bash`

This will drop you onto one node. Once you exit the node, the allocation will be relinquished.

If you want more flexibility, you can instead have the system first allocate resources for the job using the
the `salloc` command:

`salloc -N 2 -p bdwall -t 00:30:00`

This job will allocate 2 nodes from bdwall partition for 30 minutes. You should get the job number from the output. This command will not log you into any of your allocated nodes by default.

You can get a list of your allocated nodes and many other slurm settings set by the salloc command by doing:

`printenv | grep SLURM`

After the resources were allocated and the session was granted use srun command to run your job:

`srun -n 8 ./myprog`

This will start 8 threads on the allocated nodes. If you try and use more resources than you allocated (say 3 nodes worth of resources while you only asked for 2), this will create a separate reservation and the other will continue to run and use hours as well.

When you allocate resources via `salloc`, you can also now freely SSH to the nodes in your allocation as well if you prefer to run jobs from the nodes themselves.

## Checking Queues and Jobs

To view job and job step information use [`squeue`](https://slurm.schedmd.com/squeue.html).

Here's a quick example of what the output may look like:

```sh
squeue

JOBID   PARTITION       NAME        USER   ST       TIME     NODES  NODELIST(REASON)
999     bdwall          test-joba   user2  R        2:40:31  2      bdw-[0010-0011]
998     bdwall          test-job2   user1  R        45:20    1      bdwd-0120
997     knld            test-job1   user1  R        3:04     1      knld-0030
```

Here are also some common options for squeue:

| Option | Description |
| ------ | ----------- |
| -a  | Display information about all jobs in all partitions. This is the default when running squeue with no options. |
| -u <user_list> | Request jobs or job steps from a comma separated list of users. The list can consist of user names or user id numbers. |
| j <job_id_list> | Requests a comma separated list of job IDs to display. Defaults to all jobs. |
| -l | Report more of the available information for the selected jobs or job steps. |

## Deleting a Job

To delete a job use `scancel` This command will take the job id as its argument. Your job id will be given to when you submit the job. You can also retrieve this from the squeue command detailed above.

`scancel <job_id>`

## Other Useful Slurm Commands

### `scontrol`

`scontrol` can be used to report more detailed information about nodes, partitions, jobs, job steps, and configuration.

Common examples:

| Option | Description |
| ------ | ----------- |
| scontrol show node node-name | Shows detailed information about the nodes. |
| scontrol show partition partition-name | Shows detailed information about a specific partition. |
| scontrol show job job-id | Shows detailed information about a specific job or all jobs if no job id is given. |
| scontrol update job job-id | Change attributes of submitted job. |

For an extensive list of formatting options please consult `scontrol` man page.

### `sinfo`

`sinfo` allows you to view information about jobs, nodes and partitions located in the Slurm scheduling queue

| Option | Description |
| ------ | ----------- |
|-a, --all | Display information about all partitions. |
| -t, --states <states> | Display nodes in a specific state. Example: idle |
| -i <seconds>, --iterate=<seconds> | Print the state on a periodic basis. Sleep for the indicated number of seconds between reports. |
| -l, --long | Print more detailed information.
| -n <nodes>, --nodes=<nodes> | Print information only about the specified node(s). Multiple nodes may be comma separated or expressed using a node range expression. For example “bdw-[0001-0007]” |
| `-o <output_format>`,  `--format=<output_format>` | Specify the information to be displayed using an sinfo format string. |

For an extensive list of formatting options please consult `sinfo` man page.

### `sacct`

`sacct` displays accounting data for all jobs and job steps and can be used to display the information about the complete jobs.

| Option | Description |
| ------ | ----------- |
| `-S`, `--starttime` | Select jobs in any state after the specified time. |
| `-E end_time`, `--endtime=end_time` | Select jobs in any state before the specified time. |

Valid time formats are:

`HH:MM[:SS] [AM|PM]`

`MMDD[YY]` or `MM/DD[/YY]` or `MM.DD[.YY]`

`MM/DD[/YY]-HH:MM[:SS]`

`YYYY-MM-DD[THH:MM[:SS]]`

Example:

```sh
# sacct -S2014-07-03-11:40 -E2014-07-03-12:00 -X -ojobid,start,end,state
JobID                 Start                  End        State
--------- --------------------- -------------------- ------------
2         2014-07-03T11:33:16   2014-07-03T11:59:01   COMPLETED
3         2014-07-03T11:35:21   Unknown               RUNNING
4         2014-07-03T11:35:21   2014-07-03T11:45:21   COMPLETED
5         2014-07-03T11:41:01   Unknown               RUNNING              
```

For an extensive list of formatting options please consult `sacct` man page.

### `sprio`

`sprio` is used to view the components of a job’s scheduling priority when the multi-factor priority plugin is installed. sprio is a read-only utility that extracts information from the multi-factor priority plugin. By default, sprio returns information for all pending jobs. Options exist to display specific jobs by job ID and user name.

For an extensive list of formatting options please consult the `sprio` man page.

## Why Isn’t My Job Running Yet?

If today is NOT LCRC Maintenance Day and you find that your job is in the pending (PD) state after running squeue, Slurm will provide a reason for this shown in the squeue command. Here are a few of the most common reasons your job may not be running.

First, check to the see reason code by querying your job number in Slurm:

`squeue -j <job_id>`

Then, you can determine why the job has not started by deciphering this sample reason list:

| Reason Code | Description |
| ----------- | ----------- |
| AccountNotAllowed | The job isn’t using an account that is allowed on the partition. Certain projects may be restricted to certain partitions. For example, a project may only be allowed to run on the knl partitions. Bebop condo node users must use the ‘condo‘ account when running on their dedicated partitions. |
| AssocGrpBillingMinutes | The job doesn’t have enough time in the banking account to begin. | 
| BadConstraints | The job’s constraints can not be satisfied. |
| BeginTime | The job’s earliest start time has not yet been reached. |
| Cleaning | The job is being requeued and still cleaning up from its previous execution. |
| Dependency | This job is waiting for a dependent job to complete. |
| JobHeldAdmin | The job is held by a system administrator. |
| JobHeldUser | The job is held by the user. |
| NodeDown | A node required by the job is down. |
| PartitionNodeLimit | The number of nodes required by this job is outside of it’s partitions current limits. Can also indicate that required nodes are DOWN or DRAINED. |
| PartitionTimeLimit | The job’s time limit exceeds it’s partition’s current time limit. |
| Priority | One or more higher priority jobs exist for this partition or advanced reservation. |
| QOSMaxJobsPerUserLimit | The job’s QOS has reached its maximum job count for the user at one time. |
| ReqNodeNotAvail | During LCRC Maintenance Day, you may see this reason. In addition, jobs that have a walltime that runs into a scheduled maintenance period will also show this message. The job TimeLimit should be adjusted accordingly. Otherwise, some node specifically required by the job is not currently available. The node may currently be in use, reserved for another job, in an advanced reservation, DOWN, DRAINED, or not responding. Nodes which are DOWN, DRAINED, or not responding will be identified as part of the job’s “reason” field as “UnavailableNodes”. Such nodes will typically require the intervention of a system administrator to make available. |
| Reservation | The job is waiting its advanced reservation to become available. |
| Resources | The job is waiting for resources to become available. |
| TimeLimit | The job exhausted its time limit. |

While this is not every reason code, these are the most common on Bebop. You can view the full list of Slurm reason codes [here](https://slurm.schedmd.com/squeue.html#lbAF).

Assuming your job is in the Priority/Resources state, you can use the `sprio` command to get a closer idea on when your job may start based on the priorities of other pending jobs. The priority is the sum of age, fairshare, jobsize and QOS (quality of service).

## Command Line Quick Reference Guide

| Command | Description |
| ------- | ----------- |
| `sbatch <script_name>` | Submit a job. |
| `scancel <job_id>` | Delete a job. |
| `squeue`<br><br><br>`squeue -u <username>` | Show queued jobs via the scheduler.<br><br>Show queued jobs from a specific user. |
| `scontrol show job <job_id>` | Provide a detailed status report for a specified job via the scheduler. |
| `sinfo -t idle` | Get a list of all free/idle nodes. |
