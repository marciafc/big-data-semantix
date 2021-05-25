# Exercícios - CRUD de Documentos

CRUD de Documentos

```

> use marcia
switched to db marcia


1. Criar a collection teste no banco de dados seu nome

> db.createCollection("teste")
{ "ok" : 1 }


> show collections
produto
teste


2. Inserir o seguinte documento:

	Campo: usuario, valor: Semantix
	Campo: data_acesso, valor: data atual no formato ISODate


> db.teste.insertOne( {usuario: 'Semantix', data_acesso: new Date() } )

	{
		"acknowledged" : true,
		"insertedId" : ObjectId("60ac7fcf22c6c9bc89df7007")
	}


> db.teste.find()

	{ "_id" : ObjectId("60ac7fcf22c6c9bc89df7007"), "usuario" : "Semantix", "data_acesso" : ISODate("2021-05-25T04:40:47.502Z") }
	
	
3. Buscar todos os documentos do ano >= 2020

// Buscando com Date
> db.teste.find( { data_acesso: { $gte: new Date("2020") } } )

	{ "_id" : ObjectId("60ac7fcf22c6c9bc89df7007"), "usuario" : "Semantix", "data_acesso" : ISODate("2021-05-25T04:40:47.502Z") }


// Buscando com ISODate
> db.teste.find( { data_acesso: { $gte: ISODate("2020-01-01") } } )

	{ "_id" : ObjectId("60ac7fcf22c6c9bc89df7007"), "usuario" : "Semantix", "data_acesso" : ISODate("2021-05-25T04:40:47.502Z") }


4. Atualizar o campo "data_acesso" para timestamp com o valor da atualização do usuario Semantix

// Não é necessário $set
> db.teste.updateOne( { usuario: 'Semantix'}, { $currentDate: {data_acesso: { $type: "timestamp" } } } )

	{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }


5. Buscar todos os documentos


> db.teste.find()

	{ "_id" : ObjectId("60ac7fcf22c6c9bc89df7007"), "usuario" : "Semantix", "data_acesso" : Timestamp(1621918042, 1) }


6. Deletar o documento criado pelo id

> db.teste.deleteOne( {  "_id" : ObjectId("60ac7fcf22c6c9bc89df7007") }) 
	
	{ "acknowledged" : true, "deletedCount" : 1 }


7. Deletar a collection

> db
marcia

> show collections
produto
teste

// deletando a collection teste
> db.teste.drop()
true

> show collections
produto


```