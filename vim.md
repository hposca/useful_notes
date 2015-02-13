# Dicas gerais para vim

# Abrindo um arquivo em hexadecimal

Depois de abrir o arquivo, para se ver o hex dump dele:

~~~ Bash
:%!xxd
~~~

Depois de editar o hexadecimal, para converter o arquivo devolta ao formato binário:

~~~ Bash
:%!xxd -r
~~~
