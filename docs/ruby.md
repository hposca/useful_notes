# Dicas de Ruby e/ou Rails

## Executando um script em ruby

Para se executar um script em ruby, podemos fazer:

~~~ Bash
bundle exec rails runner "eval(File.read 'arquivo.rb')"
~~~

## Analisando logs de ambientes

    gem install request-log-analyzer

    request-log-analyzer -f rails3 --output html --file report.html production.log

**Referências:**

- [Get S3 request statistics in four simple steps - Jayway](http://www.jayway.com/2013/02/14/get-s3-request-statistics-in-four-simple-steps/)
- [Teaser check failed · Issue #181 · wvanbergen/request-log-analyzer · GitHub](https://github.com/wvanbergen/request-log-analyzer/issues/181)

# Running Rspec tests automatically

With the use of the [guard-rspec gem](https://github.com/guard/guard-rspec) and the help of this [StackOverflow question](http://stackoverflow.com/questions/17974421/automatically-run-rspec-when-plain-old-ruby-not-rails-files-change) we configured the use of `guard`, to watch for modifications to our files and run `rspec` when something changes.

To use guard, simply run `bundle exec guard` inside the container, in the `/app` directory, and be happy!
