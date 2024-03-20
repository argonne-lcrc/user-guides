# Data Storage

## Overview

- Users have access to a global home, project, and group space.
- Files created on one cluster can be used across all clusters.
- All storage systems use GPFS as the file system.
- Certain filesystems are backed up nightly.

## Backup Storage

### Disk Drives

- Total Number of Drives Across All JBODs: 612 (6 JBODs x 102 Drives each)
- Drive Type: WDC HC530 SAS Enterprise Disk Drives
- Drive Capacity: 14TB each
- Uses ZFS for its the filesystem

**Maintenance and Data Integrity**:

- Automated monthly scrubs are scheduled for each zpool. These scrubs are essential for maintaining data integrity and for early identification of potential issues.

### Tape Storage

- LCRC has roughly 760 tapes to which we backup and replicate data from the disk storage.
- We are currently running LTO9 tape technology.

## Purchasing Resources

We offer the ability for groups to purchase their own storage resources to be hosted with us. In doing so you get access to the storage space across all of our clusters (unless otherwise specified), and we take care of supporting the system, replacing parts, and when possible tuning the storage resources to fit your data model.

If you are interested in learning more about purchasing additional storage resources please contact us at <support@lcrc.anl.gov>

You also have access to scratch space on the compute nodes while you have a job running on the node.
