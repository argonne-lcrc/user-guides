# SSH Policy

This document explains the policies users must follow when creating, storing, and using SSH keys for accessing the LCRC systems.

## Summary

* SSHv1 keys are not supported, therefore v1 keys are unusable.
* SSHv2 RSA keys are allowed.
* SSHv2 keys must contain at least 4096 bits.
* SSHv2 keys must have a strong passphrase (details below).
* Keys should be generated on a known secure machine.
* Private keys should not be stored on LCRC systems.

## Explanation

The ability to crack SSHv2 keys depends directly on the type of key, the number of bits in the key, and the strength/quality and secrecy of the passphrase. The above guidelines are intended to minimize the risk of compromise if someone obtained a copy of your keys or was able to intercept your SSH session.

## SSH Passphrases

SSHv2 keys must have a strong passphrase that:

1. Is NOT a word in any language
2. Is NOT a proper noun/name, brand name, foreign name, or other name
3. Is NOT a word or set of words obtainable by:
    * "finger" command (e.g. QWERTY),
    * looking in your .plan, .project, or other public files,
    * looking at any of your public web pages.
4. Is NOT a word followed by a number.
5. Is NOT a reversed version of any of the above.
6. Is NOT an address (e.g. 9700S.Cass).
7. Is NOT derived from a single word (e.g. He77o).
8. Is a long passphrase, must be longer than 8 characters. SSH passphrases can be made up of several words or character strings. Unlike traditional Unix passwords that are limited to 8 characters, these can and should be much longer. However, it should not be so long that you are unable to type it correctly multiple times.
9. Should contain mixed case letters, digits, and special characters.

* Should be kept secret and NOT written down anywhere.
* Should NOT be used as a password anywhere else, or as passphrases to other SSH keys.

Failure to comply with the strong passphrase guidelines may make your passphrase guessable by people who are resourceful in finding information about you, or crackable using commonly available cracking software.

You should NOT use the passphrase to your LCRC SSH key(s) as a password on any other machines. If you share passwords or passphrases between resources and one resource is compromised and passwords or passphrases are stolen, they could be used to compromise LCRC.

If you learn of a security compromise at a remote site where you have an account please notify support@lcrc.anl.gov.

NEVER GIVE YOUR PASSPHRASE TO ANYONE! Never tell anyone your passphrase or password over
the phone. Nobody from LCRC will ask for your passphrase or password over the phone (we can access your account without it, system administrators never need to know your passphrase or password). If someone calls you and asks for your passphrase or password, please report this by sending mail to support@lcrc.anl.gov. If you receive an email from someone requesting your passphrase or password (including support@lcrc.anl.gov and root), please inform us immediately.

NEVER WRITE YOUR PASSPHRASE OR PASSWORD DOWN. Make your passphrase unique but something you can remember so you don’t have to write it down. Having a longer passphrase can help you remember it. If the piece of paper you write your passphrase down on is stolen, your
SSH keys could be compromised.

## Storing SSH Key Pairs

Protecting your ssh private keys is important:

* Set permissions so that only you have read/write access (i.e. `chmod 600 <privatekeyfile>` ). If you are a Cygwin user, please read the note about Cygwin Unix file permissions at the bottom of this page.
* Try to only have them on a single machine that is locked down very tightly (i.e. no remote access allowed, no sshd, ftpd, telnetd, etc running), firewall protected, up-to-date on all security patches, including kernel patches.
* Try not to have them on an NFS mounted file system.

Public keys don’t need to be protected:

* May be copied to other machines.
* May have permissions that allow others to read it.
* We recommend that the authorized_keys file have permissions of 600.

Security of SSH keys depends on keeping both the private key and the passphrase secret. The best way to keep the private key secure is to store it on known secure machines like a personal laptop or workstation. When a machine is compromised the private keys on that machine are available to the hacker. It’s very important to keep your private keys on as few machines as possible, to pick the most secure machines possible, and to avoid whenever possible storing them on machines and file-systems available to many users.

If you wish to be even more secure, you can keep your keys on external media, such as a USB memory stick. Then when you wish to log onto a remote computer, mount the external media on your local system, add the key to your ssh-agent with a very short timeout (1 minute is reasonable), unmount the external media and open the connection to the
remote system. This allows you get access to the remote system while keeping your private key virtually inaccessible to any remote hacker. For information on using ssh-agent, read the ssh-agent man page.

## Using SSH Key Pairs

When you use a passphrased SSHv2 key the ssh client will prompt you for your passphrase. This passphrase is used on your local machine to decrypt your private key so it can be used to connect to the remote machine. The private key never leaves the client machine (in encrypted or decrypted form).

For the remote machine to accept your ssh connection it must have your public key. Instructions for setting up your LCRC keys are available [here](../../account-project-management/ssh).

If you ssh many times and you wish to avoid typing in the passphrase every time, you can use an ssh-agent. An ssh-agent allows your client machine to keep a decrypted form of your ssh private key in memory for use when ssh'ing to multiple machines. For information on using ssh-agent, read the ssh-agent man page.

You may use ssh-agent forwarding when connecting through one machine to another machine. But, because of security issues, you should only enable agent forwarding for connections where you will need it and shutdown the connection as soon as you are finished using it.
