# Criando um servidor HTTP para disponibilizar arquivos rapidamente

Para se criar um servidor HTTP para disponibilizar arquivos rapidamente, em um terminal:

    python -m SimpleHTTPServer 7777

# Vendo os atributos de um objeto

Seguindo a [dica do StackOverflow](http://stackoverflow.com/questions/25150955/python-iterating-through-object-attributes) conseguimos ver os atributos e respectivos valores de qualquer obejto em python:

    for attr, value in objeto.__dict__.iteritems():
        print attr, value
