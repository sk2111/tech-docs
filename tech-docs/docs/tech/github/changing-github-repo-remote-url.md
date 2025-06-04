---
sidebar_position: 2
---

# Changing github repo remote URL

Changing the remote Git repository URL in a local Git repository can be done
using the `git remote set-url` command.

This is useful when you want to switch from one remote repository to another,
such as changing from a personal repository to an organizational one.

`git remote set-url origin https://sathish-thangavel@github.com/<org>/<repo>.git`

## Set local git username and email

If you want to set the local Git username and email for the repository,
you can use the following commands:

```bash
git config user.name "<Your Name>"
git config user.email "<Your Email>"
```

## Set global git username and email

If you want to set the global Git username and email, which will apply to all
repositories on your machine, you can use the following commands:

```bash
git config --global user.name "<Your Name>"
git config --global user.email "<Your Email>"
```

## Verify the changes

To verify that the remote URL has been changed, you can use the following command

```bash
git remote -v
```
