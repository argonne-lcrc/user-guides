# Logging In and SSH Keys

**LCRC only supports OpenSSH-based clients and does not support PuTTY.**
  
After you have [configured Duo MFA](mfa.md) for LCRC, you can continue to creating an SSH keypair. You will need **BOTH** Duo MFA and an SSH keypair configured to login to LCRC resources.

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

*A strong passphrase is required!*

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

## Logging In

Due to updated security requirements, direct SSH access to LCRC login nodes is no longer permitted. All inbound access must now go through the CELS login nodes using a jump host configuration.

You cannot SSH directly into the CELS login nodes. Instead, connect using the command below (replacing `<username>` and `<ssh_private_key>` accordingly):

### Basic Configuration

 We’ve added an LCRC alias to the CELS login nodes for this as well. Using Improv as an example, a connection would look like:
 
```bash
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@improv.lcrc.anl.gov
```

### Setting Up SSH Config

Alternatively, you can add something like the following to your .ssh/config file:

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

Host improv.lcrc.anl.gov improv
  HostName improv.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>

Host crossover.lcrc.anl.gov crossover
  HostName crossover.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>

Host chrysalis.lcrc.anl.gov chrysalis
  HostName chrysalis.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>

Host swing.lcrc.anl.gov swing
  HostName swing.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>
```

If you add the above to your SSH config file, you can do the following, using Improv as an example:

```bash
ssh improv.lcrc.anl.gov
```

or

```bash
ssh improv
```

Regardless of how you decide to SSH into LCRC, with Duo MFA also configured, you will see a Duo prompt during your SSH connection:

```bash
Duo two-factor login for <username>

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-1234

Passcode or option (1-1): 1
Success. Logging you in...
```

Assuming you have only configured one device, type in the number *1* on the 'Passcode or option' line like above and press Enter to choose your configured mobile device and *Accept* the Duo push on that mobile device. You should now be logged into the desired cluster.

## Using GUI Applications Over SSH

For GUI applications over SSH, enable X11 forwarding:

Command line:

```bash
ssh -X -o ProxyCommand="ssh -q -W %h:%p -i ~/.ssh/<ssh_private_key> <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@improv.lcrc.anl.gov
```

Or in your config file:

```bash
Host improv.lcrc.anl.gov improv
  HostName improv.lcrc.anl.gov
  ProxyJump logins.lcrc.anl.gov
  User <username>
  IdentityFile ~/.ssh/<ssh_private_key>
  ForwardX11 yes
