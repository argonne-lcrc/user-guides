# Data Storage

## Overview

- Users have access to a global home, project, and group space.
- Files created on one cluster can be used across all clusters.
- All storage systems use GPFS as the file system.
- Certain filesystems are backed up nightly.

## Disk Storage

### ZFS Backup Storage

**JBODs (Just a Bunch Of Disks)**:

- Total Number of JBODs: 6
- Each JBOD is equipped with 102 x 14TB WDC HC530 SAS Enterprise Disk Drives.
- This configuration ensures that every disk in a zpool is from the same JBOD, maximizing performance.

**Disk Drives**:

- Total Number of Drives Across All JBODs: 612 (6 JBODs x 102 Drives each)
- Drive Type: WDC HC530 SAS Enterprise Disk Drives
- Drive Capacity: 14TB each

**ZFS Pools (zpools)**:

- Total Number of zpools: 18
  - Distribution: 3 zpools per JBOD
  - Configuration of Each zpool:
    - Composed of 3 vdevs (Virtual Devices)
    - Each vdev is a RAIDZ3 configuration with 11 disks
    - One hot spare disk per zpool for redundancy and immediate replacement in case of a disk failure
  - Usable Space per zpool: ~305 TB
  - Total Usable Space: ~5.5 PB

**Maintenance and Data Integrity**:

- Automated monthly scrubs are scheduled for each zpool. These scrubs are essential for maintaining data integrity and for early identification of potential issues.

## Tape Storage

LCRC operates a 700. We are currently running a combination of LTO6 and LTO8 tape technology. The LTO tape drives have built-in hardware compression which typically achieve compression ratios between 1.25:1 and 2:1 depending on the data yielding an effective capacity of approximately 65PB.

## Purchasing Resources

We offer the ability for groups to purchase their own storage resources to be hosted with us. In doing so you get access to the storage space across all of our clusters (unless otherwise specified), and we take care of supporting the system, replacing parts, and when possible tuning the storage resources to fit your data model.

If you are interested in learning more about purchasing additional storage resources please contact us at <support@lcrc.anl.gov>

You also have access to scratch space on the compute nodes while you have a job running on the node.
