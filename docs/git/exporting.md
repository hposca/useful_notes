# Exporting your repository

To export your repository to another person you have two options: `git archive` and `git bundle`

## Git archive

With `git archive` you export the repository but only the files on `HEAD`, without the history.

At the repository's root:

```
git archive -o ${PWD##*/}.zip HEAD
```

## Git bundle

With `git bundle` you export the repository including the entire history.

At the repository's root, create the bundle file:

```
git bundle create ${PWD##*/}.bundle --all
```

After sending this file to another person s/he can clone it and start analyzing it:

```
git clone repositry.bundle
```