```

Then connect as usual:

```bash
ssh improv
```

You can apply these same methods to other clusters.

## Windows Users

Windows 10/11 users connecting to LCRC via Command Prompt, PowerShell, or Visual Studio Code’s SSH extension may encounter errors such as “Corrupted MAC on input” or “message authentication code incorrect.” This results from an outdated OpenSSL library in Windows.

If you see this error, a simple workaround is to specify a supported MAC algorithm in your SSH command:

```bash
ssh -m hmac-sha2-512 -o ProxyCommand="ssh -q -W %h:%p -i ~/.ssh/<ssh_private_key> <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@improv.lcrc.anl.gov 
```

### Setting Up SSH Config on Windows (Command Prompt)
 
**1. Create the .ssh directory (if it doesn't exist):**

Open Command Prompt and run:

```console
if not exist "%USERPROFILE%\.ssh" md "%USERPROFILE%\.ssh"
```

**2. Generate the SSH config file with your host entries:**

Run the following block exactly as shown:

```console
(
echo Host *
echo MACs hmac-sha2-512
echo Host logins.lcrc.anl.gov
echo HostName logins.lcrc.anl.gov
echo User ^<username^>
echo IdentityFile ~/.ssh/^<ssh_private_key^>
echo Host bebop.lcrc.anl.gov bebop
echo HostName bebop.lcrc.anl.gov
echo ProxyJump logins.lcrc.anl.gov
echo User ^<username^>
echo IdentityFile ~/.ssh/^<ssh_private_key^>
echo Host improv.lcrc.anl.gov improv
echo HostName improv.lcrc.anl.gov
echo ProxyJump logins.lcrc.anl.gov
echo User ^<username^>
echo IdentityFile ~/.ssh/^<ssh_private_key^>
echo Host crossover.lcrc.anl.gov crossover
echo HostName crossover.lcrc.anl.gov
echo ProxyJump logins.lcrc.anl.gov
echo User ^<username^>
echo IdentityFile ~/.ssh/^<ssh_private_key^>
echo Host chrysalis.lcrc.anl.gov chrysalis
echo HostName chrysalis.lcrc.anl.gov
echo ProxyJump logins.lcrc.anl.gov
echo User ^<username^>
echo IdentityFile ~/.ssh/^<ssh_private_key^>
echo Host swing.lcrc.anl.gov swing
echo HostName swing.lcrc.anl.gov
echo ProxyJump logins.lcrc.anl.gov
echo User ^<username^>
echo IdentityFile ~/.ssh/^<ssh_private_key^>
) > "%USERPROFILE%\.ssh\config"
```

**3. Edit the SSH config file to insert your actual details:**

```console
notepad %USERPROFILE%\.ssh\config
```

Then:

* Replace `<username>` with your actual LCRC username.
* Replace `<ssh_private_key>` with the filename of your private key (e.g., id_ed25519).

**4. Test the SSH connection:**

Try:

`ssh improv`

You'll be prompted twice for your *SSH key* password followed by a Duo prompt.

### Connecting with WinSCP

**Note: These steps assume you have already set up a functional SSH config file. If not, refer to the previous guide before proceeding.**

**1. Open WinSCP and go to the Site Manager.**

**2. Click Tools (bottom left) and select Import Sites.**

<details markdown="1">
<summary>Click to view screenshot</summary>

![WinSCP 1](../images/winscp-1.png)

</details>

**3. A list of LCRC cluster entries should appear, pulled directly from your SSH config file.**

– Ensure all relevant entries are checked, then click OK.

<details markdown="1">
<summary>Click to view screenshot</summary>

![WinSCP 2](../images/winscp-2.png)

</details>

**4. When prompted to convert your OpenSSH private key to PuTTY format, click OK.**

<details markdown="1">
<summary>Click to view screenshot</summary>

![WinSCP 3](../images/winscp-3.png)

</details>

**5. Enter your private key passphrase when prompted.**

<details markdown="1">
<summary>Click to view screenshot</summary>

![WinSCP 4](../images/winscp-4.png)

</details>

**6. Save the resulting .ppk (PuTTY Private Key) file to your ~\.ssh\ directory.**

<details markdown="1">
<summary>Click to view screenshot</summary>

![WinSCP 5](../images/winscp-5.png)

</details>

**7. You should now see the imported LCRC clusters listed in Site Manager.**

* Select your desired cluster.
* Leave the **password** field blank.
* Click **Login** to connect.

<details markdown="1">
<summary>Click to view screenshot</summary>

![WinSCP 6](../images/winscp-6.png)

</details>

**8. During the connection, you will be prompted to:**

* Enter your **SSH private key passphrase** (you will be asked twice)
* Complete **Duo authentication**

<details markdown="1">
<summary>Click to view screenshot</summary>

![WinSCP 7](../images/winscp-7.png)

</details>

## Debugging a Failed Connection

If you encounter login issues, check your internet connection, ensure the correct public key is uploaded, verify your username, hostname, and SSH private key are correct, and check for any misconfigurations in `~/.ssh/config`.

Common Issues:

* Client Configuration: Verify the correct permissions for your SSH files.
* Server Configuration: Ensure your home and .ssh directories have the correct permissions.
* Maintenance and Login Nodes: [Be aware of maintenance periods and check if you're connecting to responsive nodes](../best-practices-and-policies/monthly-maintenance-day.md).
* Not enrolled in Duo MFA.

Advanced Troubleshooting:

If you're still facing issues, provide the output from the following commands to [support](mailto:support@lcrc.anl.gov):

```bash
ssh -vvv -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@improv.lcrc.anl.gov
ls -la ~/.ssh
cat ~/.ssh/config
```

### Specific Error Messages

There are a several error messages that you may need to look out for.

> Your account does not have access to this application. Contact an administrator for assistance.

You haven't joined the LCRC project or sub-project of LCRC. Please see our documentation on [joining a project](../project-management/#join-an-existing-lcrc-project).

> Access is not allowed because you are not enrolled in Duo. Please contact your organization's IT help desk.

You haven't enrolled in Duo MFA. Please see our documentation on [Duo MFA enrollment](mfa.md).

> Your account is disabled and cannot access this application. Please contact your administrator.

You have failed a Duo attempt 3 times in a row. Duo will automatically lock you out for *30 minutes*. You may try logging in again after the 30 minutes. If you continue to have lock out failures, please contact LCRC support.

> Too many authentication failures

You have failed an SSH key or Duo credential check and were kicked out to avoid a block in Duo (a Duo block will be issued after 3 failed attempts for 30 minutes and will be cleared automatically). You can try again with the correct SSH key passphrase and Accepting your login attempt from your Duo device.

> Operation Timed Out

You have failed an SSH key or Duo credential check too many times and have been blocked by the Argonne firewall. You will need to send an email to LCRC support with the IP address you are trying to connect from. Please make sure to send your real, public IP and not one from your local network. After we remove the block, please check your SSH and Duo settings to correct the issue before trying your connection again to avoid future blocks.

### Downed Login Nodes

Occasionally LCRC login nodes may become unresponsive due to a number of various failures unexpectedly. Because of the round-robin nature of the connection, you may land on one of these bad nodes when you try to SSH to LCRC. We'll try to make sure each node is up at all times, but you may attempt to connect to the clusters during this unexpected down time. To test whether or not the problem is on your end or on the LCRC side and if you've already exhausted the other troubleshooting steps, try to connect to a specific login node.

To do this, you can substitute your basic SSH command of `ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@improv.lcrc.anl.gov`, for example, with these instead.

For Improv:

```bash
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@ilogin1.lcrc.anl.gov
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@ilogin2.lcrc.anl.gov
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@ilogin3.lcrc.anl.gov
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@ilogin4.lcrc.anl.gov
```

For Bebop:


```bash
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@beboplogin1.lcrc.anl.gov
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@beboplogin2.lcrc.anl.gov
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@beboplogin3.lcrc.anl.gov
ssh -o ProxyCommand="ssh -i ~/.ssh/<ssh_private_key> -W %h:%p <username>@logins.lcrc.anl.gov" -i ~/.ssh/<ssh_private_key> <username>@beboplogin4.lcrc.anl.gov
```

If you try a couple of these nodes and still can't connect, you can continue troubleshooting. Of course in extremely rare cases most of our login nodes can be down so you can always contact us if you've exhausted all of your connection options.
