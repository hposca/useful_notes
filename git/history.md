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
