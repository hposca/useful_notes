Dicas de como se utilizar branches no git.

## Showing which files have changed between git branches

From [Showing which files have changed between git branches](http://stackoverflow.com/questions/822811/showing-which-files-have-changed-between-git-branches) :

~~~ Bash
git diff --name-status master..branchName
~~~

## Git rename local git branch

From [Git rename local git branch](http://stackoverflow.com/questions/6591213/rename-local-git-branch) :

~~~ Bash
git branch -m <oldname> <newname>
# or, if you want to rename the current branch:
git branch -m <newname>
~~~

## Pushing local branch to origin

~~~ Bash
git push origin <branchName>
~~~

## Remove remote branches

From [Remove remote branches](http://stackoverflow.com/questions/2003505/delete-a-git-branch-both-locally-and-remotely) :

~~~ Bash
git push origin --delete <branchName> # or
git push origin :<branchName>
~~~

# Visualization

## Branches Hierarchy

~~~ Bash
git log --oneline --decorate --color --graph --all --simplify-by-decoration
# ou
git log --graph --simplify-by-decoration --pretty=format:'%d' --all
~~~

## Git log all branches

From [Git log all branches](http://www.lornajane.net/posts/2014/git-log-all-branches) :

~~~ Bash
git log --oneline --graph --decorate --all
~~~

# Moving

## Move commits from one branch to another

From [Move commits from one branch to another](http://effectif.com/git/move-commit-from-one-branch-to-another) :

~~~ Bash
# This will move commits from master to the new branch
git checkout -b some-feature
git checkout master
git reset --hard HEAD^
git checkout some-feature
~~~

## Movendo conteúdo de uma branch para outra

De vez em quando não podemos simplesmente mergear uma branch com a outra, pois isso pode trazer algumas modificações indesejadas.
Uma coisa que podemos fazer é crirar um conjunto de patches e transportá-los para a branch onde queremos aplicar as mudanças que foram feitas na outra branch.

**Gerando os patches:**

~~~ Bash
git checkout <SHA-1 do commit que você quer considerar como último>
git format-patch <SHA-1 do commit que você quer utilizar como primeiro>
# Serão gerados vários arquivos .patch
~~~

**Aplicando os patches:**

~~~ Bash
# Primeiramente, mude seu workspace para a branch onde você quer aplicar as mudanças
# e.g.: git checkout production

git am *.patch
# No caso que enfrentei tinha alguns problemas de espaço, final de linha, etc. então utilizei:
git am -3 --ignore-whitespace *.patch
~~~

Caso algum dos patches falhe na sua aplicação basta editar o arquivo, adicioná-lo com `git add` e dar um `git am --continue`

## Move the most recent commit

From [Move the most recent commit(s) to a new branch with Git](http://stackoverflow.com/questions/1628563/move-the-most-recent-commits-to-a-new-branch-with-git) :

~~~ Bash
git branch newbranch
git reset --hard HEAD~3 # Go back 3 commits. You *will* lose uncommitted work.
git checkout newbranch
~~~

## Movendo um diretório de um repositório para outro, preservando o histórico!

Quem nunca começou a mexer em um diretório e depois percebeu que ele precisava ficar em outro repositório, ou até ser um repositório separado (uma biblioteca, por exemplo)? Seus problemas se acabaram-se!! `git filter-branch` ao resgate!

Para esta receita de bolo queremos:

- Mover um diretório de um repositório A para um repositório B

E temos as seguintes condições:

- O repositório A contém outras coisas além do diretório que queremos mover
- Queremos preservar todo o histórico de commits que envolvam o diretório em questão

(depois de fazer um backup do seu repositório, utilizando sua forma favorita)

Na raiz do "repositório A":

~~~ Bash
git filter-branch --subdirectory-filter [oDitoCujoDoDiretorioQueQueremosSeparar] -- --all
~~~

If you want your code to be on another directory structure on repository B, execute the following commands still on Repository A:

~~~
mkdir -p [new directory location]
mv * [new directory location]
git add .
git commit
~~~

Este lindo comando fará, segundo a documentação, como se o repositório fosse a raíz do repositório e todo o resto, desconhecido.

Já no seu "repositório B":

~~~ Bash
git checkout -b new_branch
git remote add branchA [caminhoParaORepositorioA]
git pull branchA master --allow-unrelated-histories # Ao invés de master você pode colocar a branch que estava sendo utilizada no repositório A
git remote rm branchA
git checkout master
git merge --no-ff new_branch
git branch -d new_branch
~~~

Pronto! Agora em seu "repositório B" você terá todo o conteúdo do diretório que estava no "repositório A", e com o histórico preservado!
E isto tudo só foi possível devido às dicas [deste site](http://gbayer.com/development/moving-files-from-one-git-repository-to-another-preserving-history/)!
[This gist](https://gist.github.com/whistler/de34b77aba2221ed8b2e) has a very similar strategy based on the site above.

A simple bash script with all the steps at once:

```bash
#!/usr/bin/env bash

GITHUB_USER=""
REPO_FROM=""
REPO_FROM_BRANCH="master"
DIRECTORY_FROM="a/b/c/"
REPO_TO=""
REPO_TO_NEW_BRANCH="moving"
DIRECTORY_TO="c/"

git clone git@github.com:${GITHUB_USER}/${REPO_FROM}.git
git clone git@github.com:${GITHUB_USER}/${REPO_TO}.git

cd ${REPO_FROM}
git filter-branch --subdirectory-filter "${DIRECTORY_FROM}" -- --all
mkdir -p "${DIRECTORY_TO}"
mv * "${DIRECTORY_TO}"
git add .
git commit -m "Moving files to new location"

cd ../${REPO_TO}
git checkout -b "${REPO_TO_NEW_BRANCH}"
git remote add repoFrom "../${REPO_FROM}"
git pull repoFrom "${REPO_FROM_BRANCH}" --allow-unrelated-histories
git remote rm repoFrom
git checkout master
git merge --no-ff "${REPO_TO_NEW_BRANCH}"
# git branch -d "${REPO_TO_NEW_BRANCH}"
```

# How do you squash commits into one patch with git format-patch?

With the help of [StackOverflow](http://stackoverflow.com/questions/616556/how-do-you-squash-commits-into-one-patch-with-git-format-patch).

~~~ Bash
[(master)]$ git checkout -b tmpsquash
Switched to a new branch "tmpsquash"

[(tmpsquash)]$ git merge --squash newlines
Updating 4d2de39..b6768b2
Fast forward
Squash commit -- not updating HEAD
 test.txt |    2 ++
 1 files changed, 2 insertions(+), 0 deletions(-)

[(tmpsquash)]$ git commit -a -m "My squashed commits"
[tmpsquash]: created 75b0a89: "My squashed commits"
 1 files changed, 2 insertions(+), 0 deletions(-)

[(tmpsquash)]$ git format-patch master
0001-My-squashed-commits.patch
~~~

# Moving branch pointer without checkout

As stated [here](http://stackoverflow.com/questions/5471174/git-move-branch-pointer-to-different-commit-without-checkout) we can use:

```
git branch -f branch-name new-tip-commit
```

# Git patch in different directory

As suggested in [How to apply a Git patch to a file with a different name and path? - Stack Overflow](https://stackoverflow.com/questions/16526321/how-to-apply-a-git-patch-to-a-file-with-a-different-name-and-path):

```
git format-patch --relative <commit>
```

It can be something like: `git format-patch --relative -10 HEAD`

Then apply pointing to the new directory:

```
git am --directory the/directory/ 0001-patchfile.patch
```

# Merging two repositories

How to merge a repository (`REPO_TO_DELETE`) into another (`REPO_TO_KEEP`):

```bash
#!/usr/bin/env bash

GITHUB_USER=""
REPO_TO_KEEP=""
REPO_TO_DELETE=""

git clone git@github.com:${GITHUB_USER}/${REPO_TO_KEEP}.git
git clone git@github.com:${GITHUB_USER}/${REPO_TO_DELETE}.git

cd ${REPO_TO_DELETE}
mkdir ${REPO_TO_DELETE}
mv * ${REPO_TO_DELETE}
git add .
git commit -a -m "Moving ${REPO_TO_DELETE} project into its own directory

So we can merge into the ${REPO_TO_KEEP} repository with less problems"

cd ../${REPO_TO_KEEP}
git remote add ${REPO_TO_DELETE} ../${REPO_TO_DELETE}
git fetch ${REPO_TO_DELETE}
git checkout -b merging_${REPO_TO_DELETE}_repo
git merge --allow-unrelated-histories ${REPO_TO_DELETE}/master

# Solve any conflicts that appear
# git commit
```

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

# How to work with a encrypted repository using git-crypt

[git-crypt repo](https://github.com/AGWA/git-crypt/)

- Clone it
    `git clone git@github.com:AGWA/git-crypt.git`
- Install it
    - `make && make install`
    - Make sure that you have `g++` installed

## To generate a key

```
cd $(mktemp -d); git init; git-crypt init; git-crypt export-key ~/git-crypt.key; cd
```

Don't forget to secure your key in a safe place.

## Configuring your repository to encrypt all files

In a repository create and commit a `.gitattributes` file with this content:

```
/** filter=git-crypt diff=git-crypt
/public/** !filter !diff
.gitattributes !filter !diff
.gitmodules !filter !diff
.git* !filter !diff
```

After the `.gitattributes` file is commited, do the following steps:

```
git-crypt unlock ~/git-crypt.key
git-crypt status -f
```

Then commit your changes.

On all new clones of this repository you'll need to unlock it:

```
git-crypt unlock ~/git-crypt.key
...do your stuff...
git-crypt status # to check if all files are encrypted (-f to fix)
git add .
git commit
git push
```

**CAUTION**

When backuping the repository do not copy it to another place, `clone` it.
When `git-crypt` `unlocks` a repository it creates a git-crypt directory inside `.git` which contains your key.

**CAUTION**

# References

- [encryption - git encrypt/decrypt remote repository files while push/pull - Stack Overflow](https://stackoverflow.com/questions/2456954/git-encrypt-decrypt-remote-repository-files-while-push-pull/45047100#45047100)
- [Storing sensitive data in a git repository using git-crypt | Technology | Blog | Twinbit](http://www.twinbit.it/en/blog/storing-sensitive-data-git-repository-using-git-crypt)
- [Encrypting all by default, then exclude... · Issue #76 · AGWA/git-crypt · GitHub](https://github.com/AGWA/git-crypt/issues/76)

# Exporting your repository

To export your repository to another person you have two options: `git archive` and `git bundle`

## Git archive

With `git archive` you export the repository but only the files on `HEAD`, without the history.

At the repository's root:

```
git archive -o ${PWD##*/}-$(date +%Y%m%d).zip HEAD
```

## Git bundle

With `git bundle` you export the repository including the entire history.

At the repository's root, create the bundle file:

```
git bundle create ${PWD##*/}-$(date +%Y%m%d).bundle --all
```

After sending this file to another person s/he can clone it and start analyzing it:

```
git clone repositry.bundle
```

﻿Dicas gerais sobre o git.

## How can I determine the url that a local git repo was originally cloned from?

From [How can I determine the url that a local git repo was originally cloned from?](http://stackoverflow.com/questions/4089430/how-can-i-determine-the-url-that-a-local-git-repo-was-originally-cloned-from) :

~~~ Bash
# If referential integrity has been broken:
git config --get remote.origin.url
# If referential integrity is intact:
git remote show origin
~~~

## Problema ao se fazer push para um repositório "non-bare"

~~~ Bash
$ git push
Counting objects: 20, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (17/17), done.
Writing objects: 100% (18/18), 2.69 KiB | 0 bytes/s, done.
Total 18 (delta 9), reused 0 (delta 0)
remote: error: refusing to update checked out branch: refs/heads/master
remote: error: By default, updating the current branch in a non-bare repository
remote: error: is denied, because it will make the index and work tree inconsistent
remote: error: with what you pushed, and will require 'git reset --hard' to match
remote: error: the work tree to HEAD.
remote: error:
remote: error: You can set 'receive.denyCurrentBranch' configuration variable to
remote: error: 'ignore' or 'warn' in the remote repository to allow pushing into
remote: error: its current branch; however, this is not recommended unless you
remote: error: arranged to update its work tree to match what you pushed in some
remote: error: other way.
remote: error:
remote: error: To squelch this message and still keep the default behaviour, set
remote: error: 'receive.denyCurrentBranch' configuration variable to 'refuse'.
To /home/hugo/Dropbox/workplace/eve_grabber
 ! [remote rejected] master -> master (branch is currently checked out)
error: failed to push some refs to '/home/hugo/Dropbox/workplace/eve_grabber'
~~~

Com a ajuda do [stack overflow](http://stackoverflow.com/questions/2816369/git-push-error-remote-rejected-master-master-branch-is-currently-checked):

At the remote repo:

~~~ Bash
git config --bool core.bare true
~~~

Then delete all the files except .git in that folder. And then you will be able to perform git push to the remote repository without any errors.

## Creating a bare repository from a normal one

From [version control - How to convert a normal Git repository to a bare one? - Stack Overflow](https://stackoverflow.com/a/2200662):

```
cd repo
mv .git ../repo.git # renaming just for clarity
cd ..
rm -fr repo
cd repo.git
git config --bool core.bare true
```

## Gerando Patches

Dica do [How to create and apply a patch with Git](https://ariejan.net/2009/10/26/how-to-create-and-apply-a-patch-with-git/):

### Para se gerar o patch:

~~~ Bash
git format-patch against_branch_name --stdout > name_of.patch
~~~

Outra forma de se gerar o patch é com `git diff`:

~~~ Bash
git diff sha1..sha2 > name_of.patch
~~~

### Para se aplicar o patch:

~~~ Bash
git apply --stat name_of.patch
~~~
Assim, ele só irá mostrar como será a aplicação do patch, não aplicará de verdade.

Para realmente se aplicar o patch devemos chamar

~~~ Bash
git am name_of.patch
~~~

## Submodules

Atualizando todos os submodules:

~~~ Bash
git submodule update --init --recursive
~~~

## How to grep (search) committed code in the git history?

~~~ Bash
git grep [regexp] $(git rev-list --all)
~~~

or

~~~ Bash
git log -S"search term"
~~~

## How to grep commit messages

```bash
git log --grep "search term"
```

On all branches

```
git log --all --grep="search term"
```

# Add only non-whitespace changes

Suponha que o seu editor tenha feito mudanças nos whitespaces/tabs, para apenas adicionar ao staging as mudanças de verdade:

    git diff -w --no-color <arquivo> | git apply --cached --ignore-whitespace

# Temporarily stop watching a file

    git update-index --assume-unchanged <file>

Now the file, even if modified, will not appear in a git status.

To undo it:

    git update-index --no-assume-unchanged <file>

To list all the files that are "hidden" in this way:

    git ls-files -v | grep '^h'

With the help of [StackOverflow](http://stackoverflow.com/questions/17195861/undo-git-update-index-assume-unchanged-file)

# Discovering when text was modified in a file

With the help of [StackOverflow](http://stackoverflow.com/questions/12591247/find-when-line-was-deleted):

    git log -c -S'missingtext' /path/to/file

# Pushing to a Remote Branch with a Different Name

```
git push origin local-name:remote-name
```

Tip from https://penandpants.com/2013/02/07/git-pushing-to-a-remote-branch-with-a-different-name/

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

## Get all files that have been modified between two git branches

From [Get all files that have been modified in git branch - Stack Overflow](https://stackoverflow.com/questions/10641361/get-all-files-that-have-been-modified-in-git-branch#10641810):

```
git diff --name-only <notMainDev> $(git merge-base <notMainDev> <mainDev>)
```

An example if you are on a branch and want to compare with master:

```
git diff --name-only HEAD $(git merge-base master HEAD)
```

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

# Changing the whole history

You may want to `rebase` from the beginning of your repository. For doing that the `--root` flag is very useful:

```
git rebase -i --root
```

# Re-creating the first commit

```
git checkout -b new-master
git reset --hard <First commit SHA>
git update-ref -d HEAD
# Now remove all files from stage
git commit --allow-empty
git merge --no-ff <other branch>
```

Using the help from [How to revert initial git commit? - Stack Overflow](https://stackoverflow.com/questions/6632191/how-to-revert-initial-git-commit#answer-6637891)

# Changelog of what has been added to your project since your last release

It’s time to email your mailing list of people who want to know what’s happening in your project.

    $ git shortlog --no-merges base...latest

More info at [The Shortlog](https://git-scm.com/book/en/v2/Distributed-Git-Maintaining-a-Project#The-Shortlog).

# Getting all submodules recursively

    git submodule foreach --recursive git submodule update --init

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

