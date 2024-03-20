# OpenMC

OpenMC uses both distributed-memory (MPI) and shared-memory (OpenMP) parallelism to maximize computational efficiency. Optimal performance is achieved by aligning parallelization strategies with hardware architecture, such as using one MPI process per NUMA node on servers with large core counts or employing hardware threading.

## Using OpenMC on Improv

On Improv, OpenMC was installed in a Conda environment. The envionment can be loaded and unloaded with the following commands respectively (for these examples, I'm using `openmc/0.14.0`):

```bash
module load openmc/0.14.0
module unload openmc/0.14.0
```

Below is a sample PBS script that can be used to run OpenMC on an Improv node with optimal performance.

* The PBS directive `-l select=1:ncpus=128:mpiprocs=8:ompthreads=16` requests a single node (`select=1`) with 128 CPUs, where 8 MPI processes (`mpiprocs=8`) are to be launched. Each MPI process is configured to use 16 OpenMP threads (`ompthreads=16`).
* When the script executes the `mpiexec` command, it launches 8 separate MPI processes (`-np 8`). The `--bind-to numa` and `--map-by numa` options instruct OpenMPI to bind each process to a NUMA node and map the processes by NUMA node.

```bash
#!/bin/bash

#PBS -N <JOB_NAME>
#PBS -l select=1:ncpus=128:mpiprocs=8:ompthreads=16
#PBS -l walltime=6:00:00
#PBS -j oe
#PBS -A <PROJECT_ALLOCATION>
#PBS -o output_<JOB_NAME>
#PBS -e error_<JOB_NAME>

module load openmc/0.14.0

# Set the necessary OpenMC variables and paths. For example:
export OPENMC_CROSS_SECTIONS=<PATH_TO_CROSS_SECTIONS>
export OPENMC_DEPLETE_CHAIN=<PATH_TO_DEPLETE_CHAIN>

# Set the number of OpenMP threads
export OMP_NUM_THREADS=16

# Change to the directory from which the job was submitted
cd $PBS_O_WORKDIR

# Launch mpiexec with proper NUMA binding and process mapping. Replace the python script name with your script.
mpiexec -np 8 --bind-to numa --map-by numa python <YOUR_SCRIPT_NAME>.py
```
