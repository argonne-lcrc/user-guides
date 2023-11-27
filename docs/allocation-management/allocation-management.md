# Managing Your Allocations

## Overview of LCRC Clusters

The LCRC operates three distinct clusters, each with its own scheduling and allocation systems:

1. **Improv**: Operates with the PBS Pro job scheduler and measures allocations in **node hours**.
2. **Bebop**: Utilizes the Slurm scheduler and measures allocations in **core hours**.
3. **Swing**: Employs the Slurm scheduler but measures allocations in **GPU hours**.

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

#### Examples

1. If your initial allocation was **600K core-hours** or **4688 node hours**  and you request an additional **500K** 6 weeks into the quarter, you may be granted up to **125K additional core-hours** (`250K * 6/12`).
2. If your initial request was **200K core-hours** and you request **300K** after 7 weeks, the maximum you may receive is approximately **58K additional core-hours** (`200K * 0.5 * 5/12`).

### Other Important Notes

- **No allocations** are made within **two weeks before the quarter's end**
- Project renewals are at the **start of the fiscal year**.
- Failing to renew before the allocation deadline results in postponement to the **following quarter (Q2 onwards)**.
- For large core-hour needs or high usage rates, consider applying through the [Director's Discretionary Allocation Program](https://www.alcf.anl.gov/science/directors-discretionary-allocation-program).

## Startup Projects and Allocations

### Overview

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

Requesting Additional Project Time

Occasionally, some LCRC projects might use up their quarterly time allocation well before the start of their next quarterly allocation. This can happen if the computations require more time than anticipated or if more case studies need to be completed. Under such circumstances, the project PIs can request additional time with proper justification at the LCRC Accounts page via the project management screens.

The LCRC core team meets every Tuesday and reviews all time requests for both new and existing projects. The LCRC core team can grant additional time up to 150K core-hours with requests above this needing approval from the LCRC allocations committee. PIs are reminded that any unused additional time will also expire at the end of the quarter. For instance, if a PI requested 100K core-hours for the first quarter and used only 40K core-hours, 60K core-hours would be lost at the end of the quarter.

The LCRC Account page is also used for requesting time for the entire next fiscal year after the first week of September until October 1st for existing projects. New projects will always have the full year to request time for the remaining quarters. After October 1st, existing projects can only request time for the current quarter.
Allocations Note
Swing, unlike other LCRC clusters, charges allocation time based on GPU Hours instead of Core Hours. Please factor this in when applying for time on Swing.Please see GPU Hour Usage for more details.

