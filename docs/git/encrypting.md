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
