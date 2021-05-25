# Exercícios - Consultas 1

Consulta básica em documentos

```

> use marcia
switched to db marcia


> show collections
produto


1. Mostrar todos os documentos da collection produto

> db.produto.find( {} )

	{ "_id" : 1, "nome" : "cpu i5", "qtd" : "15" }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


2. Pesquisar na collection produto, os documentos com os seguintes atributos:

	a) Nome = mouse

	> db.produto.find( {nome: "mouse"} )

		{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }


	b) Quantidade = 20 e apresentar apenas o campo nome

	// Filtrar Quantidade = 20
	> db.produto.find( {qtd: 20})

		{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


	// Filtrar Quantidade = 20, ocultando o nome
	> db.produto.find( {qtd: 20}, {nome: 0})

		{ "_id" : 4, "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


	// Filtrar Quantidade = 20, exibindo o nome
	> db.produto.find( {qtd: 20}, {nome: 1})
	
		{ "_id" : 4, "nome" : "hd externo" }


	// Filtrar Quantidade = 20, exibindo o nome e ocultando o _id
	> db.produto.find( {qtd: 20}, {nome: 1, _id: 0})
	
		{ "nome" : "hd externo" }

	
	// Filtrar Quantidade = 20, exibindo o nome e ocultando o _id
	// Usar o "$eq" é igual a escrever com "dois pontos" -->  {qtd: { $eq: 20} }  OU {qtd: 20}
	> db.produto.find( {qtd: { $eq: 20} }, {nome: 1, _id: 0})
	
		{ "nome" : "hd externo" }
		
		

	c) Quantidade <= 20 e apresentar apenas os campos nome e qtd

	// Quantidade <= 20 exibindo nome e qtd
	> db.produto.find( {qtd: { $lte: 20} }, {nome: 1, qtd:1})
	
		{ "_id" : 2, "nome" : "memória ram", "qtd" : 10 }
		{ "_id" : 4, "nome" : "hd externo", "qtd" : 20 }


	d) Quantidade entre 10 e 20

	// Quantidade >= 10 e <= 20 
	> db.produto.find( {qtd: { $gte: 10, $lte: 20} } )
	
		{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
		{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }



	e) Conexão = USB e não apresentar o campo _id e qtd

	// Pesquisa campos aninhados, deve estar entre aspas
	// ERRADO: descricao.conexao
	// CORRETO: "descricao.conexao"
	> db.produto.find( { descricao.conexao: "USB"  } )
	
		uncaught exception: SyntaxError: missing : after property id :
		@(shell):1:28
	
	
	// Conexão = USB
	> db.produto.find( { "descricao.conexao": "USB"  } )

		{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
		{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


	// Conexão = USB e não apresentar o campo _id e qtd
	> db.produto.find( { "descricao.conexao": "USB"  }, { _id: 0, qtd: 0 } )

		{ "nome" : "mouse", "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
		{ "nome" : "hd externo", "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


	f) SO que contenha "Windows" ou "Windows 10"
	
	// Só Windows
	> db.produto.find( { "descricao.so" : "Windows"  })
	
		{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }

	// que contenha "Windows" ou "Windows 10"
	> db.produto.find( { "descricao.so" : { $in: ["Windows", "Windows 10"] } })

		{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
		{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }

	// que contenha "Windows" ou "Windows 10", limitando a 1 elemento
	> db.produto.find( { "descricao.so" : { $in: ["Windows", "Windows 10"] } }, 1 )

		{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
		{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


```
