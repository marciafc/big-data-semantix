# Exercícios - Sets

```

1. Deletar a chave "pesquisa:produto"

// Apareceu 0 pq nem existia a chave (se aparecer 1 é pq existia e foi deletada)
del pesquisa:produto
(integer) 0


2. Criar a chave "pesquisa:produto" do tipo set com os seguintes valores: monitor, mouse e teclado

sadd pesquisa:produto monitor mouse teclado
(integer) 3


3. Visualizar a quantidade de valores da chave

scard pesquisa:produto
(integer) 3


4. Retornar todos os elementos da chave

// Set não é ordenado
smembers pesquisa:produto
1) "teclado"
2) "mouse"
3) "monitor"


5. Verificar se existe o valor monitor

sismember pesquisa:produto monitor
(integer) 1

sismember pesquisa:produto monitorr
(integer) 0


6. Remover o valor monitor

SREM pesquisa:produto monitor
(integer) 1

sismember pesquisa:produto monitor
(integer) 0


7. Recuperar um elemento e remove-lo do set

spop pesquisa:produto
"mouse"


8. Criar a chave "pesquisa:desconto" do tipo set com os seguintes valores: memória RAM, monitor, teclado, HD

// Usar aspas quando o valor conter mais de uma palavra
SADD pesquisa:desconto 'memoria RAM' monitor teclado HD
(integer) 4


9. Próximas questões fazem uso dos sets pesquisa:produto e pesquisa:desconto

	- Visualizar a interseção entre os 2 sets
	- Visualizar a diferença entre os 2 sets
	- Criar o set "pesquisa:produto_desconto" com a união entre os 2 sets


// pesquisa:produto
smembers pesquisa:produto
1) "teclado"

// pesquisa:desconto
smembers pesquisa:desconto
1) "teclado"
2) "monitor"
3) "memoria RAM"
4) "HD"

// interseção
SINTER pesquisa:produto pesquisa:desconto
1) "teclado"

// diferença
SDIFF pesquisa:produto pesquisa:desconto
(empty array)

// união
SUNIONSTORE pesquisa:produto_desconto pesquisa:produto pesquisa:desconto 
(integer) 4

SMEMBERS pesquisa:produto_desconto
1) "teclado"
2) "memoria RAM"
3) "monitor"
4) "HD"


```