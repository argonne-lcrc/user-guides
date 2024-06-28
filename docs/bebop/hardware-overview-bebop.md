# Bebop Hardware Overview

Bebop has 1,024 compute nodes with Intel Broadwell. The Broadwell nodes have DDR4 memory, and 128 nodes have a 4TB HDD for scratch disk. The high-performance interconnect is Intel Omni-Path (OPA) 100G. There are 12 OPA connections to LCRC’s existing data storage system so you will have access to all of the same files between LCRC clusters.

## Bebop Compute Nodes

672 Broadwell Nodes

| Bebop Broadwell | Description | Per Node | Aggregate |
| -------------- | ----------- | -------- | --------- |
| Processor | Intel Xeon E5-2695v4 | 2 Sockets | 1,344 |
| Cores/Threads | 18 Cores/1 Thread per core | 36 | 24,192/24,192 |
| Memory | DDR4 | 128 GiB | 86,016 GiB |
| Local SSD | 4 TB (Nodes bdwd-[0001-0064]) | 1 | 256 TB |

## Bebop Login Nodes

There are four login nodes available to users for editing code, building code, submitting/monitoring jobs, checking usage (`sbank`), etc. Their full hostnames are `beboploginN.lcrc.anl.gov` for `N` equal to `1` through `6`. The login nodes hardware is identical to the compute nodes. The various compilers and libraries are present on the logins, so most users should be able to build their code. All users share the same login nodes so please be courteous and respectful of your fellow users. For example, please do not run computationally or IO intensive pre- or post-processing on the logins and keep the parallelism of your builds to a reasonable level.
