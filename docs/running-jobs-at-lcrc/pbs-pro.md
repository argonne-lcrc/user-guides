# Running Jobs on PBS Clusters

This document complements the information provided on the [Improv](../improv/getting-started-improv.md), [Bebop](../bebop/getting-started-bebop.md), and [Swing](../swing/getting-started-swing.md) pages, forming a comprehensive resource for running jobs effectively. Both pages should be referenced for a complete understanding.

## Obtaining and Managing Compute Resources

### Definitions and Notes

`chunk`: A set of resources allocated as a unit to a job. Specified inside a selection directive. All parts of a chunk come from the same host. In a typical MPI (Message-Passing Interface) job, there is one chunk per MPI process.

`vnode`: A virtual node, or vnode, is an abstract object representing a host or a set of resources which form a usable part of an execution host. This could be an entire host, or a nodeboard or a blade. A single host can be made up of multiple vnodes. Each vnode can be managed and scheduled independently. Each vnode in a complex must have a unique name. Vnodes on a host can share resources, such as node-locked licenses. PBS operates on vnodes. **A vnode on Improv and Bebop represents an entire host**.

`ncpus`: Number of resources available to execute a program. Users are encouraged to use all of the cores on the node and pack nodes with multiple jobs when possible.

`ngpus`: Only relevant on [Swing](../swing/getting-started-swing.md); Requests the specified number of GPUs. Users can only request 1, 2, 4, or 8 at a time.

`job`: A job equates to a qsub. A set of resources allocated to you for a period of time. Your will execute one or more tasks on those resources during your job.

`task`: A single execution on the resources of your job, often an mpirun invocation. You may run one task or many tasks during your job. You may run tasks sequentially or divide your resources up and run several tasks concurrently. Also sometimes referred to as job steps.

### Quick Start

If you are an LCRC user and are familiar with Slurm, you will find the PBS Pro commands very similar though the options to qsub are quite different. We have added a handy conversion “cheat sheet” here. Here are the “Big Four” commands you will use with PBS Pro:

1. `qsub`: request resources (generally compute nodes) to run your job and start your script/executable on the head node. Here is the minimal qsub allowed at the LCRC:
    * `qsub -A <project> -l select=<# of nodes>,walltime=HH:MM:SS <your job script>`
    * The `-A` and walltime options are mandatory. You will receive errors if they are not specified.
    * We automatically add `-l place=scatter` for you so that each of your chunks (`<# of nodes>`) gets its own vnode.
    * `-q <queue_name>` will place your job on the correct queue depending on the node type you want. The compute queue is the default on Improv.
    * If you want to run an executable rather than a script replace `<your jobs script>` in the example above with `-- <your executable>` (that is dash dash)
2. `pbsq`: a user-friendly filter for `qstat` to view the status of jobs and queues on the cluster.
3. `qstat`: view the status of jobs and queues on the cluster.
    * Try these variations and see which you like best: `qstat, qstat -was, qstat -was1, qstat -wan, qstat -wan1`. Add `-x` to see jobs that have completed. We keep one week of history.
4. `qalter`: update your request for resources
    * Just like `qsub`, just add a jobid at the end. Only works before the job starts;
    * If you want to change the walltime to 30 minutes: `qalter -l walltime=30:00:00 <jobid>`
5. `qdel`: cancel a job that you don’t need. This will also kill a running job.
    * `qdel <jobid>`

**Note: The page numbers in the PBS guides are unique. If you search for the specified page number it will take you directly to the relevant page.**

### qsub: Submit a job to run

At the LCRC, your qsub will likely use the following parameters:

`qsub -A <project> -l select=<#>,walltime=HH:MM:SS <your job script>`

Where:

* `<project>` is the project name associated with your allocation. What you check the balance of with the `sbank` command. This is a mandatory option at the LCRC. If you don’t include it you will get `qsub: Account_Name is required to be set`.
* `walltime=HH:MM:SS` specifying a wall time is mandatory at the LCRC. Jobs on Improv can run up to 72 hours maximum.
* `<your job script>`: For options that won’t change, you do have the option of taking things off the command line and putting them in your job script. For instance the above command line could be simplified to `qsub -l select=<#> <your job script>` if you added the following to the top (the PBS directives have to be before any executable line) of your job script:

