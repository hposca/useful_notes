# Pegando nome e extensão de um arquivo

Para se pegar o nome de um arquivo:

~~~ Bash
${name%.*}
~~~

Para se pegar a extensão de um arquivo:

~~~ Bash
${name##*.}
~~~

Exemplo:

- Renomear vários arquivos deixando-os com 3 digitos:

~~~ Bash
for name in *.jpg; do
  echo mv $name $(printf %03d.%s ${name%.*} ${name##*.})
done
~~~

# Convertendo tabs para espaços

Misturando algumas [respostas do StackOverflow](http://stackoverflow.com/questions/11094383/how-can-i-convert-tabs-to-spaces-in-every-file-of-a-directory) conseguimos converter todos os tabs (iniciais) em espaços:

    find . -type f -not -iwholename '*.git*' -exec bash -c 'expand -t 2 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;

Se estivermos em um repositório git podemos garantir que não alteramos nada de
verdade executando um:

    git diff --ignore-all-space
