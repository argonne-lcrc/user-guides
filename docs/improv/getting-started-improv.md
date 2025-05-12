# Getting Started on Improv

## Accessing Improv

Due to updated security requirements, direct SSH access to Improv is no longer permitted. All inbound access must now go through the CELS login nodes using a jump host configuration.

You cannot SSH directly into the CELS login nodes. Instead, connect using the command below (replacing `<username>` and `<ssh_private_key>` accordingly):

```bash
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@improv.lcrc.anl.gov
```

Alternatively, you can simplify future connections by adding the following block to your `~/.ssh/config`:

```bash
Host logins.lcrc.anl.gov
  HostName logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>

Host improv.lcrc.anl.gov improv
  HostName improv.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>
```

After configuring this, you can connect with either:

```bash
ssh improv.lcrc.anl.gov
```

or simply:

```bash
ssh improv
```

Note: The logins.lcrc.anl.gov alias is provided as part of LCRC's integration with CELS login infrastructure.

## System Architecture

For a detailed overview of the Improv system, including the compute node architecture, refer to the [Hardware Overview](../improv/hardware-overview-improv.md) page.

## Job Execution

For information on how to run jobs on Improv, refer to the [Running Jobs](../improv/running-jobs-improv.md) page.
