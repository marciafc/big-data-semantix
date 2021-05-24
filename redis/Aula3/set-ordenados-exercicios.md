# Exercícios - Sets Ordenados

```
	1. Deletar a chave "pesquisa:produto"

	// Deletou chave do exercício anterior (set)
	del pesquisa:produto
	(integer) 1


	2. Criar a chave "pesquisa:produto" do tipo set ordenado com os seguintes valores:

		- Valor: monitor, Score: 100
		- Valor: HD, Score: 200
		- Valor: mouse, Score: 10
		- Valor: teclado, Score: 50
				
	zadd pesquisa:produto 100 monitor 200 HD 10 mouse 50 teclado
	(integer) 4	


	O score representa a quantidade de pesquisas feitas para aquele produto na aplicação
	
	1. Visualizar a quantidade de produtos

	ZCARD pesquisa:produto
	(integer) 4


	2. Visualizar todos os produtos do mais pesquisado ao menos pesquisado

	ZREVRANGE pesquisa:produto 0 -1 
	
	1) "HD"
	2) "monitor"
	3) "teclado"
	4) "mouse"
	

	3. Visualizar o rank do produto teclado
	
	zrevrank pesquisa:produto teclado
	(integer) 2


	4. Visualizar o score do produto teclado

	zscore pesquisa:produto teclado
	"50"


	5. Remover o valor HD da chave

	zrem pesquisa:produto HD
	(integer) 1
	

	6. Recuperar e remover do set o produto mais pesquisado
	
	ZPOPMAX pesquisa:produto 
	
	1) "monitor"
	2) "100"


	7. Recuperar e remover do set o produto menos pesquisado

	ZPOPMIN pesquisa:produto
	
	1) "mouse"
	2) "10"


	8. Visualizar todos os produtos

	ZRANGE pesquisa:produto -1 0
	
	1) "teclado"


	ZRANGE pesquisa:produto 0 -1
	
	1) "teclado"
	

```