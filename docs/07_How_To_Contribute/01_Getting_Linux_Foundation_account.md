---
title: Getting Linux Foundation account
---

In order to participate in the development of the Automotive Grade Linux
project, you will need a Linux Foundation account. You will need to use
your LF ID to access to all the AGL community development tools,
including [Gerrit](http://gerrit.automotivelinux.org/),
[Jira](https://jira.automotivelinux.org/) and the
[Wiki](https://wiki.automotivelinux.org/) (for editing, only).

**NOTE:** Further information about Contributing to the AGL Distro
available at [AGL wiki](https://wiki.automotivelinux.org/agl-distro/contributing).

## Creating Linux Foundation ID

 1. Go to the [Linux Foundation ID website](https://identity.linuxfoundation.org/).

 2. Select the option `I need to create a Linux Foundation ID`, and fill
 out the form that appears. (It is advised to authenticate through email
 instead of logging through Facebook/Google/Github.)

 3. Wait a few minutes, then look for an email message with the subject
 line: `Validate your Linux Foundation ID email`.

 4. Open the received URL to validate your email address.

 5. Verify that your browser displays the message  ``You have
 successfully validated your e-mail address``.

 6. Access [Gerrit](http://gerrit.automotivelinux.org/) by selecting
 ``Sign In``, and use your new Linux Foundation account ID to sign in.

## Configuring Gerrit to Use SSH

Gerrit uses SSH to interact with your Git client. If you already have an SSH
key pair, you can skip the part of this section that explains how to generate one.

What follows explains how to generate an SSH key pair in a Linux environment ---
follow the equivalent steps on your OS.

First, create an SSH key pair with the command:
**Note:** This guide recommends using ed25519 keys because it has been found that this type works well across all operating systems.

 ```sh
 $ ssh-keygen -t ed25519 -C "your_name@example.com"
 ```

**Note:** When you’re prompted to “Enter a file in which to save the key” press Enter. This accepts the default location. Next, it will ask you for a password to protect the private key as
it generates a unique key. Please keep this password private, and DO NOT
enter a blank password.

The generated SSH key pair can be found in the files ``~/.ssh/id_ed25519`` and
``~/.ssh/id_ed25519.pub``.

Next, add the private key in the ``id_ed25519`` file to your key ring, e.g.:

 ```sh
 $ ssh-add ~/.ssh/id_ed25519
 ```

Finally, add the public key of the generated key pair to the Gerrit
server, with the following steps:

1. Go to [Gerrit](http://gerrit.automotivelinux.org/).

2. Click on your account name in the upper right corner.

3. From the pop-up menu, select ``Settings``.

4. On the left side menu, click on ``SSH Public Keys``.

5. Paste the contents of your public key ``~/.ssh/id_ed25519.pub`` and click
   ``Add key``.

**Note:** The ``id_ed25519.pub`` file can be opened with any text editor or you can run the command ``cat ~/.ssh/id_ed25519.pub`` in your terminal and copy output. Ensure that all the contents of the file are selected, copied and pasted into the ``Add SSH key`` window in Gerrit.

**Note:** The SSH key generation instructions operate on the assumption
that you are using the default naming. It is possible to generate
multiple SSH keys and to name the resulting files differently. See the
[ssh-keygen](https://en.wikipedia.org/wiki/Ssh-keygen) documentation for
details on how to do that. Once you have generated non-default keys, you
need to configure SSH to use the correct key for Gerrit. In that case,
you need to create a ``~/.ssh/config`` file with command ``touch ~/.ssh/config`` and add details in config file.

```
host gerrit.automotivelinux.org
  HostName gerrit.automotivelinux.org
  IdentityFile ~/.ssh/id_ed25519
  User <LFID>
  Port 29418
```

`<LFID>` is your Linux Foundation ID(username) and the value of IdentityFile is the
name of the public key file you generated.

**Warning:** Potential Security Risk! Do not copy your private key
``~/.ssh/id_ed25519``. Use only the public ``~/.ssh/id_ed25519.pub``.
