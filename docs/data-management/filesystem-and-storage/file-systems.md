# File Systems

## Home, Project, and Group Disks

Your home, project, and group directories are located on separate GPFS filesystems that are shared by all nodes on the cluster. These filesystems are located on a raid array and are served by multiple file servers. This provides both a performance increase and protection against the filesystems being inaccessible. If one server goes down, the other servers can continue to serve the filesystems.

### Pros

- Global namespace
- Multi-TB filesystem
- Large file support (> 2GB)
- Backed up
- Raid protection
- Stable hardware
- Native InfiniBand support

### Cons

- Moderate performance

## Local and Global Scratch Disks

### Local

If you need a place to put temporary files that don’t need to be accessed by other nodes, we recommend that you put them into the local scratch disk on the nodes during job runs. All jobs create a job specific directory with local storage which can be referenced from your job submission script using the variable `$TMPDIR`. The normal publicly available nodes offer 15 GB of temporary scratch space while the ‘biggpu’ queue offers 1 TB. Diskfull Bebop nodes also house a 4TB disk on each node. Note that these spaces aren’t backed up and the space will be cleared out on job end!

#### Local Scratch Disk Pros

- Fast access times
- Large file support (> 2GB)

#### Local Scratch Disk Cons

- Unique to each node; not shared between nodes
- GB filesystem
- Not backed up
- Cleared out at the end of your job
- No raid protection

### Global

LCRC also has a global scratch space located at /lcrc/globalscratch. This space is a GPFS filesystem that is several TBs in size (the size may change over time). This space is shared to all LCRC nodes, unlike the local scratch space. Because this is a GPFS filesystem, IO will not be as fast.

NOTE: This global scratch space may be cleaned up during our maintenance days or at other intervals and all files deleted in this space with or without notice. This space is not intended for long term storage and is not backed up in any way. Files deleted either accidentally or on purpose are permanently deleted.

#### Global Scratch Disk Pros

- Shared between nodes
- Very large capacity

#### Global Scratch Disk Cons

- Slower access time
- Not backed up
- Cleared out with or without notice
- No raid protection
