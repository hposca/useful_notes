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
