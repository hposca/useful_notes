# Pegando nome e extensão de um arquivo

Para se pegar o nome de um arquivo:

~~~ Bash
    ${name%.*}
~~~

Para se pegar a extensão de um arquivo:

~~~ Bash
    ${name##*.}
~~~

Assim podemos, por exemplo, renomear vários arquivos deixando-os com 3 digitos:

~~~ Bash
    for name in *.jpg; do
        echo mv $name $(printf %03d.%s ${name%.*} ${name##*.})
    done
~~~
