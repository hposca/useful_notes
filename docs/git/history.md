# Analisando a história

## Mudanças entre dois changests

Apenas os arquivos que foram mudados:

~~~ Bash
git diff --name-only sha1..sha2
# ou
git diff --name-only HEAD~10..HEAD~5
~~~

O conteúdo do que foi mudado:

~~~ Bash
git diff sha1..sha2
# ou
git diff HEAD~10..HEAD~5
~~~

# Revert a commit alredy pushed to a remote repository

From [this blog entry](http://christoph.ruegg.name/blog/git-howto-revert-a-commit-already-pushed-to-a-remote-reposit.html) there are two good ways:

~~~ Bash
git push origin +commit_sha^:master
~~~

or (The one I used):

~~~ Bash
git reset HEAD^ --hard
git push origin -f
~~~

# Making git “forget” about a file that was tracked but is now in .gitignore

.gitignore will prevent untracked files from being added (without an add -f) to the set of files tracked by git, however git will continue to track any files that are already being tracked.

To stop tracking a file you need to remove it from the index. This can be achieved with this command.

~~~ Bash
git rm --cached <file>
~~~

The removal of the file from the head revision will happen on the next commit.

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
