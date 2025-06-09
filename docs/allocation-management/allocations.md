# Allocations on LCRC Computing Resources

## Overview of LCRC Clusters

The LCRC operates three distinct clusters, all of which measure allocations in node hours.

## Allocations Metrics for Each Cluster

### Node Hours

We currently allow for an *Overburn* of a project allocation. Each allocation is allowed to use 10% over the granted allocation before getting an error message. Once the project allocation is exhausted, the following error message will be displayed when submitting a job:

`qsub: Job violates queue and/or server resource limits`

Projects will need to [Request Additional Project Time](https://docs.lcrc.anl.gov/allocation-management/allocations.md#requesting-additional-project-time) when allocations are exhausted.

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

Projects at LCRC may deplete their quarterly time allocation sooner than expected due to unforeseen computational demands or additional case studies. In these instances, Principal Investigators (PIs) have the option to request extra time.

### How to Request

PIs can submit requests for additional project time through the LCRC Accounts page by accessing the project management screens. Requests must include proper justification for the additional time needed.

### Review Process

- The LCRC core team convenes every Tuesday to assess time requests for both new and ongoing projects.
- Additional time can be granted up to 5,000 node-hours directly by the LCRC core team.
- Requests exceeding 5,000 node-hours might require approval from the LCRC allocations committee.

### Expiration of Additional Time

PIs should note that any extra time granted but not used within the quarter will expire at the quarter's end. For example, if a PI is allocated an extra 25K node-hours and only utilizes 10K, the remaining 15K node-hours will be forfeited at the quarter's conclusion.

### Annual Time Requests for Projects

- **Existing Projects**: Time requests for the upcoming fiscal year can be made from the first week of September until October 1st.
- **New Projects**: New projects may request time throughout the year for the remaining quarters.
- **Post-October 1st**: After October 1st, existing projects can request time for any quarter, but there may be restrictions on the maximum hours granted.

## Mid-Quarter Allocations

Projects that have exhausted their initial quarterly allocations should use the backfill queue till the end of the quarter. Mid-quarter allocations are granted to new projects or old projects that had not requested time for a particular quarter.  The maximum time that can be granted for such projects is 2000 node-hours on Improv, 7000 node-hours on Bebop and 25 node-hours on Swing, prorated with time remaining in the quarter.  For instance, if a PI requests a new project on June 1st (3rd month of Q3) requesting 10K node hours for Q3 and Q4 on Improv, they will be granted a maximum of 667 node-hours for Q3 (can be lower if the cluster is over-subscribed at the start of Q3).  This allocation of 667 node-hours corresponds to the pro-rated maximum time (2000*1/3).  Once allocated hours for Q3 have been exhausted - they can run in the backfill queue till the end of Q3.  The request of 10K for Q4 will be based on the allocation rules for Q4.  Similar procedure will be applied to old projects where time was not requested for all the quarters at the start of Q1 for that particular allocation year.

All allocation requests are reviewed and responded to once a week on Tuesday.
No allocations are made within two weeks before the end of the quarter.  For large node-hour needs or high usage rates, consider applying through the [Director's Discretionary Allocation Program](https://www.alcf.anl.gov/science/directors-discretionary-allocation-program) in ALCF.