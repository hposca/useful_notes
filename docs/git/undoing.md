Undoing mistakes

# Find and restore a deleted file

With the help of [StackOverflow](http://stackoverflow.com/questions/953481/find-and-restore-a-deleted-file-in-a-git-repo):

Find the last commit that affected the given path. As the file isn't in the HEAD commit, this commit must have deleted it.

~~~ bash
git rev-list -n 1 HEAD -- <file_path>
~~~

Then checkout the version at the commit before, using the caret (^) symbol:

~~~ bash
git checkout <deleting_commit>^ -- <file_path>
~~~

Or in one command, if `$file` is the file in question.

~~~ bash
git checkout $(git rev-list -n 1 HEAD -- "$file")^ -- "$file"
~~~

__HINT:__ Use `git log --diff-filter=D --summary` to get all the commits which have deleted files and the files deleted;

__HINT 2:__ [from here](http://blog.kablamo.org/2013/12/08/git-restore/) How to search the contents of deleted files

Lets say I don’t remember the filename of that file I deleted in a fit of cleanup passion. I do remember the name of one of the functions in it though. Here is how to deal with that. Search the contents of all files that have ever existed in git for a string:

~~~ Bash
git log --summary -S<string> [<path/to/file>] [--since=2009.1.1] [--until=2010.1.1]
~~~

Another way to do this:

~~~ Bash
git rev-list --all | xargs git grep 'string'
~~~


# Undoing a git rebase

Sometimes we want to undo a bad `git rebase`, hints from [StackOverflow](http://stackoverflow.com/questions/134882/undoing-a-git-rebase) :

* First, we have to discover where is the head commit of the branch as it was immediately before the rebase started in the reflog:

~~~ Bash
git reflog
~~~

or

~~~ Bash
git log -g
~~~

* Then, reset the current branch to it (with the usual caveats about being absolutely sure before reseting with the --hard option):

~~~ Bash
# Suppose the old commit was HEAD@{5} in the ref log
git reset --hard HEAD@{5}
~~~

__HINT:__ Just in case, make a backup first: `git tag BACKUP`. You can return to it if something goes wrong: `git reset --hard BACKUP`

## Desfazendo o último commit

~~~ Bash
git reset --soft HEAD~1
~~~

# Desfazendo o primeiro commit de um repositório

~~~ Bash
git update-ref -d HEAD
~~~

# Undo a git merge

From [Undo a git merge](http://stackoverflow.com/questions/2389361/undo-a-git-merge) :

~~~ Bash
git reset --hard commit_sha
# ou
git reset --hard HEAD~5 #will get you back 5 commits.
# ou
git reset --merge ORIG_HEAD
~~~

## How to revert a merge that is already pushed to remote?

From this [StackOverflow hint](http://stackoverflow.com/questions/7099833/how-to-revert-a-merge-commit-thats-already-pushed-to-remote-branch):

    git revert merge_sha -m [1,2]

To discover if you have to use 1 or 2 do a `git log` on the merge_sha, then 1 will represent the left SHA and 2 the right SHA that appear in the 'Merge' line.

# Removing sensitive files from old commits

From [Remove sensitive data - User Documentation](https://help.github.com/articles/remove-sensitive-data/):

    git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA' --prune-empty --tag-name-filter cat -- --all

Then, to remove the newly generated 'refs/original' reference and clear the reflog and do a garbage collect:

    git for-each-ref --format='delete %(refname)' refs/original | git update-ref --stdin
    git reflog expire --expire=now --all
    git gc --prune=now

# Going way back to the origin of time

```
git checkout --orphan branch_name
```
