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
