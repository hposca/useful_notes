## Utilizando o Apache Benchmark

O `ab` ou `Apache HTTP server benchmarking tool` _"is a tool for benchmarking your Apache Hypertext Transfer Protocol (HTTP) server. It is designed to give you an impression of how your current Apache installation performs. This especially shows you how many requests per second your Apache installation is capable of serving"_.

Para se instalar o `ab` basta executar um

~~~ Bash
sudo apt-get install apache2-utils
~~~

### Exemplos

- Para se fazer 10000 requisições, de 100 em 100 dizendo ser o firefox:

~~~ Bash
ab -n 10000 -c 100 -H "User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:38.0) Gecko/20100101 Firefox/38.0" http://45.55.95.61:3000/
~~~

Para saber todos os parâmetros e opções pode-se ver a [documentação](http://httpd.apache.org/docs/2.4/programs/ab.html).
