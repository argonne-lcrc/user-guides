# Logging In and SSH Keys

## Creating SSH Keys

Generate a pair of SSH keys with the ssh-keygen command; in this example we create an ed25519 key.

```console
$ ssh-keygen -a 100 -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/USER/.ssh/id_ed25519):
```

*You can change the name of the ssh key here, which is useful if you have several keys. Make note of the name if you use a different one other than the default.  We will presume you went with the defaults here.*

 **Note: If you choose a different name you will need to specify the key to use in the ssh config file or in the ssh command itself.**

```console
Enter passphrase (empty for no passphrase):
```

*A strong passphrase is required*

```console
Enter same passphrase again:
```

*Repeat the strong passphrase.*

```console
Your identification has been saved in /Users/USER/.ssh/id_ed25519.
```

*(`id_ed25519` is your private key. Keep it secure, do not share it, and be cognizant of where you store it.)*

```console
Your public key has been saved in /Users/USER/.ssh/id_ed25519.pub.
The key fingerprint is:
SHA256:lSglfiIzcdJumCtz1RI03sulFJ3pA3hZcFMbPPEY14Y USER@foo
```

*This is your public key.*

## Adding Your Public Key to Your Account

After you generate your SSH key pair, add ONLY the public key to your account at <https://accounts.lcrc.anl.gov>.

* Copy the contents of the .pub file generated above (in the example above, `~/.ssh/id_ed25519.pub`)
* Choose “Add Key” at the bottom of the Account Information page after login.
* Paste what you copied into the “Key” section that appears.  The “Description” is purely informational and allows you to give it a meaningful or memorable name/description for future reference.

You will then be able to SSH directly to our login nodes.

## Logging In

LCRC supports multiple clusters, and you can adapt the following instructions to your specific cluster needs. Each cluster utilizes the same SSH keypair and file systems.

### Basic Configuration

When using OpenSSH for logging in, it must know at least the hostname you wish to connect to. If your local username differs from your LCRC cluster username, you'll need to specify the correct one. Also, if your SSH key isn't the default `~/.ssh/ed_25519`, specify its path.

```sh
ssh -i /path/to/your/ssh_private_key <username>@improv.lcrc.anl.gov
```

### Simplifying Future Logins

OpenSSH allows you to store configurations in `~/.ssh/config`, making logins simpler.

In `~/.ssh/config`, you can set parameters for various hosts. For example, to simplify logging in to Improv, you can add the following to your `~/.ssh/config` file:

```ssh
Host improv
    HostName improv.lcrc.anl.gov
    User <username>
    IdentityFile /path/to/your/ssh_private_key
```

After configuring OpenSSH, log in using:

```sh
ssh improv
```

You can apply this method to other clusters as well.

## Using GUI Applications Over SSH

For GUI applications over SSH, enable X11 forwarding:

Command line:

```sh
ssh -X -i /path/to/your/ssh_private_key <username>@improv.lcrc.anl.gov
```

Or in your config file:

```ssh
Host improv
   HostName improv.lcrc.anl.gov
   User username
   IdentityFile /path/to/your/ssh_private_key
   ForwardX11 yes
```

Then connect as usual:

```sh
ssh improv
```

## Debugging a Failed Connection

If you encounter login issues, check your internet connection, ensure the correct public key is uploaded, verify your username, hostname, and SSH private key are correct, and check for any misconfigurations in `~/.ssh/config`.

Common Issues:

* Client Configuration: Verify the correct permissions for your SSH files.
* Server Configuration: Ensure your home and .ssh directories have the correct permissions.
* Maintenance and Login Nodes: [Be aware of maintenance periods and check if you're connecting to responsive nodes](../best-practices-and-policies/monthly-maintenance-day.md)

Advanced Troubleshooting

If you're still facing issues, provide the output from the following commands to [support](mailto:support@lcrc.anl.gov):

```sh
ssh -vvv -i /path/to/your/ssh_private_key <username>@improv.lcrc.anl.gov
ls -la ~/.ssh
cat ~/.ssh/config
```

## Downed Login Nodes

Occasionally LCRC login nodes may become unresponsive due to a number of various failures unexpectedly. Because of the round-robin nature of the connection, you may land on one of these bad nodes when you try to SSH to LCRC. We'll try to make sure each node is up at all times, but you may attempt to connect to the clusters during this unexpected down time. To test whether or not the problem is on your end or on the LCRC side and if you've already exhausted the other troubleshooting steps, try to connect to a specific login node.

To do this, you can substitute your basic SSH command of `ssh -i /path/to/your/ssh_private_key <username>@improv.lcrc.anl.gov`, for example, with these instead.

For Improv:

```sh
ssh -i /path/to/your/ssh_private_key <username>@ilogin1.lcrc.anl.gov
ssh -i /path/to/your/ssh_private_key <username>@ilogin2.lcrc.anl.gov
ssh -i /path/to/your/ssh_private_key <username>@ilogin3.lcrc.anl.gov
ssh -i /path/to/your/ssh_private_key <username>@ilogin4.lcrc.anl.gov
```

For Bebop:

```sh
ssh -i /path/to/your/ssh_private_key <username>@beboplogin1.lcrc.anl.gov
ssh -i /path/to/your/ssh_private_key <username>@beboplogin2.lcrc.anl.gov
ssh -i /path/to/your/ssh_private_key <username>@beboplogin3.lcrc.anl.gov
ssh -i /path/to/your/ssh_private_key <username>@beboplogin4.lcrc.anl.gov
```

If you try a couple of these nodes and still can't connect, you can continue troubleshooting. Of course in extremely rare cases most of our login nodes can be down so you can always contact us if you've exhausted all of your connection options.
