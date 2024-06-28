# Allocations on LCRC Computing Resources

## Overview of LCRC Clusters

The LCRC operates three distinct clusters, each with its own scheduling and allocation systems:

1. [**Improv**](../improv/getting-started-improv.md): Operates with the [PBS Pro](../running-jobs-at-lcrc/pbs-pro-clusters.md) job scheduler and measures allocations in **node hours**.
2. [**Bebop**](../bebop/getting-started-bebop.md): Utilizes the [PBS Pro](../running-jobs-at-lcrc/pbs-pro-clusters.md) job scheduler and measures allocations in **node hours**.
3. [**Swing**](../swing/getting-started-swing.md): Also uses Slurm but charges allocations in **GPU hours**.

## Allocations Metrics for Each Cluster

### GPU Hours (Swing Cluster)

Unlike other LCRC clusters, Swing charges time based on GPU hours instead of CPU core hours. You need to factor this in when applying for time on Swing.

On Swing, the compute nodes charge as follows for each job:

`GPU Hours = Number of Nodes Used x GPUs Per Node x Time in Hours`

- **Number of Nodes Used**: The number of compute nodes employed for the job.
- **GPUs per Node**: How many GPUs used in each node.
- **Time in Hours**: The duration for which these nodes and GPUs are used.

### Node Hours (Improv and Bebop Clusters)

Allocations on Improv and Bebop are provided (and should be requested) in Node Hours. 1 node on Improv has 128 CPU Cores, and 1 node on Bebop has 32 CPU Cores. When requesting or viewing your allocation(s), please take this into consideration. Balances, transactions and other sbank details displayed from sbank commands will update every 5 minutes.

`Node Hours = Number of Nodes Used × Time in Hours`

- **Number of Nodes Used**: The quantity of compute nodes utilized for the job.
- **Time in Hours**: The duration for which these nodes are used.

Improv and Bebop currently allow for an *Overburn* of a project allocation. Each allocation is allowed to use 10% over the granted allocation before getting an error message. Once the project allocation is exhausted, the following error message will be displayed when submitting a job:

`qsub: Job violates queue and/or server resource limits`

Projects will need to [Request Additional Project Time](https://docs.lcrc.anl.gov/allocation-management/allocations/#requesting-additional-project-time) when allocations are exhausted.

## Startup Projects and Allocations

### Startup Projects Overview

New Argonne employee accounts receive a "startup project" with a small balance of hours on the **Bebop** and **Swing** systems.

### Purpose

The startup project serves multiple functions:

- **Training Ground**: Familiarize yourself with system operations.
- **Idea Incubator**: Develop and test new project concepts.
- **User Discretion**: Use the hours for any purpose you see fit.

### Allocation Details

- **Initial Allocation**: 20,000 core-hours on Bebop, 100 GPU hours on Swing.
- **Expiration**: None. The startup hours do not have a use-by date.
- **Additional Time**: Startup projects are not eligible for additional allocations beyond the initial hours.

### Usage Guidelines

- **Exclusive Usage**: The startup project should only be used by the account originally associated with it.
- **Transition to Full Project**: If your needs exceed the startup hours, you should either apply for your own dedicated project or join an existing one.
  
### Special Note for Non-Argonne Employees

Non-Argonne staff must be part of another active project led by an Argonne PI.

### Slurm Default Account

Your startup project is set as your default project in the Slurm job scheduler. To submit jobs against your startup allocation, use the following account name format: `startup-<username>`.

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
- Additional time can be granted up to 150K core-hours directly by the LCRC core team.
- Requests exceeding 150K core-hours require approval from the LCRC allocations committee.

### Expiration of Additional Time

PIs should note that any extra time granted but not used within the quarter will expire at the quarter's end. For example, if a PI is allocated an extra 100K core-hours and only utilizes 40K, the remaining 60K core-hours will be forfeited at the quarter's conclusion.

### Annual Time Requests for Projects

- **Existing Projects**: Time requests for the upcoming fiscal year can be made from the first week of September until October 1st.
- **New Projects**: New projects may request time throughout the year for the remaining quarters.
- **Post-October 1st**: After October 1st, existing projects can request time for any quarter, but there may be restrictions on the maximum hours granted.

## Mid-Quarter Allocations

Mid-Quarter Allocations can be granted for LCRC projects after not previously requesting time or exhausting a current allocation. Principal Investigators (PIs) need to understand the guidelines and requirements for requesting additional computational resources. We have described, in detail, this process on the [Managing Projects](../../account-project-management/project-management/#mid-quarter-allocations) documentation. 
