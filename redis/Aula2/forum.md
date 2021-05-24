# Fórum

Consigo inserir no meio da lista? Exemplo entre o segundo e o terceiro elemento

```

Acredito que o LINSERT pode ser útil nesse sentido: https://redis.io/commands/linsert 

E o LINDEX também: https://redis.io/commands/lindex

```


Observei que o Redis apresenta caracteres "estranhos" quando inserimos palavras acentuadas.

```

Pesquisei e descobri que ele armazena de forma correta, mas exibe dessa forma.

Para que ele exiba a palavra acentuada basta inicializar o client com o comando --raw

redis-cli --raw

--raw Use raw formatting for replies (default when STDOUT is not a tty).

Fica a dica se mais alguém tiver esse problema.

```