```bash
#PBS -A <project>
#PBS -l walltime=HH:MM:SS
```

Also note that if you want to run an executable directly rather than a script you use two dashes and the executable name in place of your script name like this: `-- /usr/bin/sleep 600`

### Resource Selection and Job Placement

Resources come in two flavors:

* Job Wide: Walltime is the most common example of a job wide resource. You use the `-l` option to specify job wide resources, i.e. `-l walltime=06:00:00`. All the resources in the job have the same walltime.
* `-l <resource name>=<value>[,<resource name>=<value> ...]`
* Chunks: (see the definition above) This is how you describe what your needs are to run your job. You do this with the `-l select=` syntax. In the LCRC, we do whole node scheduling. This means you can typically get away with the very simple `-l select=4` which will give you 4 nodes with all of the available cpus.
* `<resource name>=<value>[:<resource name>=<value> ...]`

You can also tell PBS how you want the chunks distributed across the physical hardware. You do that via the `-l place` option (scatter is our default):

* `-l place=[<arrangement>][: <sharing> ][: <grouping>]` where
* arrangement is one of `free | pack | scatter`
* unless you have a specific reason to do otherwise, you probably want to set this to scatter, otherwise you may not get what you expect. For instance on a host with ncpus=128, if you requested -l select=8:ncpus=8:mpiprocs=8 you could end up with all of our chunks on one node.
* `free` means PBS can distribute them as it sees fit
* `pack` means all chunks from one host. Note that this is not the minimum number of hosts, it is one host. If the chunks can’t fit on one host, the qsub will fail.
* `scatter` means take only one chunk from any given host.

Here is a heavily commented sample PBS submission script that shows some more of the options, but remember that the PBS manuals referenced at the bottom of this page are the ultimate resource. If you create a file named hello.pbs for example, you can add:

```bash
#!/bin/bash -l
# Add another # at the beginning of the line to comment out a line
# NOTE: adding a switch to the command line will override values in this file.

# These options are MANDATORY in LCRC; Your qsub will fail if you don't provide them.
#PBS -A <project_name>
#PBS -l select=4:mpiprocs=128
#PBS -l walltime=HH:MM:SS

# Highly recommended
# The first 15 characters of the job name are displayed in the qstat output:
#PBS -N <job_name>

# If you want to merge stdout and stderr, use the -j option
# oe=merge stdout/stderr to stdout, eo=merge stderr/stdout to stderr, n=don't merge
#PBS -j n

# Controlling email notifications
# When to send email b=job begin, e=job end, a=job abort, j=subjobs (job arrays), n=no mail
#PBS -m be
#PBS -M <your_email_address>

# The rest is an example of how an MPI job might be set up
echo Working directory is $PBS_O_WORKDIR
cd $PBS_O_WORKDIR

echo Jobid: $PBS_JOBID
echo Running on host `hostname`
echo Running on nodes `cat $PBS_NODEFILE`

module load gcc openmpi    # Load a GCC and MPI module
module list                # Print all loaded modules
which mpirun               # Show which mpirun executable we are using
mpirun ./hello_mpi         # Run a script
```

You would be able to submit the above script with:

`qsub hello.pbs`

#### qsub example

If you don’t want to use a script, you could submit via the command line as well. For example:

`qsub -A <project_name> -l select=4:mpiprocs=128 -l walltime=30:00 -- a.out`

* run a.out on 4 chunks with a walltime of 30 minutes ; charge project_name;
* run on 128 CPUs (ncpus) per node. On Improv by default, we allocate 128 cpus per node. Set ncpus if you want less. All jobs will be charged a whole node no matter how many cpus are requested.
* Allocate 128 MPI slots (mpiprocs) per node. Note: When using MPI, you must specify mpiprocs. We recommend it to be the same as ncpus.
* Since we allocate full nodes on Improv, 4 chunks will be 4 nodes. If we shared nodes, that would be 4 threads.
* use the `--` (dash dash) syntax when directly running an executable.

### pbsq: A user-friendly filter for qstat

`pbsq` is a user-friendly filter for `qstat` to view the status of jobs and queues on the cluster. It provides a more user-friendly interface for `qstat` by providing a more intuitive way to view job and queue status.

#### pbsq basic usage

Query all jobs that are running and queued:

`pbsq`

Query all jobs that belong to a project:

