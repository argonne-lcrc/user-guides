# Logging In and SSH Keys

All of the commands that appear in the following steps should be run in your terminal. You don't need to type the leading `$` symbol; it just represents your prompt. Type the commands that occur after it and hit Enter or Return. Lines that do not start with a `$` symbol represent the output of the command. The exact output may vary depending on your operating system and the version of OpenSSH you are running.

Generating an SSH Key Pair
--------------------------

First of all, you should check if you already have an SSH key pair. Run the following command in your terminal:

`$ ls ~/.ssh`

If you see `id_rsa` and `id_rsa.pub` files, you already have SSH keys in place. You can skip to the next section: Uploading your SSH public key. If you get an error message saying "No such file or directory" or you don't see those files listed, keep reading. To generate an SSH key pair, you are going to use the `ssh-keygen` command. For maximum security, you want to generate a 4096 bit RSA key. This is generally the default on most new versions of OpenSSH. Run the following command:

```
$ ssh-keygen -b 4096 -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (~/.ssh/id_rsa):
```

Simply hit Enter or Return to select the default filename. If you choose a different filename, you will have to tell OpenSSH where to find it when logging in.

Enter passphrase (empty for no passphrase):

We require your SSH key to be passphrase protected. Otherwise, anyone who steals your laptop now has access to our clusters through your account! Note that OpenSSH requests a "passphrase", not a "password". This means that your passphrase can be several words separated by spaces. A long simple passphrase is harder for hackers to guess than a short difficult to remember password. It can even be something as silly as [correct horse battery staple](https://xkcd.com/936/). Note that when you start typing your passphrase, no text will appear. Don't panic! This is just to prevent onlookers from reading your passphrase. Just type the passphrase and hit Enter or Return when you are done.

Enter same passphrase again:

You will need to enter the same passphrase again to prevent typos.

```
Your identification has been saved in ~/.ssh/id\_rsa.
Your public key has been saved in ~/.ssh/id\_rsa.pub.
The key fingerprint is:
SHA256:XtUUVlVQgPhWV9SKQjl063VunST7J67oqwHbyowkDQM
The key's randomart image is:
+---\[RSA 4096\]----+
|         ..o..\*B%|
|          =..=. o|
|E        . oo+o+.|
| .        .o+.=oo|
|  o   . S .o.. .+|
|   +   = .    .. |
|  . o . +     ...|
|   o + . . . . ..|
|    . + .o+....  |
+----\[SHA256\]-----+
```

You're all done. You can move on to the next section now. Note that older versions of OpenSSH may not display a key's randomart.

Uploading Your SSH Public Key
-----------------------------

Now that you have an SSH key pair, you need to upload your _public_ key to your account. As the output of `ssh-keygen` mentioned, `~/.ssh/id_rsa.pub` is your _public_ key and `~/.ssh/id_rsa` is your _private_ key. **Never** share your private key with anyone! If someone has your private key and can guess your password, they will have access to every computer you have ever logged in to with it. Print out your _public_ key by running:

`$ cat ~/.ssh/id_rsa.pub`

You'll get something like this: 

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCi2MTbbOEvMgvdHZDDsGOdnRCxqx4XlEyauE3KcAa6bmUxbdd7BafZ4TAMaoGIzbpVEgixrWQ+sb//BK/yY7zkkcUAV/U6vUB4AzzPbdMhLEycabr+Nfz68SrOJyD9FsA6Czq++XHJvO341zJ9S6EkptORukhLJYHSBNKQRUEpaTz67jIdzMbpEdzNIMRToWLh8OgX5ApARN0ix3NpP8O6qPh1+eamr3pOljteVyCbq4od9dvaBHK4iXMJR47hEo0ng66CwcRxxyC+XbmO9jmvKtck/tQ5yLxxvtjkCTQN1VVmYmSnHyj4VamykykDmI48Xj5dYp/YNRUxnhbgOZnp
``` 

Your public key is safe to share. It may also have your username and hostname or an email address at the end. Now, upload this public key to your account. Log in to the [LCRC account page](https://accounts.lcrc.anl.gov) using your Argonne Domain or Argonne Collaborator username and password and scroll down to the **SSH Public Keys** section. Click on the **+Add Key** button. Copy and paste your entire public key from the terminal into the new ssh key box. Make sure that your public key does not have any newlines in it and it also prefixed with the correct ssh key type (example ssh-rsa). Any invalid keys will not be saved! Now you can click on the **Save** button. You can optionally add a description on your key to keep track of them in the box below the key upload section. Your SSH key will automatically be installed onto your account and your should be able to login to the clusters now as long as you are part of a valid LCRC project.

Configuring OpenSSH (Optional)
------------------------------

LCRC currently runs several clusters. Following the directions below, you may interchange the cluster name based on your needs as they both use the same SSH keypair and filesystems. Bebop is currently the primary cluster and most users will be using this. When logging in, OpenSSH needs to know a few things. At the bare minimum, it needs to know the hostname of the computer you want to log in to. If the username on your computer is different than the username on LCRC clusters, you'll need to specify that too. If you didn't use the default `~/.ssh/id_rsa` filename for your SSH key when creating your SSH keypair, you'll need to specify that as well.

`$ ssh -i /path/to/your/ssh_private_key <username>@bebop.lcrc.anl.gov`

Luckily, OpenSSH provides a way to permanently store these settings so that logging in is as simple as:

`$ ssh bebop`

These settings are stored in the `~/.ssh/config` file. If you don't already have one, create a new one. The general format of the SSH config file looks like:

```
Host <aliasA>
    Key1 <valueA>
    Key2 <valueB>
```

```
Host <aliasB>
    Key1 <valueC>
    Key2 <valueD>
```

You can have multiple Hosts and multiple key-value pairs per Host. For example, here is a basic SSH config file for logging into Bebop:

```
Host bebop
    HostName bebop.lcrc.anl.gov
    User <username>
```

If your SSH key is named something different than `id_rsa`, you can specify that as well:

```
Host bebop
    HostName bebop.lcrc.anl.gov
    User <username>
    IdentityFile ~/.ssh/id_rsa_bebop
```

Logging In
----------

LCRC currently runs several clusters. Following the directions below, you may interchange the cluster name based on your needs as they both use the same SSH keypair and filesystems. Bebop is currently the primary cluster and most users will be using this. Once you have uploaded your public key to the clusters, you can try logging in. If you followed our **Configuring OpenSSH** instructions above, logging in is as simple as:

`$ ssh bebop`

If you skipped those instructions, you'll have to be a little more specific. For most users, you'll run the following command(s), substituting <username> for your LCRC username. **To access Bebop**:

`$ ssh <username>@bebop.lcrc.anl.gov`

**To access Blues**:

`$ ssh <username>@blues.lcrc.anl.gov`

Once you've logged in successfully, you're good to go. If you have any problems logging in, see **Debugging a Failed Connection** below.

Debugging a Failed Connection
-----------------------------

Your first steps to debugging why you may not be able to login to the LCRC clusters are to answer the following questions and to resolve them:

*   Do you have internet access?
*   Have you uploaded the correct public key?
*   Are you using the right username, hostname, and SSH private key? Remember to use your Argonne username to connect remotely.
*   Does your .ssh/config contain any incorrect settings?
*   Have you moved to using new computer? If so, you'll need to create a new SSH key pair, upload it, and configure OpenSSH to use it.

If you've troubleshooted the above, you likely see an error message that looks something like this:

Permission denied (publickey,hostbased).

There are many different things that could cause this.

#### Improper Configuration

##### Client Configuration

If you believe the key you uploaded to your account was correct and you still get the above error, you may be setting the wrong permissions on the files on your SSH client (your laptop/desktop/etc.) that you are trying to access the LCRC clusters from. SSH requires the following permissions to be set on your local SSH files (replacing id_rsa with the location of your key name):

```
chmod go-w ~<username>/
chmod 700 ~<username>/.ssh
chmod 600 ~<username>/.ssh/id_rsa
chmod 644 ~<username>/.ssh/id_rsa.pub
chmod 644 ~<username>/.ssh/authorized_keys
```

##### Server Configuration

If you've logged in successfully before and are all of a sudden having problems, have you made any changes to your remote directory permissions lately? Collaboration among scientists is important, and once in a while, you'll need to share files in your home directory with other colleagues. A naïve way to do this would be to run the following command (**don't run this command!**):

`$ chmod -R +rw ~`

This would give your colleagues read/write permission on every file and directory in your home directory, which seems like what you want. But it would also give everyone permission to edit files in your `~/.ssh` directory. They could delete your SSH public keys, or worse, add their own SSH public keys to your account and log in as you. They could have full access to your data and you would never know! OpenSSH is smart enough to recognize this threat, so unless the following conditions are met, it won't let you or anyone else log in:

```
chown <username> ~
chmod go-w ~<username>
chmod 700 ~<username>/.ssh
chmod 644 ~<username>/.ssh/authorized_keys
```
These are all of the default permissions. If you made the mistake of giving everyone write access to your home directory, email us at [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov), let us know what happened, and we'll sort everything out. So if that's the wrong way to give your colleagues access to your files, what is the right way? Well there's no problem with giving them read permissions to your files, or read/execute permissions on your directories; you just can't give them write access. You should also be more specific as to what you want to share. If you don't want to mess around with complicated permissions, the best way to share data is to copy it to your project folder, where everyone in your group should have access to it.

##### Keypair Location

If you didn't use the default `~/.ssh/id_rsa` filename for your SSH key when creating your SSH keypair, you'll need to specify that as well.

`$ ssh -i /path/to/your/ssh_private_key <username>@bebop.lcrc.anl.gov`

##### .ssh/config Misconfiguration

If you have configured your `~/.ssh/config` file, please make sure that it works as expected. You may want to remove it temporarily and try connecting again to determine whether or not this is the problem.

#### Maintenance Period

Once per month, usually on the second Monday of the month, we have a maintenance period in which we bring down the clusters to apply security updates or make changes to the job scheduler. We send out a reminder email the week beforehand. Maintenance doesn't usually last longer than a day, so try again tomorrow. If problems persist, let us know.

#### Downed Login Nodes

Occasionally LCRC login nodes may become unresponsive due to a number of various failures unexpectedly. Because of the round-robin nature of the connection, you may land on one of these bad nodes when you try to SSH to LCRC. We'll try to make sure each node is up at all times, but you may attempt to connect to the clusters during this unexpected down time. To test whether or not the problem is on your end or on the LCRC side and if you've already exhausted the other troubleshooting steps, try to connect to a specific login node. To do this, you can substitute your SSH commands of `ssh bebop.lcrc.anl.gov`, for example, with these instead. For Bebop:

```
ssh beboplogin1.lcrc.anl.gov
ssh beboplogin2.lcrc.anl.gov
ssh beboplogin3.lcrc.anl.gov
ssh beboplogin4.lcrc.anl.gov
```

For Blues:

```
ssh blueslogin1.lcrc.anl.gov
ssh blueslogin2.lcrc.anl.gov
ssh blueslogin3.lcrc.anl.gov
ssh blueslogin4.lcrc.anl.gov
```

If you try a couple of these nodes and still can't connect, you can continue troubleshooting. Of course in extremely rare cases most of our login nodes can be down so you can always contact us if you've exhausted all of your connection options.

#### Further Debugging

If for whatever reason you cannot successfully log in, shoot us an email at [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov). We'll need 3 things from you to diagnose your problem. Run the following commands and give us the output:

```
$ ssh -vvv <username>@bebop.lcrc.anl.gov
$ ls -la ~/.ssh
$ cat ~/.ssh/config
```

The first command enables verbose mode and should tell us exactly what is going wrong. The second command tells us if you have multiple SSH keys and are possibly using the wrong one. The third command tells us if you have any OpenSSH settings that are preventing you from logging in. You can either paste the output of these commands into the body of your email or attach them separately.

X11 Forwarding
--------------

In order to run programs with a graphical user interface (GUI) over SSH, you will need to enable X11 forwarding. This is fairly simple with OpenSSH. You can either run the following command:

`ssh -X <username>@bebop.lcrc.anl.gov`

or add the following to your `~/.ssh/config` file:

```
Host bebop
    HostName bebop.lcrc.anl.gov
    User <username>
    ForwardX11 yes
```

and run `ssh bebop` normally.

