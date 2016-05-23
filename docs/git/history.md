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

## Branches and commits between two commits

- All commits between two commits (only the direct path)

    git log --ancestry-path <root>..<descendant>

- Current branches that are children of another

    git branch --contains <root branch>

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

# Edit an incorrect commit message in Git

With hints from [StackOverflow](http://stackoverflow.com/questions/179123/edit-an-incorrect-commit-message-in-git):

1. If the commit you want to fix isn’t the most recent one:

    If you want to fix several flawed commits, pass the parent of the oldest one of them.
    `git rebase --interactive $parent_of_flawed_commit`

1. An editor will come up, with a list of all commits since the one you gave.

    1. Change `pick` to `reword` (or on old versions of Git, to _edit_) in front of any commits you want to fix.
    1. Once you save, Git will replay the listed commits.

1. For each commit you want to `reword`, Git will drop you back into your editor. For each commit you want to edit, Git drops you into the shell. If you’re in the shell:

    1. Change the commit in any way you like.
    1. `git commit --amend`
    1. `git rebase --continue`

Most of this sequence will be explained to you by the output of the various commands as you go. It’s very easy, you don’t need to memorise it – just remember that `git rebase --interactive` lets you correct commits no matter how long ago they were.
