# Disk Quota

## Home and Project Directory Quotas

- **Home Directories**: 100 GB quota.
- **Project Directories**: Default 1 TB quota, with potential for increase upon approval.

## Quota Limits and Grace Period

- **Soft Limits**: Exceeding your quota is initially allowed, preventing disruptions in ongoing tasks.
- **Grace Period**: 2 weeks to reduce usage back within quota limits after exceeding them.
- **Hard Limits**: Exceeding quotas beyond the grace period results in write.

## Checking Your Quota

- To view your current quota usage, use the command `lcrc-quota`.

```bash
$ lcrc-quota

----------------------------------------------------------------------------------------
Home                          Current Usage   Space Avail    Quota Limit    Grace Time
----------------------------------------------------------------------------------------
tlivolsi                          113 GB         -13 GB         100 GB         8 days
----------------------------------------------------------------------------------------
Project                       Current Usage   Space Avail    Quota Limit    Grace Time
----------------------------------------------------------------------------------------
ImprovAcceptance                    0 GB        1024 GB        1024 GB
lcrc-app-engr                   11231 GB        9248 GB       20480 GB
mcnp_software                       1 GB        1023 GB        1024 GB
support                          6442 GB        3781 GB       10240 GB
```

## Requesting Additional Storage

- **Eligibility**: Only available for project directories.

## Process

1. Visit <https://accounts.lcrc.anl.gov>
2. Under `Owned` projects, select the project requiring more storage.
3. Input the desired storage amount in TB in the `Storage (TB)` field.
4. Provide a justification under `Storage Justification`.
5. Save changes to submit your request for review.

## Over-Quota Management

- If your home directory is over quota, consider deleting unnecessary files or moving data to project or group directories.
