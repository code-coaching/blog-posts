---
title: Using SSH to work with GitHub
slug: using-ssh-to-work-with-github
tags:
  - SSH
  - GitHub
categories:
---

When there is a reference to `terminal` in the text below and Windows is used as an operating system, it means that the command needs to be executed in `Git Bash`. If CTRL+V does not work to paste the copied content in the terminal, try SHIFT+INSERT.

## What is SSH?

The SSH protocol is a way to authenticate a device with a server or service. **S**ecure **SH**ell is a network protocol to connect securely over an insecure connection.

## Why use SSH with GitHub?

Since the 13th of August 2021 it is no longer possible to authenticate with username and password. There are two options to authenticate: a personal token or SSH.

To work with SSH, a key pair needs to be generated:

- a private key, this should **never** be shared with anyone
- a public key, this can be shared

## Generating SSH keys

```sh
ssh-keygen -t ed25519 -C "john@duck.com"
```

Note: Change `"john@duck.com"` by the email address used to create the GitHub account.

A notification will appear to pick a location to save the keys, simply hit `Enter` to use the default location.
A notification will appear to enter a `passphrase`. Enter a passphrase (the typed characters will not show up on the screen, but they will be registered anyway).
Enter the passphrase again (the typed characters will not show up on the screen, but they will be registered anyway).

## Adding the public key to GitHub

In case the default location was chosen, the command below can be used to get the contents of the public key.

```sh
cat ~/.ssh/id_ed25519.pub
```

Copy the printed line `ssh-ed25519 [this will show all kinds of letters and numbers] [this will show the maill address]`.

Go to the [GitHub page to enter SSH keys](https://github.com/settings/keys) (click on profile top right, then on `Settings` and then on `SSH and GPG keys`).

Click on `New SSH key` (green button). Enter a title of your choice and paste the copied key in the field named `Key`.

Click on `Add SSH key`.

## Cloning a repo with SSH

While on the home page of a repo, click on the green button `Code`. In the popup that appears, there are three tabs:

- `HTTPS`: this was the way to clone the repo with username and password en these days it is used in combination with personal tokens.
- `SSH`: this is what is necessary to clone with ssh, copy the `git@github.com:here-some-more-text.git` to then execute `git clone git@github.com:here-some-more-text.git` in the terimal.
- `GitHub CLI`: another way to work with repos on GitHub.

SSH is the chosen option because this works for all platforms (GitHub, BitBucket, GitLab, etc.).
