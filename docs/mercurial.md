# Converting a Mercurial repo to a git repo

- Install the _mercurial-git_ package:

    sudo apt-get install -y mercurial-git

- Add the following to your _~/.hgrc_:

    [extensions]
    hgext.bookmarks =
    hgext.git =

- At the mercurial repo:

    hg bookmark -r default repo

- On another directory create your git repo:

    git init git_repo
    cd git_repo
    git config --bool core.bare true

- At the mercurial repo:

    hg push path/to/your/git_repo

Now your *git_repo* directory is a git bare repository, you can clone from it
