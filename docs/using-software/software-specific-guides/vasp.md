# VASP 

## Using VASP on Improv

VASP can be loaded and unloaded with the following commands respectively (for these examples, I'm using `vasp/6.4.3`):

```bash
module load vasp/6.4.3
module unload vasp/6.4.3
```
An example script to run VASP on Improv can be found at /soft/software/custom-built/vasp/6.4.3/st/example/vasp.pbs.

An example script to run VASP on Bebop can be found at /soft/software/custom-built/vasp/6.4.3_st/example/vasp.pbs.

The parallel performance of VASP can be optimized with a few INCAR tags.
  
We recommend using NCORE and KPAR in your INCAR file on Improv to improve parallel performance.

NCORE should be set to a divisor of 16 on Improv.  We find that NCORE=8 gives good parallel performance on Improv.
You should test NCORE=2, 4, 8, and 16 for your case to get the optimal performance on Improv.

NCORE should be set to a divisor of 18 on Bebop.  We find that NCORE=6 gives good parallel performance on Bebop.
You should test NCORE=2, 3, 6, 9 and 18 for your case to get the optimal performance on Bebop.

Using NCORE=1 is discouraged except for GW and RPA calculations.

KPAR should be set to a divisor of the number of k-points.                        
Setting KPAR to the number of nodes being used will give the most improvement in parallel performance.

NCORE*KPAR must be a divisor of the number of cores being used.

See the VASP wiki at https://www.vasp.at/wiki/index.php/Category:Parallelization for more details.

Unfortunately, the parallel performance of VASP on will be poor when using more cores than atoms.
For calculations involving multiple k-points the KPAR tag will improve parallel scaling for small cells.

You can run several VASP calculations in parallel in a single batch job to improve the efficiency of small VASP calculations. 
An example script to do this can be found at /soft/software/custom-built/vasp/6.4.2/example/multiple-mpi/test.sh on Improv.
```
