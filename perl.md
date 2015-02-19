# Como se instalar pacotes de perl

Como dito [aqui](https://egoleo.wordpress.com/2008/05/19/how-to-install-perl-modules-through-cpan-on-ubuntu-hardy-server/):

``` Bash
$ sudo apt-get install build-essential
```

``` Bash
$ cpan
cpan> make install
cpan> install Bundle::CPAN
# Depois podemos instalar o módulo que quisermos com
cpan> install ...
```
