# LCRC Data Storage

## Storage Systems Access

On all of our clusters, users have access to a global home, project, and group space (if they belong to a group that has purchased additional storage). This means that files created from compute nodes on one cluster can be used for calculations on any of our other clusters. All storage systems use GPFS as the file system and certain filesystems are backed up nightly.

### Filesystem Quotas

| Filesystem | Location                  | Soft Limit | Hard Limit |
|------------|---------------------------|------------|------------|
| Home       | `/home/<username>`        | 100 GB     | 1 TB       |
| **Project**    | `/lcrc/project/<project_name>` | **1+ TB**      | **2+ TB**       |
| Group      | `/lcrc/group/<group_name>`| no quotas  | no quotas  |

### Additional Storage Options

We also offer the ability for groups to purchase their own storage resources to be hosted with us. In doing so you get access to the storage space across all of our clusters (unless otherwise specified), and we take care of supporting the system, replacing parts, and when possible tuning the storage resources to fit your data model.

Interested in more storage? Contact us at <support@lcrc.anl.gov>.

### Scratch Space

You also have access to scratch space on the compute nodes while you have a job running on the node.
