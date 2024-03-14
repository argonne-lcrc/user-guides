# Job Scheduling Policy

The current LCRC job scheduling policy is pretty flexible, allowing people to submit nearly any kind of job mix into the job queue. We have implemented a policy that prevents more than 32 jobs for any one user from running at one time and we also enforce a 100 job limit per user. In addition, we have a process for handling overdrawn projects and a priority scheduling policy, both of which are described below.

It should also be noted that we have maintenance days on the second Monday of every month. Therefore, if your job requires more time than there is remaining before the maintenance period, your job will be held until after the maintenance period.

## Priority Scheduling

The priority scheduling policy is implemented using Maui queues. Jobs are submitted to a default queue, and Maui then routes the jobs to the appropriate priority queue based on the priority assigned to the job owner. The larger the priority number, the later a job will be run (i.e. a job with priority 1 will be run before a job with priority 2, if possible). Each user is assigned a priority based on their memberships in LCRC projects. If they are a member of multiple projects, they are assigned the largest priority number of the projects.

The CSAC Allocations Committee has assigned a priority to each active LCRC project. If a project goes negative, its priority is set to priority level 5. For example, if project A has been assigned priority 3 by the committee and has a positive balance, jobs of members of project A are assigned priority 3. If project A goes negative, jobs of members of project A will then be assigned priority 5.

Maui schedules jobs FIFO within each priority level. Priority 0 jobs are scheduled FIFO until no more priority 0 jobs will fit, then priority 1 jobs are scheduled FIFO until no more priority 1 jobs will fit, etc. Priority only affects jobs if not all jobs in the queue can be scheduled due to not enough resources. If two jobs of differing priority cannot both fit within existing resources, but either of them will fit, the job with the lower priority number will be run first.

LCRC does not use job preemption so once a job has started, it will run until it is finished, aborts with an error, or is deleted by the owner or an administrator.

## General Scheduling Guidelines

LCRC is expected to support all kinds of different usage. We do not want to unnecessarily limit the kinds of work that could be done on the system. We would prefer to maintain a flexible scheduling policy based on the needs of the community rather than impose a strict policy.

As a consequence of this liberal scheduling policy, there have been instances in the past where people inappropriately loaded up the machine, resulting in delays for everyone else. Unfortunately, as LCRC becomes more heavily used, this requires increased monitoring.

We ask that all users follow these guidelines:

1. Follow good queue etiquette. This means, among other things, don’t submit jobs that use a large number of nodes that run for a long time. If your job uses several hundred nodes, limit the run time to a few hours. Likewise, if your job runs for several hundred hours, limit the number of nodes to less than a dozen or so. We do encourage large jobs and if they need to run for awhile, this is okay. With others using the system, we only ask to you to be mindful of this and to break up runs when possible.
2. Clearly, some work will require use of the machine beyond those bounds. In those cases, please send a quick note to <support@lcrc.anl.gov> so that we can be aware of these. If necessary, we can arrange for a reservation and notify the community.
3. If we get well-founded complaints from other users of the system about your jobs, we will attempt to contact you to determine the best course of action. However, under some circumstances, we may have to kill running jobs. We don’t want to do this, but it has been necessary a few times.
4. We strongly encourage checkpointing. Checkpointing not only allows you to recover from a job that has died unexpectedly, but can also allow you to break a long-running job into smaller chunks that are therefore easier to schedule.

## Process for Overdrawn LCRC Projects

1. The system automatically checks for LCRC projects that have exceeded their balance.
2. The queue priority of overdrawn projects will drop automatically to priority level 5. For details on the scheduling policy, please see the priority scheduling section of this document.
3. Overdrawn projects will be limited to running only one job per user at a time.
4. A member of the LCRC staff will notify overdrawn projects via email (to all Project members) within one business day. We will also invite the PIs to submit a revised allocation request if needed.
5. The PIs may request a change in the distribution of the current project allocation (e.g., some/all of their 2nd half time in the first half). LCRC staff may move up to 100K hours; larger requests need approval by the Allocations Committee.
6. Alternately, the PIs may submit a revised allocation request, asking for more time. Increments of less than 200K hours will be managed by LCRC staff and reported to the Allocations Committee; larger requests need approval by the Allocations Committee.
7. If no request is received, the overdrawn project will be suspended (unable to start new jobs) when its time use exceeds 100% overdrawn or 25K hours overdrawn, whichever comes first. This gives the project a cushion to finish up, to put in their revised request, or to make other arrangements.

Projects are encouraged to contact LCRC staff when their project needs change, particularly when there are special needs. We will make every effort to find ways to meet your research needs. The Allocations Committee meets quarterly.
