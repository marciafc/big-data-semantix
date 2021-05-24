# Exercícios - Listas


```
1. Criar a chave "views:ultimo_usuario" e insira nesta ordem os seguintes valores como lista:

	- Final da lista "Joao"
	- Final da lista "Ana"
	- Inicio da lista "Carlos"
	- Final da lista "Carol"
	
// Pode inserir mais de um valor no mesmo comando	
127.0.0.1:6379> RPUSH views:ultimo_usuario Joao Ana
(integer) 2	
	
127.0.0.1:6379> LPUSH views:ultimo_usuario Carlos
(integer) 3


127.0.0.1:6379> RPUSH views:ultimo_usuario Carol
(integer) 4

	
2. Visualizar todos os elementos da lista

127.0.0.1:6379> LRANGE views:ultimo_usuario 0 -1

1) "Carlos"
2) "Joao"
3) "Ana"
4) "Carol"


3. Visualizar todos os elementos da lista, com exceção do último

127.0.0.1:6379> LRANGE views:ultimo_usuario 0 2
1) "Carlos"
2) "Joao"
3) "Ana"

// Penúltimo é o -2
127.0.0.1:6379> LRANGE views:ultimo_usuario 0 -2
1) "Carlos"
2) "Joao"
3) "Ana"


4. Visualizar o tamanho da lista

127.0.0.1:6379> LLEN views:ultimo_usuario
(integer) 4


5. Redefinir o tamanho da lista, removendo o primeiro elemento (Sem usar o pop)

127.0.0.1:6379> LTRIM views:ultimo_usuario 1 -1
OK


6. Visualizar o tamanho da lista

127.0.0.1:6379> LRANGE views:ultimo_usuario 0 -1
1) "Joao"
2) "Ana"
3) "Carol"


7. Recuperar os elementos da lista da seguinte ordem:

	- Primeiro
	- Último
	- Primeiro com bloqueio de 5 segundos se a lista estiver vazia
	- Primeiro com bloqueio de 5 segundos se a lista estiver vazia


127.0.0.1:6379> LPOP views:ultimo_usuario
"Joao"

127.0.0.1:6379> RPOP views:ultimo_usuario
"Carol"

127.0.0.1:6379> BLPOP views:ultimo_usuario 5
1) "views:ultimo_usuario"
2) "Ana"

127.0.0.1:6379> BLPOP views:ultimo_usuario 5
(nil)
(5.02s)


127.0.0.1:6379> EXISTS views:ultimo_usuario
(integer) 0


```