`pbsq -f <project_name>`

Query all jobs that belong to a user:

`pbsq -f <user_name>`

Use `pbsq -h` to see the help menu and other options.

### qstat: Query the status of jobs/queues

The `qstat` command lists jobs on the system, showing their status, user, and other details. You can specify job IDs for detailed information about specific jobs.

#### qstat basic usage

`qstat`: Lists all jobs with basic details.

`qstat [jobID]`: Displays information for specific jobs.

**Options for expanded information:**

`-w`: Expands the width of the output to reduce truncation.

`-was1`: Shows nodes, tasks, requested walltime, and comments in a single line.

`-wan`: Provides the list of nodes used.

`-T`: Estimates the start time for the job (only for the next expected jobs).

`-f`: Provides comprehensive details about a job.

`-x`: Shows finished jobs (history retained for one week) along with comments.

The comment field is crucial for understanding job status or issues. It's often the first place to check when troubleshooting job-related queries in PBS.

### qalter: Alter a queued job

Basically takes the same options as `qsub`; Say you typoed and set the walltime to 300 minutes instead of 30 minutes. You could fix it (if the job had not started running) by doing `qalter -l walltime=30:00 <jobid> [<jobid> <jobid>...]` The new value overwrites any previous value.

### qdel: Delete a queued or running job

`qdel <jobid> [<jobid> <jobid>...]`

### qhold,qrls: Place / release a user hold on a job

`[qhold | qrls] <jobid> [<jobid> <jobid>...]`

### qselect: Query jobids for use in commands

`qdel $(qselect -N test1)` will delete all the jobs that had the job name set to test1.

### qmsg: Write a message into a jobs output file

`qmsg -E -O "This is the message" <jobid> [<jobid> <jobid>...]`

`-E` writes it to standard error, `-O` writes it to standard out

### qsig: Send a signal to a job

```console
qsig -s <signal> <jobid> [<jobid> <jobid>...]
```

If you don’t specify a signal, `SIGTERM` is sent.

### pbsnodes: Get information about the current state of nodes

This is more for admins, but it can tell you more about the nodes themselves.

`pbsnodes <node name>`: Everything there is to know about a node

`pbsnodes -avSj`: A nice table to see what is free and in use

`pbsnodes -l`: (lowercase l) see which nodes are down. The comment often indicates why it is down

## Job Priority

In PBS it is not easy to see a priority order for which jobs will run next. The best way is to use the `-T` option on `qsub` and look at the estimated start times. LCRC runs a custom scheduler algorithm, but in general, the job priority in the queue is based on several criteria:

1. positive balance of your project
2. size (in nodes) of the job, larger jobs receive higher priority
3. job duration: shorter duration jobs will accumulate priority more quickly, so it is best to specify the job run time as accurately as possible

## General PBS Example Scripts/Commands

### Submitting an Interactive Job

Here is how to submit an interactive job to, for example, edit/build/test an application:

```console
qsub -I -A <PROJECT_NAME> -l select=1:ncpus=128:mpiprocs=128,walltime=01:00:00 -q compute
```

This command requests 1 node for a period of 1 hour in the compute queue. After waiting in the queue for a node to become available, a shell prompt on a compute node will appear. Remember to replace `<PROJECT_NAME>` with a valid LCRC project.

### Submitting a PBS Array Job

```bash
#!/bin/bash

###### Tells PBS the job name
#PBS -N array_example
###### Tells PBS to run on 1 node with 128 cpus
#PBS -l select=1:ncpus=128:mpiprocs=128
###### Tells PBS the walltime (Max 72 hours)
#PBS -l walltime=00:05:00
###### Tells PBS the project to charge (Replace with a valid project)
#PBS -A <PROJECT_NAME>
###### Tells PBS to run 5 subjobs (Required to run in an array)
#PBS -J 1-5
###### Tells PBS to rerun the job (Rquired to run in an array)
#PBS -r y

cd $PBS_O_WORKDIR
echo "Running subjob $PBS_ARRAY_INDEX"

# Create a subjob-specific directory
mkdir -p subjob_${PBS_ARRAY_INDEX}
cd subjob_${PBS_ARRAY_INDEX}

# Run a command unique to each subjob
echo "This is subjob ${PBS_ARRAY_INDEX}" > output_${PBS_ARRAY_INDEX}.txt

# Sleep for a different amount of time based on the subjob index
sleep ${PBS_ARRAY_INDEX}

echo "Subjob $PBS_ARRAY_INDEX completed"
```

