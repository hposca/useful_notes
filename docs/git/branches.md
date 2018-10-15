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

Este lindo comando fará, segundo a documentação, como se o repositório fosse a raíz do repositório e todo o resto, desconhecido.

Já no seu "repositório B":

~~~ Bash
git checkout -b new_branch
git remote add branchA [caminhoParaORepositorioA]
git pull branchA master # Ao invés de master você pode colocar a branch que estava sendo utilizada no repositório A
git remote rm branchA
git checkout master
git merge --no-ff new_branch
git branch -d new_branch
~~~

Pronto! Agora em seu "repositório B" você terá todo o conteúdo do diretório que estava no "repositório A", e com o histórico preservado!
E isto tudo só foi possível devido às dicas [deste site](http://gbayer.com/development/moving-files-from-one-git-repository-to-another-preserving-history/)!
[This gist](https://gist.github.com/whistler/de34b77aba2221ed8b2e) has a very similar strategy based on the site above.

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
