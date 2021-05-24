# Exercícios - Hashes


```
1. Deletar a chave "usuario:100"

del usuario:100
(integer) 0


2. Criar uma chave "usuario:100" do tipo hash com a seguinte estrutura

	nome – Augusto
	estado – SP
	views – 10

// hset OU hmset

hmset usuario:100 nome Augusto estado SP views 10
OK


3. Visualizar todas as chaves e valores

HGETALL usuario:100
1) "nome"
2) "Augusto"
3) "estado"
4) "SP"
5) "views"
6) "10"


4. Contar a quantidade de campos

 hlen usuario:100
(integer) 3


5. Visualizar apenas o nome e views

hmget usuario:100 nome views
1) "Augusto"
2) "10"


6. Contar o tamanho do valor do campo nome

HSTRLEN usuario:100 nome
(integer) 7


7. Incrementar em 2 o valor do campo views

HINCRBY usuario:100 views 2
(integer) 12


8. Visualizar apenas os campos

hkeys usuario:100
1) "nome"
2) "estado"
3) "views"


9. Visualizar apenas os valores

HVALS usuario:100
1) "Augusto"
2) "SP"
3) "12"


10. Deletar o campo estado

hdel usuario:100 estado
(integer) 1

HGETALL usuario:100
1) "nome"
2) "Augusto"
3) "views"
4) "12"


```