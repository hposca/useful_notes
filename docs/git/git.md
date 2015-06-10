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

Para realmente se aplicar o patch devemos chamar

~~~ Bash
$ git am name_of.patch
~~~

# Trabalhando com Branches


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
$ git filter-branch --subdirectory-filter <oDitoCujoDoDiretorioQueQueremosSeparar> -- --all
~~~
Este lindo comando fará, segundo a documentação, como se o repositório fosse a raíz do repositório e todo o resto, desconhecido.

Já no seu "repositório B":
~~~ Bash
$ git remote add branchA <caminhoParaORepositorioA>
$ git pull branchA master # Ao invés de master você pode colocar a branch que estava sendo utilizada no repositório A
$ git remote rm branchA
~~~

Pronto! Agora em seu "repositório B" você terá todo o conteúdo do diretório que estava no "repositório A", e com o histórico preservado!
E isto tudo só foi possível devido às dicas [deste site](http://www.google.com/url?q=http%3A%2F%2Fgbayer.com%2Fdevelopment%2Fmoving-files-from-one-git-repository-to-another-preserving-history%2F&sa=D&sntz=1&usg=AFrqEzd245648I-fl6TPK2YXtsyvjdMGLw)!

# Submodules

Atualizando todos os submodules:

~~~ Bash
git submodule update --init --recursive
~~~

# How to grep (search) committed code in the git history?

    git grep <regexp> $(git rev-list --all)
