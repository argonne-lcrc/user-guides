# Gaussian 

Gaussian uses Linda rather than MPI to communicate between nodes. You need to [pass directives to Gaussian](http://gaussian.com/equivs/) to specify which nodes to use and how many processes to use on each node.  The simplest way to do this is to start a Linda worker on each node and spawn a number of threads equal to the number of cores on the node. 

## Using Gaussian on Improv

Gaussian can be loaded and unloaded with the following commands respectively (for these examples, I'm using `gaussian/16.C.02`):

```bash
module load gaussian/16.C.02
module unload gaussian/16.C.02
```

It is extremely important that you define the environmental variable GAUSS_SCRDIR to be the local disk on the compute node (/scratch).  Otherwise, Gaussian will use the working directory on a shared file system as scratch space. This can slow down the LCRC servers.

Below are sample PBS script that can be used to run Gaussian on Improv node(s) with optimal performance. The first example will run on 2 nodes for larger calculations that can scale to a full node of Improv.

```bash
#!/bin/bash
#PBS -N <JOB_NAME>
#PBS -l select=2:ncpus=128:mpiprocs=1:ompthreads=128
#PBS -A <PROJECT_ALLOCATION>
#PBS -l walltime=02:00:00

module load gaussian/16.C.02

cd $PBS_O_WORKDIR
np=`wc -l < $PBS_NODEFILE`
nn=`sort -u $PBS_NODEFILE | wc -l`
node_list=`sort -u $PBS_NODEFILE| paste -d, -s`

echo " np= " $np
echo " nn= " $nn
echo " node_list= " $node_list

export GAUSS_CDEF=0-127
export GAUSS_MDEF=200GB
export GAUSS_WDEF=$node_list
export GAUSS_SCRDIR=/scratch/test
mkdir $GAUSS_SCRDIR
export GAUSS_SDEF=ssh
export GAUSS_LFLAGS="-vv"
g16 Grubbs2
rm -rf $GAUSS_SCRDIR
```

The next example will pack 4 Gaussian jobs onto 1 node to utilize all 128 cores when the calculation does not scale as well. We do this to not waste cores that would otherwise sit idle.

The PBS directive `-l select=1:ncpus=128:mpiprocs=4:ompthreads=32` requests a single node (`select=1`) with 128 CPUs, where 4 MPI processes (`mpiprocs=4`) are to be launched. Each MPI process is configured to use 32 OpenMP threads (`ompthreads=32`).

```bash
#!/bin/bash

#PBS -N <JOB_NAME>
#PBS -l select=1:ncpus=128:mpiprocs=4:ompthreads=32
#PBS -A <PROJECT_ALLOCATION>
#PBS -l walltime=02:00:00

module load gaussian/16.C.02

#This script will run four gaussian jobs
#on 32 cores of improv simulataneously

#Note that each calculation runs in a unique subfolder
#and has a unique scratch disk.

cd $PBS_O_WORKDIR

for ii in 0 1 2 3; do
	let jj=ii*32
	let kk=jj+31
	let ll=ii+1
        export GAUSS_CDEF=$jj-$kk
        export GAUSS_MDEF=200GB
        export GAUSS_SCRDIR=/scratch/$PBS_JOBID/$ll
        mkdir -p $GAUSS_SCRDIR
	cd test$ll
        (g16 test_$ll
	rm -rf $GAUSS_SCRDIR)&
	cd ..
done
wait
```
