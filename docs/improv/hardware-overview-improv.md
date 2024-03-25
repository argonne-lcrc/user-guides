# Improv Hardware Overview

Improv has 825 dual-socket compute nodes with AMD 7713 64-core processors (2.0 GHz) (or 128 cores per node) and 256 GB DDR4 memory. 68 nodes have 6TB NVMe SSD, and 12 of those have 1024 GB DDR4, instead of 256 GB. The high-performance interconnect is Nvidia/Mellanox HDR200 (14 core, 35 edge switches). There are 12 HDR200 connections to LCRC’s existing data storage system so you will have access to all of the same files between LCRC clusters.

## Improv Compute Nodes

| Improv Compute | Description | Per Node | Aggregate |
| -------------- | ----------- | -------- | --------- |
| Processor (Note 1) | 2.0 GHz 7713 | 2 Sockets | 1,650 |
| Cores/Threads | 64 Cores/1 Thread per core | 128 | 105,600/105,600 |
| Memory | DDR4 | 256 GiB (Nodes i001-814)<br>1 TiB (Nodes i815-825) | 219,648 GiB |
| Local SSD | 1 TB (Nodes i001-375, i444-813)<br>5.9 TB (Nodes i376-443, i814-825) | 1 | 1,217 TB |

**Note 1**<br>
*L1d Cache (Level 1 Data Cache): 32 KB*, *L1i Cache (Level 1 Instruction Cache): 32 KB*, *L2 Cache: 512 KB*, *L3 Cache: 32,768 KB (or 32 MB)*

## Improv Login Nodes

There are four login nodes available to users for editing code, building code, submitting/monitoring jobs, checking usage (`sbank`), etc. Their full hostnames are `iloginN.lcrc.anl.gov` for `N` equal to `1` through `4`.  The login nodes hardware is identical to the compute nodes. The various compilers and libraries are present on the logins, so most users should be able to build their code. All users share the same login nodes so please be courteous and respectful of your fellow users. For example, please do not run computationally or IO intensive pre- or post-processing on the logins and keep the parallelism of your builds to a reasonable level.
