# Data Storage

## Overview

- Users have access to a global home, project, and group space.
- Files created on one cluster can be used across all clusters.
- All storage systems use GPFS as the file system.
- Certain filesystems are backed up nightly.

## Disk Storage

### GPFS

- FS1
- FS2
- FS3
- FS4

- **Informative ZFS storage information**

## Tape Storage

LCRC operates a 700. We are currently running a combination of LTO6 and LTO8 tape technology. The LTO tape drives have built-in hardware compression which typically achieve compression ratios between 1.25:1 and 2:1 depending on the data yielding an effective capacity of approximately 65PB.

## Purchasing Resources

We offer the ability for groups to purchase their own storage resources to be hosted with us. In doing so you get access to the storage space across all of our clusters (unless otherwise specified), and we take care of supporting the system, replacing parts, and when possible tuning the storage resources to fit your data model.

If you are interested in learning more about purchasing additional storage resources please contact us at <support@lcrc.anl.gov>

You also have access to scratch space on the compute nodes while you have a job running on the node.
