# Exercícios - Comandos Básicos

Comando Básicos para BD, Collections e Documentos

```
$ docker-compose up -d

$ docker exec -it mongo bash

root@2a7938cd311e:/# mongo


1. Criar o banco de dados com seu nome.

> use marcia
switched to db marcia


2. Listar os banco de dados.

// O banco de dados 'marcia' só aparece quando criar uma collection
> show dbs

admin   0.000GB
config  0.000GB
local   0.000GB


3. Criar a collection produto no bd com seu nome.

> db.createCollection('produto')

{ "ok" : 1 }


4. Listar os banco de dados.

> show dbs

admin   0.000GB
config  0.000GB
local   0.000GB
marcia  0.000GB


5. Listar as collections.

> show collections

produto


6. Inserir os seguintes documentos na collection produto:

	_id: 1, "nome": “cpu i5", "qtd": "15“
	
	_id: 2, nome: “memória ram", qtd: 10, descricao: {armazenamento: “8GB”, tipo:“DDR4“}
	
	_id: 3, nome: "mouse", qtd: 50, descricao: {conexao: “USB”, so: [“Windows”, “Mac”, “Linux“]}
	
	_id: 4, nome: “hd externo", "qtd": 20, descricao: {conexao: “USB”, armazenamento: “500GB”, so: [“Windows 10”, “Windows 8”, “Windows 7”]}
	
	
> db.produto.insertOne( { _id: 1, "nome": "cpu i5", "qtd": "15"} )
{ "acknowledged" : true, "insertedId" : 1 }

> db.produto.insertOne( { _id: 2, nome: "memória ram", qtd: 10, descricao: {armazenamento: "8GB", tipo:"DDR4"}} )
{ "acknowledged" : true, "insertedId" : 2 }

> db.produto.insertOne( { _id: 3, nome: "mouse", qtd: 50, descricao: {conexao: "USB", so: ["Windows", "Mac", "Linux"]} } )
{ "acknowledged" : true, "insertedId" : 3 }

> db.produto.insertOne( { _id: 4, nome: "hd externo", "qtd": 20, descricao: {conexao: "USB", armazenamento: "500GB", so: ["Windows 10", "Windows 8", "Windows 7"]} } )
{ "acknowledged" : true, "insertedId" : 4 }	

	
7. Mostrar todos os documentos.

db.produto.find()

{ "_id" : 1, "nome" : "cpu i5", "qtd" : "15" }
{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


```
