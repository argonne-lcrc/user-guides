# Allocations on LCRC Computing Resources

## Overview of LCRC Clusters

The LCRC operates three distinct clusters, each with its own scheduling and allocation systems:

1. [**Improv**](../improv/getting-started-improv.md): Operates with the [PBS Pro](../running-jobs-at-lcrc/pbs-pro-clusters.md) job scheduler and measures allocations in **node hours**.
2. [**Bebop**](../bebop/getting-started-bebop.md): Utilizes the [Slurm](../running-jobs-at-lcrc/slurm-clusters.md) job scheduler and measures allocations in **core hours**.
3. [**Swing**](../swing/getting-started-swing.md): Also uses Slurm but charges allocations in **GPU hours**.

## Allocations Metrics for Each Cluster

### GPU Hours (Swing Cluster)

Unlike other LCRC clusters, Swing charges time based on GPU hours instead of CPU core hours. You need to factor this in when applying for time on Swing.

On Swing, the compute nodes charge as follows for each job:

`GPU Hours = Number of Nodes Used x GPUs Per Node x Time in Hours`

- **Number of Nodes Used**: The number of compute nodes employed for the job.
- **GPUs per Node**: How many GPUs used in each node.
- **Time in Hours**: The duration for which these nodes and GPUs are used.

### Core Hours (Bebop Cluster)

`Core Hours = Number of Nodes Used x Cores per Node x Time in Hours`

- **Number of Nodes Used**: How many separate computing nodes are being used.
- **Cores per Node**: Number of CPU cores in each node.
- **Time in Hours**: Duration for which these nodes (and therefore the cores within them) are used.

### Node Hours (Improv Cluster)

Allocations on Improv are provided (and should be requested) in Node Hours. 1 node on Improv has 128 CPU Cores. When requesting or viewing your allocation(s), please take this into consideration. Balances, transactions and other sbank details displayed from sbank commands will update every 5 minutes.

`Node Hours = Number of Nodes Used × Time in Hours`

- **Number of Nodes Used**: The quantity of compute nodes utilized for the job.
- **Time in Hours**: The duration for which these nodes are used.

## Mid-Quarter Allocations

### Overview

Mid-Quarter Allocations by the LCRC are detailed here. Principal Investigators (PIs) need to understand the guidelines and requirements for requesting additional computational resources.

### Allocation Planning and Timing

- PIs should ensure their allocated hours last the entire quarter.
- LCRC reviews mid-quarter allocation requests once a week.

### How to Request Additional Allocations

1. **Check Current Usage**: Verify usage of allocated resources using `lcrc-usage <project_name>`.
2. **Prepare Documentation**: Justify core-hour requirements, guided by the [Sample Project Request PDF](https://accounts.lcrc.anl.gov/sample_project_request.pdf).
3. **Special Criteria for Large Requests**: Provide scaling results for requests exceeding 100K core-hours (781 node hours).

> **Note**: Requests with incomplete or unclear information may result in a delay of up to two weeks in the decision-making process.

### Allocation Limits and Calculation Examples

- Maximum request: allocation one can request is either **250K core-hours** or **half of the initial allocation**, whichever is less.
- This amount is pro-rated based on the remaining time in the quarter.

**Examples**

1. **Initial Allocation of 600K Core-Hours**: If you request an additional 500K core-hours 6 weeks into a 12-week quarter, you may be granted up to **125K additional core-hours**. This is calculated as half of the maximum requestable amount, since half the quarter has passed (`250K * 6/12`).

2. **Initial Allocation of 200K Core-Hours**: If you request an additional 300K core-hours after 7 weeks into the quarter, the maximum you may receive is approximately **58K additional core-hours**. This is calculated by taking half of the initial allocation, prorated for the remaining 5 weeks of the quarter (`200K * 0.5 * 5/12`).

### Other Important Notes

- **No allocations** are made within **two weeks before the quarter's end**
- Project renewals are at the **start of the fiscal year**.
- Failing to renew before the allocation deadline results in postponement to the **following quarter (Q2 onwards)**.
- For large core-hour needs or high usage rates, consider applying through the [Director's Discretionary Allocation Program](https://www.alcf.anl.gov/science/directors-discretionary-allocation-program).

## Startup Projects and Allocations

### Startup Projects Overview

New Argonne employee accounts receive a "startup project" with 20,000 core-hours on the **Bebop** and **Swing** systems.

### Purpose

The startup project serves multiple functions:

- **Training Ground**: Familiarize yourself with system operations.
- **Idea Incubator**: Develop and test new project concepts.
- **User Discretion**: Use the hours for any purpose you see fit.

### Allocation Details

- **Initial Allocation**: 20,000 core-hours.
- **Expiration**: None. The 20,000 hours do not have a use-by date.
- **Additional Time**: Startup projects are not eligible for additional allocations beyond the initial 20,000 hours.

### Usage Guidelines

- **Exclusive Usage**: The startup project should only be used by the account originally associated with it.
- **Transition to Full Project**: If your needs exceed 20,000 hours, you should either apply for your own dedicated project or join an existing one.
  
### Special Note for Non-Argonne Employees

Non-Argonne staff must be part of another active project led by an Argonne PI.

### Slurm Default Account

Your startup project is set as your default project in the Slurm job scheduler. To submit jobs against your startup allocation, use the following account name format: `_startup-<username>_`.

## Allocation Usage and Tracking

### General Policies

- **System Error**: If a system error occurs that causes a program to crash while it is running, a project won’t be charged for that time. (This policy may be amended in the future in order to promote the use of user-based checkpointing.) The scheduler may or may not deduct the time used from the project’s allocation, depending on how the crash took place. If someone thinks their project should be credited time because of a system crash or other system problem, they should send email to support@lcrc.anl.gov to get that time back into the project.
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
- **Post-October 1st**: After October 1st, existing projects can only request time for the current quarter.
