# Dicas gerais do git

## [How can I determine the url that a local git repo was originally cloned from?](http://stackoverflow.com/questions/4089430/how-can-i-determine-the-url-that-a-local-git-repo-was-originally-cloned-from)

~~~ Bash
 # If referential integrity has been broken:
$ git config --get remote.origin.url
# If referential integrity is intact:
$git remote show origin
~~~

## Desfazendo o último commit

~~~ Bash
$ git reset --soft HEAD~1
~~~

## Desfazendo o primeiro commit de um repositório

~~~ Bash
$ git update-ref -d HEAD
~~~

## [Undo a git merge](http://stackoverflow.com/questions/2389361/undo-a-git-merge)

~~~ Bash
$ git reset --hard commit_sha
# ou
$ git reset --hard HEAD~5 #will get you back 5 commits.
# ou
$ git reset --merge ORIG_HEAD
~~~

## [Showing which files have changed between git branches](http://stackoverflow.com/questions/822811/showing-which-files-have-changed-between-git-branches)

~~~ Bash
$ git diff --name-status master..branchName
~~~

## [Git rename local git branch](http://stackoverflow.com/questions/6591213/rename-local-git-branch)

~~~ Bash
$ git branch -m <oldname> <newname>
# or, if you want to rename the current branch:
$ git branch -m <newname>
~~~

## [Git log all branches](http://www.lornajane.net/posts/2014/git-log-all-branches)

~~~ Bash
$ git log --oneline --graph --decorate --all
~~~

## [Move commits from one branch to another](http://effectif.com/git/move-commit-from-one-branch-to-another)

~~~ Bash
$ git checkout -b some-feature
$ git checkout master
$ git reset --hard HEAD^
$ git checkout some-feature
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
$ git config --bool core.bare true
~~~

Then delete all the files except .git in that folder. And then you will be able to perform git push to the remote repository without any errors.

# Gerando Patches

Dica do [How to create and apply a patch with Git](https://ariejan.net/2009/10/26/how-to-create-and-apply-a-patch-with-git/):

## Para se gerar o patch:

~~~ Bash
$ git format-patch against_branch_name --stdout > name_of.patch
~~~

Outra forma de se gerar o patch é com `git diff`:

~~~ Bash
$ git diff sha1..sha2 > name_of.patch
~~~

## Para se aplicar o patch:

~~~ Bash
$ git apply --stat name_of.patch
~~~
Assim, ele só irá mostrar como será a aplicação do patch, não aplicará de verdade.

# Trabalhando com Branches

## [Move the most recent commit(s) to a new branch with Git](http://stackoverflow.com/questions/1628563/move-the-most-recent-commits-to-a-new-branch-with-git)

~~~ Bash
$ git branch newbranch
$ git reset --hard HEAD~3 # Go back 3 commits. You *will* lose uncommitted work.
$ git checkout newbranch
~~~

## Moving to an existing branch

~~~ Bash
$ git checkout existingbranch
$ git merge master
$ git checkout master
$ git reset --hard HEAD~3 # Go back 3 commits. You *will* lose uncommitted work.
$ git checkout existingbranch
~~~

## Vendo a hierarquia das branches

~~~ Bash
$ git log --oneline --decorate --color --graph --all --simplify-by-decoration
# ou
$ git log --graph --simplify-by-decoration --pretty=format:'%d' --all
~~~

## Adicionando branches remotas

~~~ Bash
$ git push origin <branchName>
~~~

## Apagando branches remotas

~~~ Bash
$ git push origin --delete <branchName>
~~~

## Movendo conteúdo de uma branch para outra

De vez em quando não podemos simplesmente mergear uma branch com a outra, pois isso pode trazer algumas modificações indesejadas.
Uma coisa que podemos fazer é crirar um conjunto de patches e transportá-los para a branch onde queremos aplicar as mudanças que foram feitas na outra branch.

**Gerando os patches:**

~~~ Bash
$ git checkout <SHA-1 do commit que você quer considerar como último>
$ git format-patch <SHA-1 do commit que você quer utilizar como primeiro>
# Serão gerados vários arquivos .patch
~~~

**Aplicando os patches:**

~~~ Bash
# Primeiramente, mude seu workspace para a branch onde você quer aplicar as mudanças
# e.g.: git checkout production

$ git am *.patch
# No caso que enfrentei tinha alguns problemas de espaço, final de linha, etc. então utilizei:
$ git am -3 --ignore-whitespace *.patch
~~~

Caso algum dos patches falhe na sua aplicação basta editar o arquivo, adicioná-lo com `git add` e dar um `git am --continue`
