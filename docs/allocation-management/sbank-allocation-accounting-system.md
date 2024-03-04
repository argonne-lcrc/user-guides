# sbank Allocation Accounting System (PBS Clusters)

sbank is the new accounting system within LCRC, starting with Improv. It tracks project allocations, usage charges, and refunds. sbank allows queries about the balance and expiration of project allocations, and has replaced the outdated lcrc-sbank accounting system.

The sbank accounting system helps users manage their allocations and usage per job. It gives the PIs the ability to monitor their allocation usage by user, job, and machine. It also allows the user to monitor their usage per allocation and provides insight on how many hours are left on the project.

## Getting Started with sbank

[sbank Example Commands](not_in_nav/sbank-examples.md) provides a set of example commands on how to use the most common commands.

## sbank Man Pages

Use these sbank man pages to get information on how to use the commands.

- [sbank](not_in_nav/sbank-manpage.md)
- [sbank-detail](not_in_nav/sbank-detail.md)
- [sbank-detail-allocations](not_in_nav/sbank-detail-allocations.md)
- [sbank-detail-jobs](not_in_nav/sbank-detail-jobs.md)
- [sbank-detail-projects](not_in_nav/sbank-detail-projects.md)
- [sbank-detail-transactions](not_in_nav/sbank-detail-transactions.md)
- [sbank-detail-users](not_in_nav/sbank-detail-users.md)
- [sbank-list](not_in_nav/sbank-list.md)
- [sbank-list-allocations](not_in_nav/sbank-list-allocations.md)
- [sbank-list-jobs](not_in_nav/sbank-list-jobs.md)
- [sbank-list-projects](not_in_nav/sbank-list-projects.md)
- [sbank-list-transactions](not_in_nav/sbank-list-transactions.md)
- [sbank-list-users](not_in_nav/sbank-list-users.md)

## PBS/Slurm Conversion Chart

If you are coming to PBS from Slurm, we have added a basic conversion chart for your general commands and submit scripts that you can reference that provide similar functions.

| Description | PBS Pro | Slurm |
| -------------- | ----------- | --------- |
| Submit Job | qsub [job_script] | sbatch [job_script] |
| Query Jobs | qstat | squeue |
| Delete Job | qdel [job_id] | scancel [job_id] |
| Hold User Job | qhold [job_id] | scontrol hold [job_id] |
| Release User Job | qrls [job_id] | scontrol release [job_id] |
| List Nodes | pbsnodes -a | scontrol show nodes |

| Description | PBS Pro | Slurm |
| -------------- | ----------- | --------- |
| Submission Directive | #PBS | #SBATCH |
| Queue/Partition Selection | -q [queue_name] | -p [queue_name] |
| Number of Nodes | -l select=[count] | -N [count] |
| Number of CPUS per Node | -l ncpus=[count] | -ntasks-per-node=[count] |
| Charge Account | -A [project_name] | -account=[project_name] |
| Walltime | -l walltime=[hh:mm:ss] | -time=[hh:mm:ss] |
| Job Name | -N [name] | -job-name=[name] |
| Standard Out | -o [file_name] | -o [file_name] |
| Standard Error | -e [file_name] | -e [file_name] |
| Email Options | -m abe | -mail-type=[flags] |
| Email Address | -M [email_address] | -mail-user=[email_address] |

Useful variables to use in your scripts:

| Description | PBS Pro | Slurm |
| -------------- | ----------- | --------- |
| Submission Directory | $PBS_O_WORKDIR | $SLURM_SUBMIT_DIR |
| Submit Host | $PBS_O_HOST | $SLURM_SUBMIT_HOST |
| Job ID Number | $PBS_JOBID | $SLURM_JOBID |
| Job Node List | $PBS_NODEFILE | $SLURM_JOB_NODELIST |
| Job Array Index | $PBS_ARRAYID | $SLURM_ARRAY_TASK_ID |
