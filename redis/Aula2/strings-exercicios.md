# Exercícios - Strings


docker ps

docker ps -a

docker-compose stop

docker-compose start

# Se tiver atualização no yml, vai atualizar e subir

	docker-compose up -d

# Acessando container redis

	docker exec -it redis bash

# Acessando redis-cli
	
	redis-cli

```

1. Criar a chave "usuario:nome" e atribua o valor do seu nome

127.0.0.1:6379> set usuario:nome Marcia
OK

2. Criar a chave "usuario:sobrenome" e atribua o valor do seu sobrenome

127.0.0.1:6379> set usuario:sobrenome Castagna
OK

3. Busque a chave "usuario:nome"

127.0.0.1:6379> get usuario:nome
"Marcia"


4. Verificar o tamanho da chave "usuario:nome"

127.0.0.1:6379> strlen usuario:nome
(integer) 6


5. Verificar o tipo da chave "usuario:sobrenome"

127.0.0.1:6379> type usuario:sobrenome
string


6. Criar a chave "views:qtd" e atribua o valor 100

127.0.0.1:6379> SET views:qtd 100
OK


7. Incremente o valor em 10 da chave "views:qtd"

127.0.0.1:6379> incrby views:qtd 10
(integer) 110


8. Busque a chave "views:qtd"

127.0.0.1:6379> GET views:qtd
"110"


9. Deletar a chave "usuario:sobrenome"

127.0.0.1:6379> del usuario:sobrenome
(integer) 1

127.0.0.1:6379> EXISTS usuario:nome
(integer) 1

127.0.0.1:6379> EXISTS usuario:sobrenome
(integer) 0


10. Verificar se a chave "usuario:sobrenome" existe

127.0.0.1:6379> EXISTS usuario:sobrenome
(integer) 0


11. Definir um tempo de 3600 segundos para a chave "views:qtd" ser removida

127.0.0.1:6379> EXPIRE views:qtd 3600
(integer) 1


12. Verificar quanto tempo falta para a chave "views:qtd" ser removida

127.0.0.1:6379> ttl views:qtd
(integer) 3582

127.0.0.1:6379> ttl views:qtd
(integer) 3579

127.0.0.1:6379> ttl views:qtd
(integer) 3578


13. Verificar a persistência da chave "usuario:nome"

# Retornou -1 pois está sempre ativa
127.0.0.1:6379> ttl usuario:nome
(integer) -1


14. Definir para a chave "views:qtd" ter persistência para sempre

127.0.0.1:6379> PERSIST views:qtd
(integer) 1

// Agora está sempre ativa (ou seja, -1)
127.0.0.1:6379> ttl views:qtd
(integer) -1


```