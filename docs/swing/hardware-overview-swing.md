# Swing Hardware Overview

8x NVIDIA A100 GPUS per node
1-2TB DDR4 and 320-640GB GPU memory per node
128 cpu cores per compute node
Infiniband HDR Interconnect

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

There are two login nodes available to users for editing code, building code, submitting/monitoring jobs, checking usage (`sbank`), etc. Their full hostnames are `gpulogin1.lcrc.anl.gov` and `gpulogin2.lcrc.anl.gov`.  The login nodes hardware is quite different to the compute nodes. The various compilers and libraries are present on the logins, so most users should be able to build their code. However, software requiring GPUs should be built on the compute nodes as the login nodes do not have GPUs installed in them. All users share the same login nodes so please be courteous and respectful of your fellow users. For example, please do not run computationally or IO intensive pre or post-processing on the logins and keep the parallelism of your builds to a reasonable level.
