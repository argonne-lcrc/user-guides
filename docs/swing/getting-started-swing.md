# Getting Started on Swing

## Accessing Swing

Due to updated security requirements, direct SSH access to Swing is no longer permitted. All inbound access must now go through the CELS login nodes using a jump host configuration.

You cannot SSH directly into the CELS login nodes. Instead, connect using the command below (replacing <username> and <ssh_private_key> accordingly):

`ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@swing.lcrc.anl.gov`

Alternatively, you can simplify future connections by adding the following block to your `~/.ssh/config`:

```bash
Host logins.lcrc.anl.gov
  HostName logins.lcrc.anl.gov
  User username
  IdentityFile ~/.ssh/id_rsa

Host swing.lcrc.anl.gov swing
  HostName swing.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User username
  IdentityFile ~/.ssh/id_rsa
```

After configuring this, you can connect with either:

`ssh swing.lcrc.anl.gov`

or simply:

`ssh swing`

Note: The logins.lcrc.anl.gov alias is provided as part of LCRC's integration with CELS login infrastructure.

## System Architecture

For a detailed overview of the Swing cluster, including the compute node architecture, refer to the [Hardware Overview](../swing/hardware-overview-swing.md) page.

## Job Execution

For information on how to run jobs on Swing, refer to the [Running Jobs](../swing/running-jobs-swing.md) page.