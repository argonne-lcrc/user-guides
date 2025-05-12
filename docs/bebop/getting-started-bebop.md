# Getting Started on Bebop

## Accessing Bebop

Due to updated security requirements, direct SSH access to Bebop is no longer permitted. All inbound access must now go through the CELS login nodes using a jump host configuration.

You cannot SSH directly into the CELS login nodes. Instead, connect using the command below (replacing `<username>` and `<ssh_private_key>` accordingly):

```bash
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@bebop.lcrc.anl.gov
```

Alternatively, you can simplify future connections by adding the following block to your `~/.ssh/config`:

```bash
Host logins.lcrc.anl.gov
  HostName logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>

Host bebop.lcrc.anl.gov bebop
  HostName bebop.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>
```

After configuring this, you can connect with either:

```bash
ssh bebop.lcrc.anl.gov
```

or simply:

```bash
ssh bebop
```

Note: The logins.lcrc.anl.gov alias is provided as part of LCRC's integration with CELS login infrastructure.

## System Architecture

For a detailed overview of Bebop, including the compute node architecture, refer to the [Hardware Overview](../bebop/hardware-overview-bebop.md) page.

## Job Execution

For information on how to run jobs on Bebop, refer to the [Running Jobs](../bebop/running-jobs-bebop.md) page.

## Bebop Rebuild FAQ

On July 1, 2024, Bebop was rebuilt with a new operating system, featuring an entirely new software tree and a transition from Slurm to PBS Professional as the job scheduler. If you haven't logged in for a while and are experiencing connection issues, or for more details about these changes, please refer to our [Bebop Rebuild FAQ here](../bebop/bebop-rebuild-faq.md).