The `pbsq` command subsequently shows the status of the job array.

```console
$ pbsq
138617[].imgt1    user1             support               00:05:00    00:00:04  n/a       00:00:06       00:04:54      1  B      compute  array_example     Job Array Began at Thu Mar 21 at 12:02
138617[1].imgt1   user1             support               00:05:00    00:00:04  n/a       00:00:06       00:04:54      1  E      compute  array_example     i475
138617[2].imgt1   user1             support               00:05:00    00:00:04  n/a       00:00:06       00:04:54      1  E      compute  array_example     i730
138617[3].imgt1   user1             support               00:05:00    00:00:04  n/a       00:00:06       00:04:54      1  E      compute  array_example     i799
138617[4].imgt1   user1             support               00:05:00    00:00:04  n/a       00:00:06       00:04:54      1  E      compute  array_example     i285
138617[5].imgt1   user1             support               00:05:00    00:00:04  n/a       00:00:06       00:04:54      1  E      compute  array_example     i286
```

If you want to increase the number of nodes per subjob, you can increase the `select` value in `#PBS -l select=1:ncpus=128` to be equal to the number of nodes you want per subjob.

### Running Multiple MPI Applications on a Node

Multiple applications can be run simultaneously on a node by launching several mpirun commands and backgrounding them. For performance, it will likely be necessary to ensure that each application runs on a distinct set of CPU resources.

```bash
#!/bin/bash

###### Tells PBS the job name
#PBS -N packing_example
###### Tells PBS to run on 1 node with 128 cpus
#PBS -l select=1:ncpus=128:mpiprocs=128
###### Tells PBS the walltime (Max 72 hours)
#PBS -l walltime=00:05:00
###### Tells PBS the project to charge (Replace with a valid project)
#PBS -A <PROJECT_NAME>

cd $PBS_O_WORKDIR

export range=$(eval echo {0..31} | xargs | sed -e 's/ /,/g’)
mpirun –map-by pe-list:${range}:ordered –np 32 a.out &

export range=$(eval echo {32..95} | xargs | sed -e 's/ /,/g’)
mpirun –map-by pe-list:${range}:ordered –np 64 b.out &

export range=$(eval echo {96..111} | xargs | sed -e 's/ /,/g’)
mpirun –map-by pe-list:${range}:ordered –np 16 c.out &

wait
```

* Put each MPI tasks in the background with "&".
* Use mpirun placement options to specifically place them on certain cores, to avoid oversubscription on some cores. A good practice would be to run each command in its own directory.
* Add `–report-bindings` to see what is happening.
* You **must** use `wait` command.

## Troubleshooting / Common Errors

If you receive a `qsub: Job rejected by all possible destinations error`, then check your submission parameters. The issue is most likely that your walltime or node count do not fall within the ranges listed above for the production execution queues. Please see the table above for limits on production queue job sizes.

NOTE: For batch submissions, if the parameters within your submission script do not meet the parameters of any of the above queues you might not receive the “Job submission” error on the command line at all. This can happen because your job is in waiting in a routing queue and has not yet reached the execution queues. In this case you will receive a jobid back and qsub will exit, however when the proposed job is routed, it will be rejected from the execution queues. In that case, the job will be deleted from the system and will not show up in the job history for that system. If you run a `qstat` on the jobid, it will return `qstat: Unknown Job Id <jobid>`.

## Documentation and Tools

* The [PBS “BigBook”](https://help.altair.com/2022.1.0/PBS%20Professional/PBS2022.1.pdf): This is really excellent. We highly suggest you download it and search through it when you have questions. However, it is big at about 2000 pages / 40MB and contains a bunch of stuff you don’t really need, so you can also download the guides separately here:
* The [PBS User Guide](https://help.altair.com/2022.1.0/PBS%20Professional/PBSUserGuide2022.1.pdf): This is the user guide.
* The [PBS Reference Guide](https://help.altair.com/2022.1.0/PBS%20Professional/PBSReferenceGuide2022.1.pdf): This is the Reference Guide. It shows every option and gives you details on how to format various elements on the command line.
