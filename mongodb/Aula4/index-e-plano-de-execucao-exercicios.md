# Exercícios - Index

Index e plano de execução

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



2. Criar o index "query_produto" para pesquisar o campo nome do produto em ordem alfabética

// ordem alfabética => 1
> db.produto.createIndex( { nome: 1 }, { name: "query_produto"  } )
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}

// "numIndexesBefore" : 1 -> tinha um index (o _id )
// "numIndexesAfter" : 2  -> agora tem dois (o _id e o nome)


3. Pesquisar todos os índices da collection produto

db.produto.getIndexes()

[
	{
		"v" : 2,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_"
	},
	{
		"v" : 2,
		"key" : {
			"nome" : 1
		},
		"name" : "query_produto"
	}
]


4. Pesquisar todos os documentos da collection produto

db.produto.find()

{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows 10", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z"), "ts_modificado" : Timestamp(1621917005, 1) }
{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }


5. Visualizar o plano de execução do exercício 4

// db.produto.explain().find() -> também funciona
// winningPlan: plano vencedor
db.produto.find().explain()

{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "marcia.produto",
		"indexFilterSet" : false,
		"parsedQuery" : {
			
		},
		"queryHash" : "8B3D4AB8",
		"planCacheKey" : "8B3D4AB8",
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"serverInfo" : {
		"host" : "10ffe503c15c",
		"port" : 27017,
		"version" : "4.4.6",
		"gitVersion" : "72e66213c2c3eab37d9358d5e78ad7f5c1d0d0d7"
	},
	"ok" : 1
}


6. Pesquisar todos os documentos da collection produto, com uso da index "query_produto"

// Agora está vindo ordenado pelo 'nome' em ordem alfabética
db.produto.find().hint( {nome: 1} )

	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows 10", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z"), "ts_modificado" : Timestamp(1621917005, 1) }



// Usando o nome do INDEX
db.produto.find().hint( "query_produto" )

	{ "_id" : 1, "nome" : "cpu i7", "qtd" : 15 }
	{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB 3.0", "armazenamento" : "500GB", "sistema" : [ "Windows 10", "Windows 8", "Windows 7", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z") }
	{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }
	{ "_id" : 3, "nome" : "mouse", "qtd" : 30, "descricao" : { "conexao" : "USB 3.0", "sistema" : [ "Windows 10", "Linux" ] }, "data_modificacao" : ISODate("2021-05-25T03:54:08.289Z"), "ts_modificado" : Timestamp(1621917005, 1) }



7. Visualizar o plano de execução do exercício 7


// Plano vencedor é IXSCAN (e não COLLSCAN)
> db.produto.find().hint( {nome: 1} ).explain()


{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "marcia.produto",
		"indexFilterSet" : false,
		"parsedQuery" : {
			
		},
		"queryHash" : "8B3D4AB8",
		"planCacheKey" : "8B3D4AB8",
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"nome" : 1
				},
				"indexName" : "query_produto",
				"isMultiKey" : false,
				"multiKeyPaths" : {
					"nome" : [ ]
				},
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 2,
				"direction" : "forward",
				"indexBounds" : {
					"nome" : [
						"[MinKey, MaxKey]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"serverInfo" : {
		"host" : "10ffe503c15c",
		"port" : 27017,
		"version" : "4.4.6",
		"gitVersion" : "72e66213c2c3eab37d9358d5e78ad7f5c1d0d0d7"
	},
	"ok" : 1
}


// Usando o nome do INDEX
> db.produto.find().hint( "query_produto" ).explain()
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "marcia.produto",
		"indexFilterSet" : false,
		"parsedQuery" : {
			
		},
		"queryHash" : "8B3D4AB8",
		"planCacheKey" : "8B3D4AB8",
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"nome" : 1
				},
				"indexName" : "query_produto",
				"isMultiKey" : false,
				"multiKeyPaths" : {
					"nome" : [ ]
				},
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 2,
				"direction" : "forward",
				"indexBounds" : {
					"nome" : [
						"[MinKey, MaxKey]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"serverInfo" : {
		"host" : "10ffe503c15c",
		"port" : 27017,
		"version" : "4.4.6",
		"gitVersion" : "72e66213c2c3eab37d9358d5e78ad7f5c1d0d0d7"
	},
	"ok" : 1
}


8. Remover o index "query_produto"

// OU db.produto.dropIndex({nome: 1})
db.produto.dropIndex("query_produto")

	{ "nIndexesWas" : 2, "ok" : 1 }


9. Pesquisar todos os índices da collection produto

// Agora só tem o _id como index
db.produto.getIndexes()

	[ { "v" : 2, "key" : { "_id" : 1 }, "name" : "_id_" } ]

```

## Referência

  - [Documentação MongoDB - Hint](https://docs.mongodb.com/manual/reference/operator/meta/hint/)