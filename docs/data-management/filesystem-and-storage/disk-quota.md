# Disk Quota Information

## Overview

Quotas are enforced on home and project directories to manage storage resources effectively.

## Home and Project Directory Quotas
- **Home Directories**: Each user is allocated a standard quota of 100 GB.
- **Project Directories**: Projects are allocated a default of 1 TB, with the option to request increases.

## Quota Enforcement
- **Soft Limits**: Users can temporarily exceed their allocated quotas to avoid disrupting running jobs.
- **Grace Period**: Users have a two-week period to manage and reduce their data usage back within quota limits after exceeding the soft limit.
- **Hard Limits**: Persistent excess use beyond the grace period will result in write restrictions, preventing new data from being saved until the usage is reduced.

## Quota Table

| Filesystem | Location                              | Soft Limit | Hard Limit |
|------------|---------------------------------------|------------|------------|
| Home       | `/home/<username>`                    | 100 GB     | 1 TB       |
| Project    | `/lcrc/project/<project_name>`        | 1+ TB      | 2+ TB      |
| Group      | `/lcrc/group/<group_name>`            | No quotas  | No quotas  |

## Checking Your Quota

To view your current quota usage, use the command `lcrc-quota`.

```bash
$ lcrc-quota

----------------------------------------------------------------------------------------
Home                          Current Usage   Space Avail    Quota Limit    Grace Time
----------------------------------------------------------------------------------------
UserX                         113 GB         -13 GB         100 GB         8 days
----------------------------------------------------------------------------------------
Project                       Current Usage   Space Avail    Quota Limit    Grace Time
----------------------------------------------------------------------------------------
ImprovAcceptance                    0 GB        1024 GB        1024 GB
lcrc-app-engr                   11231 GB        9248 GB       20480 GB
mcnp_software                       1 GB        1023 GB        1024 GB
support                          6442 GB        3781 GB       10240 GB
```

## Requesting Additional Storage

**Eligibility**: Only available for project directories.

### Process

1. Visit <https://accounts.lcrc.anl.gov>
2. Under `Owned` projects, select the project requiring more storage.
3. Input the desired storage amount in TB in the `Storage (TB)` field.
4. Provide a justification under `Storage Justification`.
5. Save changes to submit your request for review.

## Over-Quota Management

If your home directory is over quota, consider deleting unnecessary files or moving data to project or group directories.
