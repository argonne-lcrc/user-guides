# Swing Hardware Overview
User
8x NVIDIA A100 GPUS per node
1-2TB DDR4 and 320-640GB GPU memory per node
128 cpu cores per compute node
Infiniband HDR Interconnect
Swing has 6 compute nodes that contain 8 NVIDIA A100 GPUs each. Each node has

root@gpu1:~# lscpu
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   43 bits physical, 48 bits virtual
CPU(s):                          256
On-line CPU(s) list:             0-255
Thread(s) per core:              2
Core(s) per socket:              64
Socket(s):                       2
NUMA node(s):                    8
Vendor ID:                       AuthenticAMD
CPU family:                      23
Model:                           49
Model name:                      AMD EPYC 7742 64-Core Processor
Stepping:                        0
Frequency boost:                 enabled
CPU MHz:                         3389.384
CPU max MHz:                     2250.0000
CPU min MHz:                     1500.0000
CPU(s):                          256
On-line CPU(s) list:             0-255
Thread(s) per core:              2
Core(s) per socket:              64
Socket(s):                       2
NUMA node(s):                    8
Vendor ID:                       AuthenticAMD
CPU family:                      23
Model:                           49
Model name:                      AMD EPYC 7742 64-Core Processor

gpu1-4,6 - 1 TB of memory DDR4
gpu5 - 2 TB of memory DDR4

gpu5 - 8 x A100 80GB  GPUs
gpu1-4,6 - 8 x A100 40GB GPUs

Improv has 825 dual-socket compute nodes with AMD 7713 64-core processors (2.0 GHz) (or 128 cores per node) and 256 GB DDR4 memory. 68 nodes have 4TB NVMe SSD, and 12 of those have 1024 GB DDR4, instead of 256 GB. The high-performance interconnect is Nvidia/Mellanox HDR200 (14 core, 35 edge switches). There are 12 HDR200 connections to LCRC’s existing data storage system so you will have access to all of the same files between LCRC clusters.

## Swing Compute Nodes

| Swing Compute | Description | Per Node | Aggregate |
| -------------- | ----------- | -------- | --------- |
| Processor (Note 1) | AMD EPYC 7742 2.25GHz | 2 Sockets | 12 |
| Cores/Threads | 64 Cores/2 Threads per core | 128/256 | 768/1,536 |
| Memory | DDR4 | 1 TB (gpu1-4,6)<br>2 TB(gpu5) | 7,000 GiB |
| Local SSD | 14 TB (gpu1-4,6)<br>28 TB (gpu5) | 1 | 98 TB |
| GPUs | NVIDIA A100 80GB (Node gpu5)<br>NVIDIA A100 40GB (Nodes gpu1-4,6) | 8 | 48 |

## Swing A100 GPU Information

| Description | A100-SXM4-80GB | A100-SXM4-40GB |
| ----------- | -------------- | -------------- |
| GPU Memory | 80 GiB HBM2 |
| GPU Memory BW | 2.4 TB/s |
| Interconnect | |
| FP64 | 9.7 TF |
| FP64 Tensor Core | 19.5 TF |
| FP32 | 19.5 TF |
| BF16 Tensor Core | 312 TF |
| FP16 Tensor Core | 312 TF |
| INT8 Tensor Core | 624 TOPS |
| Max TDP Power | 400 W |

## Swing Login Nodes

There are four login nodes available to users for editing code, building code, submitting/monitoring jobs, checking usage (`lcrc-sbank`), etc. Their full hostnames are `gpulogin1.lcrc.anl.gov` and `gpulogin2.lcrc.anl.gov`.  The login nodes hardware is identical to the compute nodes. The various compilers and libraries are present on the logins, so most users should be able to build their code. All users share the same login nodes so please be courteous and respectful of your fellow users. For example, please do not run computationally or IO intensive pre or post-processing on the logins and keep the parallelism of your builds to a reasonable level.
