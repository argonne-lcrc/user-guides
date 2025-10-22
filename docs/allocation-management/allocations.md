# Allocations on LCRC Computing Resources

## Overview of LCRC Clusters

The LCRC operates three distinct clusters, all of which measure allocations in node hours.
For an overview on our general guidelines on allocation requests and policies, please visit [this link](https://docs.lcrc.anl.gov/best-practices-and-policies/allocation-request-policy/).

## Allocations Metrics for Each Cluster

### Node Hours

#### Improv and Bebop Clusters

Allocations on Improv and Bebop are provided (and should be requested) in Node Hours. 1 node on Improv has 128 CPU Cores, and 1 node on Bebop has 36 CPU Cores. When requesting or viewing your allocation(s), please take this into consideration. Balances, transactions and other sbank details displayed from sbank commands will update every 5 minutes.

`Node Hours = Number of Nodes Used × Time in Hours`

- **Number of Nodes Used**: The quantity of compute nodes utilized for the job.
- **Time in Hours**: The duration for which these nodes are used.

#### Swing Cluster

Allocations on Swing are provided in Node Hours. However, on Swing, 1 Node Hour equates to 8 GPU Hours.

## Allocation Usage and Tracking

### General Policies

- **System Error**: If a system error occurs that causes a program to crash while it is running, a project won’t be charged for that time. (This policy may be amended in the future in order to promote the use of user-based checkpointing.) The scheduler may or may not deduct the time used from the project’s allocation, depending on how the crash took place. If someone thinks their project should be credited time because of a system crash or other system problem, they should send email to <support@lcrc.anl.gov> to get that time back into the project.
- **Run Continuation**:If a project runs out of allocation time during a run, that run will be allowed to continue to completion.
- **Project Suspension**: If a project runs out of allocation time, all users on the project will not be able to run any jobs until time is added again. At the present time, no steps will be taken to stop any jobs associated with that project from running and sitting idle in the job queue.
- **Usage Tracking**

## Requesting Additional Project Time

### When to Request Additional Time

Projects that depleted their quarterly allocation before the end of the quarter should use the [backfill queue](https://docs.lcrc.anl.gov/account-project-management/project-management/#mid-quarter-allocations).

### Annual Time Requests for Projects

- **Existing Projects**: Time requests for the upcoming fiscal year can be made from the first week of September until October 1st.
- **New Projects**: New projects may request time throughout the year for the remaining quarters.
- **Post-October 1st**: After October 1st, existing projects can request time for any quarter, but there may be restrictions on the maximum hours granted.

## Mid-Quarter Allocations

Projects that have exhausted their initial quarterly allocations should use the backfill queue till the end of the quarter. Allocation requests submitted between the start of the quarter and the end of the 10th week of the quarter will also have to use the backfill queue. They will be considered for full allocation in the next quarter. Allocation requests submitted after the 10th week of a quarter will have to run in the backfill queue in the next quarter as well.

All allocation related tickets are reviewed and responded to once a week on Tuesday.