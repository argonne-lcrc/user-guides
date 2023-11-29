# Running Jobs on Improv

## Queues

Use the `-q` option with `qsub` to select a queue. The default queue is compute. We allow up to 15 jobs per user to run at the same time while 100 total jobs can be queued to run.

| Improv Queue Name | Description | Number of Nodes | CPU Type | Cores Per Node	| Memory Per Node | Local Scratch Disk | Max Walltime |
|-------------------|-------------|-----------------|----------|----------------|-----------------|--------------------|--------------|
| compute | Standard Compute Nodes | 737 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 960GB | 72 Hours (3 Days) |
| bigmem | Large Memory Compute Nodes | 12 | 2x AMD EPYC 7713 64-Core Processor | 128 | 1TB DDR4 | 6TB | 72 Hours (3 Days) |
| bigdata | Large Scratch Compute Nodes | 68 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 6TB | 72 Hours (3 Days) |
| debug | Reduced Walltime Compute Nodes | 8 | 2x AMD EPYC 7713 64-Core Processor | 128 | 256GB DDR4 | 960GB | 1 Hour |
