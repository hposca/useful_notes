#  Anotações referentes a Bancos de Dados

# PostgreSQL + Amazon RDS

## Tirando um dump de um banco que está na Amazon RDS

Para se facilitar o processo de se tirar o dump precisamos ter uma instância EC2 em pé, que servirá apenas para se tirar o dump.

* Precisamos instalar os pacotes que contêm o `pg_dump`:

~~~ Bash
$ sudo apt-get install -y postgresql-client-9.3 postgresql-client-common
~~~

* Depois, temos que configurar a variável de ambiente que tem a senha do banco de dados:

~~~ Bash
$ export PGPASSWORD="senha do banco"
~~~

* Finalmente, começamos a tirar o dump:

~~~ Bash
$ pg_dump -Fc -h host_para_o_banco -U usuario nome_do_banco | gzip -9 > dump.sql.gz
~~~

## Restaurando o banco

Ao final do processo acima temos um arquivo comprimido. Para restaurar o banco a partir deste arquivo podemos fazer algo como o seguinte:

~~~ Bash
$ gunzip -c dump.sql.gz | pg_restore -U usuario -d nome_da_base
~~~
