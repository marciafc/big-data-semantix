# Exercícios - Regex

Consultas com Regex


```

1. Mostrar todos os documentos da collection produto do banco de dados seu nome

> use marcia
switched to db marcia

> show collections
produto


> db.produto.find()
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows 10", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z"), "ts_modificado" : Timestamp(1621917005, 1) }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }


2. Buscar os documentos com o atributo nome,  que contenham a palavra "cpu"

// Sem uso do Regex
> db.produto.find( {nome: "cpu"} )

// Regex
> db.produto.find( {nome: { $regex: "cpu" }} )
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }


// Outra forma com regex
> db.produto.find( {nome: { $regex: /cpu/ }} )
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }


3. Buscar os documentos  com o atributo nome, que começam por "hd" e apresentar os campos nome e qtd

// começam por "hd" 
db.produto.find( {nome: { $regex: /^hd/ }} )

	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }

// começam por "hd" e apresentar os campos nome e qtd
db.produto.find( {nome: { $regex: /^hd/ }}, { nome: 1, qtd: 1 } )

	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20 }
	

db.produto.find( {nome: { $regex: /^hd/ }}, { nome: 1, qtd: 1, _id : 0 } )

	{ "nome" : "hd externo", "qtd" : 20 }


4. Buscar os documentos  com o atributo descricao.armazenamento, que terminam com "GB" ou "gb" e apresentar os campos nome e descricao


// Regex vai buscar GB ou gb => $regex: /gb/i 
// User o 'i'para não sensitive
db.produto.find( {"descricao.armazenamento": { $regex: /gb/i }}, { nome: 1, descricao: 1 } )

	{ "_id" : 2, "nome" : "memória ram", "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 4, "nome" : "hd externo", "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] } }


5. Buscar os documentos  com o atributo nome, que contenha a palavra memória, ignorando a letra "o"

// Sem acento, não traz
> db.produto.find( {nome : { $regex: /memoria/ }} )

// Com acento, traz
> db.produto.find( {nome : { $regex: /memória/ }} )

	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }

// Usando caracter coringa no lugar do 'o'
> db.produto.find( {nome : { $regex: /mem.ria/ }} )

	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }



6. Buscar os documentos  com o atributo qtd  que contenham valores com letras, ao invés de números.

// Não retornou nenhum: todos campos qtd são numéricos
db.produto.find( {qtd : { $regex: /[a-z]/ }} )

// nome com letras
db.produto.find( {nome : { $regex: /[a-z]/ }} )

	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows 10", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z"), "ts_modificado" : Timestamp(1621917005, 1) }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }



7. Buscar os documentos com o atributo descricao.sistema, que tenha exatamente a palavra "Windows"

// Não precisa regex, usar com cautela
> db.produto.find( {"descricao.sistema": { $regex: /^Windows$/ }} )
> db.produto.find( {"descricao.sistema": "Windows"} )


```