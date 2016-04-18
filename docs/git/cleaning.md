# Deleting a git tag

From: [How to: Delete a remote Git tag - Nathan Hoad](https://nathanhoad.net/how-to-delete-a-remote-git-tag)

- Delete your local tag

    git tag -d 12345

- Remove it from the remote

    git push origin :refs/tags/12345
