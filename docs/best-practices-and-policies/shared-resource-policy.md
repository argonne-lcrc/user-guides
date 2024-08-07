# Shared Resource Policy

## Use of Login Nodes

LCRC login nodes are primarily for managing files in the project and home directories, editing files, submitting/monitoring jobs in the queue and compiling short codes/applications. Please do not run jobs on any LCRC login nodes. Not only do they slow down the system for other users, but can also bring down the login node. Please use the cluster compute queues for testing and running production jobs and compiling large codes. The queues can be used in both interactive mode and batch mode.

Login nodes are also set to throttle user processes that exceed certain CPU or memory thresholds automatically via cgroups. For code compilation requiring multiple CPUs or large amounts of memory, it is recommended to use a compute node. Refer to each cluster's hardware overview for more details.

## Use of Debugging Queues

Some LCRC clusters have debug queues. These queues are to be used only for debugging applications, testing the scaling/timing of a code/problem or checking to see if a new problem has been correctly set-up (proper input parameters, I/O file names etc). This queue is also extensively used by the LCRC staff to test new capabilities/codes, system performance, and address user issues. Debug queues on LCRC cluster are nodes with a maximum time-limit of 1 hour and often can be shared my multiple users. Please do not run hour-long production jobs on this queue. It is also recommended that users limit the maximum job size to 4 nodes whenever possible.
