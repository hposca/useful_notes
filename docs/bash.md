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

Ignorando um diretório:

    find . -type f -iname "*.rb" -not -path "*/vendor/*" -exec bash -c 'expand -t 2 "$0" > /tmp/e && mv /tmp/e "$0"' {} \;

Se estivermos em um repositório git podemos garantir que não alteramos nada de
verdade executando um:

    git diff --ignore-all-space

# Converting spaces in filenames to underlines

    for file in *pdf; do mv $file $(echo $file | tr -s ' ' '_'); done

    or

    for file in *pdf; do rename 's/ /_/g' $file; done

# Getting the exit status of a command even when using tee

Even when using `tee` you can use the `${PIPESTATUS}` variable to see what was
the exit code of the executed command.
It can be used to check if the status was `0` so you can continue your command
chain:

    ./my_command.sh | tee -a /tmp/output.log ; test ${PIPESTATUS[0]} -eq 0 && ...
