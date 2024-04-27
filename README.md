
# Multiple Users and Multiple Projects Guide

By default, the `git` command uses the default SSH key pairs to communicate with GitHub.com. When you have different accounts and work on different projects, you need to do some special setup.

## Create Specific SSH Keys for Your User

Generate a new SSH key specifically for this account:

```shell
ssh-keygen -t ed25519 -C "your_email@oap.com" -f ~/.ssh/oap_key
```

## Copy Your Public Key to Your GitHub Account:

1. Copy your public SSH key to the clipboard (for Windows):

    ```shell
    cat ~/.ssh/oap_key.pub | clip
    ```

    For macOS:

    ```shell
    pbcopy < ~/.ssh/oap_key.pub
    ```

    For Linux (with xclip installed):

    ```shell
    xclip -sel clip < ~/.ssh/oap_key.pub
    ```

2. Go to your GitHub account on github.com, login, and navigate:
    - Go to **Settings** under your account
    - Select **SSH and GPG keys**
    - Click **New SSH key** button, paste your key there

## Test Your SSH Connection:

To check your setup is correct, you can run:

```shell
ssh -T git@github.com
```

## Add New SSH Config Beside the Default GitHub Config

Configure your SSH to recognize a new profile for GitHub using a specific key:

```config
# Default GitHub 
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes

# New config for specific account
Host oap.github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/oap_key   
  IdentitiesOnly yes
```

Now your system can recognize `oap.github.com` as an alias for `github.com`.

## Test Your SSH Connection Using the Alias

```shell
ssh -T git@oap.github.com
``` 

If everything is set up correctly, you'll see a message from GitHub stating that you've successfully authenticated, but GitHub does not provide shell access.

## Clone Your Git Repo Using the Specific Host Alias

Clone your repository using the new SSH configuration:

```shell
git clone git@oap.github.com:oap/oap_git_test.git
```

## Modify the Existing Repository Remotes:

Open the `.git/config` file in a text editor and modify it as follows:

```plaintext
[remote "origin"]
    url = git@oap.github.com:oap/oap_git_test.git
    fetch = +refs/heads/*:refs/remotes/origin/*
``` 

## Test Your Setup

To ensure that your configuration works, run:

```bash
git fetch origin
```
