# Exercícios - Atualização de Documentos


Atualização de Documentos

```

> use marcia
switched to db marcia

> show collections
produto


1. Mostrar todos os documentos da collection produto do banco de dados seu nome

> db.produto.find()

{ "_id" : 1, "nome" : "cpu i5", "qtd" : "15" }
{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


2. Atualizar o valor do campo nome para "cpu i7" do id 1

> db.produto.updateOne( { _id : 1}, { $set: { nome: "cpu i7" } }  )

{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


> db.produto.find()

{ "_id" : 1, "nome" : "cpu i7", "qtd" : "15" }
{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
{ "_id" : 3, "nome" : "mouse", "qtd" : 50, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }


3. Atualizar o atributo quantidade para o tipo inteiro do id: 1

> db.produto.findOne( { _id: 1 })
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : "15" }

> db.produto.updateOne({ _id: 1 }, { $set: {"qtd" : 15} })
	{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


// Agora qtd é INTEIRO
> db.produto.findOne( { _id: 1 })
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }


4. Atualizar o valor do campo qtd para 30 de todos os documentos, com o campo qtd >= 30

db.produto.updateMany({ qtd : { $gte: 30} }, { $set: {"qtd" : 30} })
	{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


db.produto.find()

	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB", "so" : [ "Windows", "Mac", "Linux" ] } }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }



5. Atualizar o nome do campo "descricao.so" para "descricao.sistema" de todos os documentos

// Como são todos documentos, o primeiro argumento (filtro) fica vazio
// "matchedCount" : 4 -> find sem filtro, busca todos
// "modifiedCount" : 2 -> só modificou dois
> db.produto.updateMany( {}, { $rename: {"descricao.so" : "descricao.sistema"}} )
	{ "acknowledged" : true, "matchedCount" : 4, "modifiedCount" : 2 }


> db.produto.find()
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB", "sistema" : [ "Windows", "Mac", "Linux" ] } }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7" ] } }
 


6. Atualizar o valor do campo descricao.conexao para "USB 2.0" de todos os documentos, com o campo descricao.conexão = "USB"

> db.produto.updateMany( {"descricao.conexao" : "USB"}, { $set : {"descricao.conexao" : "USB 2.0"}} )
	{ "acknowledged" : true, "matchedCount" : 2, "modifiedCount" : 2 }


> db.produto.find()
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 2.0", "sistema" : [ "Windows", "Mac", "Linux" ] } }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 2.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7" ] } }
 


7. Atualizar o valor do campo descricao.conexao para "USB 3.0" de todos os documentos, com o campo descricao.conexao = "USB 2.0" e
adicionar o campo "data_modificacao", com a data da atualização dos documentos

> db.produto.updateMany( {"descricao.conexao" : "USB 2.0"}, { $set : {"descricao.conexao" : "USB 3.0"}, $currentDate: { data_modificacao: { $type:  "date"  } } })
	{ "acknowledged" : true, "matchedCount" : 2, "modifiedCount" : 2 }


> db.produto.find()
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows", "Mac", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }



8. Atualizar um dos elementos do array descricao.sistema de "Windows" para "Windows 10" do id 3

db.produto.find()
	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows", "Mac", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }


// DICA: Fazer o find primeiro
db.produto.find( { _id: 3, "descricao.sistema": "Windows" } )
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows", "Mac", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }


// Garantir que só vai retornar uma posição do array
// CUIDADO: "descricao.sistema.$" (importante informar o DÓLAR para dizer que é o primeiro elemento que atender a condição do filtro)
// Se não usar o .$ indicando a posição, vai alterar TODO o array
> db.produto.updateOne( { _id: 3, "descricao.sistema": "Windows" }, { $set : {"descricao.sistema.$" : "Windows 10"}  } )
	{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


> db.produto.find( { _id: 3 })
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows 10", "Mac", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }



9. Acrescentar o valor "Linux" no array descricao.sistema do id 4

// Se usar $set no lugar de $push, iria ficar só o 'Linux' (e os demais iriam sumir)
> db.produto.updateOne( { _id: 4 }, { $push:  {"descricao.sistema": "Linux"} } )

	{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }

> db.produto.find( { _id: 4 } )

	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }


10. Remover o valor "Mac" no array descricao.sistema do id 3 e adicionar o campo "ts_modificacao", com o timestamp da atualização dos documentos

> db.produto.updateOne( { _id: 3 }, { $pull:  {"descricao.sistema": "Mac"}, $currentDate: { ts_modificado: { $type : "timestamp"} } } )

	{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


> db.produto.find( { _id: 3 } )

	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows 10", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z"), "ts_modificado" : Timestamp(1621917005, 1) }



```