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
