# Getting Started on Swing

## Accessing Swing

As part of upcoming security enhancements, direct SSH access to swing.lcrc.anl.gov will be disabled starting on the maintenance day April 14, 2025. All inbound direct access to Swing login and compute nodes will be removed.

Instead, users must now connect by jumping through the CELS login nodes. Note: you cannot directly log in to the CELS login nodes, but you can use them as jump hosts.

An updated SSH command for connecting to Swing looks like this:

`ssh -i ~/.ssh/<ssh_private_key> -J <username>@logins.lcrc.anl.gov <username>@swing.lcrc.anl.gov`

Alternatively, you can configure your ~/.ssh/config file as follows:

```bash
Host swing.lcrc.anl.gov swing
  HostName     swing.lcrc.anl.gov
  ProxyJump    logins.lcrc.anl.gov
  User         <username>
  IdentityFile ~/.ssh/<ssh_private_key>
```

Once configured, you can connect to Swing simply by running:

`ssh swing.lcrc.anl.gov`

or

`ssh swing`

## System Architecture

For a detailed overview of the Swing cluster, including the compute node architecture, refer to the [Hardware Overview](../swing/hardware-overview-swing.md) page.

## Job Execution

For information on how to run jobs on Swing, refer to the [Running Jobs](../swing/running-jobs-swing.md) page.
