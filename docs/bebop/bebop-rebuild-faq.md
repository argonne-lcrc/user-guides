# Bebop Rebuild FAQ

## Overview

On July 1, 2024, Bebop was rebuilt from CentOS 7 to Rocky Linux 8 as CentOS has reached End of Life. This rebuild includes a full Operating System change, a new software tree and a transition from Slurm to PBS Professional as the system job scheduler.

## Logging In

As before, to access Bebop, use the following command:

`ssh <your_argonne_username>@bebop.lcrc.anl.gov`

However, because the login nodes SSH Known Host Keys have changed, you may see something like the following message:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:XXXXXXXXXXXXXXXXXXXXXXXXXXX.
Please contact your system administrator.
Add correct host key in ~/.ssh/known_hosts to get rid of this message.
Offending RSA key in ~/.ssh/known_hosts:13
Password authentication is disabled to avoid man-in-the-middle attacks.
Keyboard-interactive authentication is disabled to avoid man-in-the-middle attacks.
```

This is expected! To bypass this message in the future, please note the line:

`Offending RSA key in ~/.ssh/known_hosts:13`

In this instance, you would delete line 13 from the `~/.ssh/known_hosts` file on your local machine. The line number(s) will be different for everyone. 
You will need to do this for any Bebop login node in this file. You can search this file for any of the following and delete those lines:

- bebop.lcrc.anl.gov
- beboplogin[1-6].lcrc.anl.gov
- 140.221.70.[5-10]

## Submitting and Running Jobs

Bebop is now using the PBS Professional job scheduler which replaces Slurm. With this change, we have removed the following queues:
`bdw`, `bdws`, and `bdwd`. As a reminder, all KNL queues were previously retired in early 2024.

All jobs should now be submitted to the queue: `bdwall`. This is also the default queue. If you still need to submit to Broadwell nodes with a large local scratch disk, we have outlined how to do that here: 

[https://docs.lcrc.anl.gov/bebop/running-jobs-bebop](https://docs.lcrc.anl.gov/bebop/running-jobs-bebop)

For tips on how to use PBS, please see:

[https://docs.lcrc.anl.gov/running-jobs-at-lcrc/pbs-pro-clusters](https://docs.lcrc.anl.gov/running-jobs-at-lcrc/pbs-pro-clusters)

Like our login nodes, all SSH Known Host keys have changed on the Bebop compute nodes. Because of this, you may get an error when trying to submit multi-node or interactive jobs to the queues. We highly recommend **ALL** users run the following after logging into a Bebop login node:

`mv ~/.ssh/known_hosts ~/.ssh/known_hosts.bak`

This will move your current LCRC SSH Known Hosts file to a backup file, and you should no longer have an issue with running a job.
 
## FAQ

### I use "X" software and it is missing, where did it go?

Prior to the Bebop rebuild, LCRC staff sent several e-mails to LCRC users. Users were informed that the previous stack of software would be removed in favor of a new one. The old software was removed since we've installed ~8 years of software and the software tree has grown too large to maintain. More importantly, most of the previous software was built with the old OS system packages and libraries and would simply be incompatible with the rebuilt OS.

LCRC staff have already installed several compilers and MPI variants as well as some standard packages. We've asked users to let us know if they need any software re-installed. If we have not been notified about a software request, the expectation will be that it won't be installed. We will gladly try to assist in installing software globally if it useful to several LCRC users, otherwise we encourage building and compiling in your project directories.

If you have software built in your home and/or project directories prior to the rebuild, you are more than welcome to try and use it. However, if it does not work correctly or at all, you should recompile your software. Anything that links to the old software tree is subject to stop working once we permanently delete the old tree in the near future.

Please e-mail [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov) with any software requests.

### What software modules are now loaded by default?

We have decided against loading any software modules by default going forward. We hope to phase out older modules after a certain amount of time to reduce software installation bloat on
 the system and to encourage the use of newer software. Compilers and MPI versions will need to be loaded and/or saved to load on login by the user. When we do decide to retire old software, we will send an announcement with a generous lead time.

### Do I still request allocations on Bebop in Core Hours?

Bebop is now calculating allocations in Node Hours instead of Core Hours. This is also how we set allocations on the Improv cluster. You should request time on Bebop for future quarters in Node Hours. For FY24 Q4, we have already converted previously submitted Core Hours to Node Hours on your behalf. For quick reference:

`1 Bebop Node Hour = 36 Core Hours`

[https://docs.lcrc.anl.gov/allocation-management/allocations](https://docs.lcrc.anl.gov/allocation-management/allocations)

### I have a condo queue on the Bebop cluster. What account should I use to submit my jobs?

Previously, condo queue users would submit jobs to their queues on Bebop with the `condo` account in Slurm. This has changed with PBS. Going forward, you should submit your jobs with the same account that is used to access your condo nodes. The following is a list of condo queues and the account that should be used:

| Condo Queue | New Account Name |
|------------|------------|
| csed | g-CSE |
| dis | g-DIS |
| es | g-ES |
| es-mpc | g-ES-mpc |
| esd | g-ESd |
| hepd | g-HEP or g-ATLAS |
| phyd | g-Physics |
| xsd | g-XSD |

As before, condo queues do not restrict usage based on node hour availability, however, an account is required to submit jobs.
 
### I am new to the cluster. Are startup accounts still available?

With the transition to PBS, we are not currently using startup accounts. If you need to run on Bebop, you should either join an LCRC project or request a new one via the accounts page. Please see our documentation about LCRC projects here:

[https://docs.lcrc.anl.gov/account-project-management/project-management](https://docs.lcrc.anl.gov/account-project-management/project-management.md)

### My jobs are failing with SSH or Host Key Verification failures. What can I do?

As mentioned above, run the following after logging into a Bebop login node: 

`mv ~/.ssh/known_hosts ~/.ssh/known_hosts.bak`

This will move your current LCRC SSH Known Hosts file to a backup file, and you should no longer have an issue with running a job.

### I cannot find "X" documentation on the LCRC website. What should I do?

We have a lot of documentation for Bebop on the LCRC website and will be working to update this in the days and weeks following the rebuild. If something is missing or inaccurate, feel free to let us know by sending e-mail to [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov).

## Getting Support

If you have any questions or issues following the rebuild, please e-mail [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov) and LCRC staff will assist you.
