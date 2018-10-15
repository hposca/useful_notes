Dicas gerais sobre o git.

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
