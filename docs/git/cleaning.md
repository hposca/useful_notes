# Deleting a git tag

From: [How to: Delete a remote Git tag - Nathan Hoad](https://nathanhoad.net/how-to-delete-a-remote-git-tag)

- Delete your local tag:

    ```
    git tag -d 12345
    ```

- Remove it from the remote:

    ```
    git push origin :refs/tags/12345
    ```

# Deleting unused local branches

    git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d

# Purging a file from your repository's history

Instructions from [Removing sensitive data from a repository - User Documentation](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)

Remove it from the history:

```bash
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA' --prune-empty --tag-name-filter cat -- --all
```

Add it to gitignore:

```bash
echo "YOUR-FILE-WITH-SENSITIVE-DATA" >> .gitignore
git add .gitignore
git commit -m "Add YOUR-FILE-WITH-SENSITIVE-DATA to .gitignore"
```

Force push:

```bash
git push origin --force --tags
```
