# VASP 

## Using VASP on Improv

VASP can be loaded and unloaded with the following commands respectively (for these examples, I'm using `vasp/6.4.2`):

```bash
module load vasp/6.4.2
module unload vasp/6.4.2 
```

Below is a sample PBS script that can be used to run VASP on an Improv node with optimal performance. It will pack 8 VASP jobs onto 1 node to utilize all 128 cores.

* The PBS directive `-l select=1:mpiprocs=128` requests a single node (`select=1`) with 128 CPUs, where 128 MPI processes (`mpiprocs=128`) are to be launched.

```bash
#!/bin/bash

#PBS -N <JOB_NAME>
#PBS -l select=1:mpiprocs=128
#PBS -A <PROJECT_ALLOCATION>
#PBS -l walltime=01:00:00

# Note that VASP was built with openmpi version 4.1.6.
# This method of assigning CPU affinities will not work with openmpi version 5.0.0 and above,
# or MPICH, MVAPICH or Intel-MPI.

module load vasp/6.4.2

cd $PBS_O_WORKDIR

# The following loop with spawn eight VASP jobs.
# Each mpi task spawned by this loop will run on a unique core.
# The output and input for each  VASP job will be in a unique subfolder.

for ii in {1..8}; do
  # Change to unique directory for this job.
  cd $ii
  # Define the first CPU for this jobi.
  let jj=(ii-1)*16
  # Define the last CPU for this job.
  let kk=$jj+15
  # The RANGE variable contains the list of cpus for this job to run on.
  export RANGE=$(eval echo {$jj..$kk} | xargs | sed -e 's/ /,/g')
  # The MPIOPT variable contains the MPIRUN directives defining the CPU affinities.
  export MPIOPT="--bind-to cpu-list:ordered --cpu-list $RANGE"
  # The ampersand at the end of the next command spawns a subprocess.
  # The --report-bindings directive is optional.
  mpirun $MPIOPT --report-bindings -np 16 vasp_gam &
  # Change back to PBS_O_WORKDIR.
  cd ..
done

# Wait for all the subprocesses to complete.
wait
```
