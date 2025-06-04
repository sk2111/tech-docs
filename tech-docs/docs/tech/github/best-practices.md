---
sidebar_position: 1
---

# Best Practices

1. **Use Branch Protection Rules**: Enforce rules on branches to ensure code
   quality and prevent unauthorized changes. This includes requiring pull
   requests for changes, enforcing status checks, and restricting who
   can push to the branch.
2. **Code Owners & PR Templates**: Use `CODEOWNERS` files to define who
   is responsible for code in specific directories. This helps in ensuring
   that the right people review changes. Also, use pull request templates
   to guide contributors on how to submit changes.
3. **Remove unused branches**: Regularly clean up branches that are no longer
   needed. This helps in keeping the repository organized and reduces clutter.
   Use the `git branch -d <branch-name>` command to delete local branches,
   and `git push origin --delete <branch-name>` to delete remote branches.
