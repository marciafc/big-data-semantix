# Fórum

Referente ao "Exercícios - Consultas 2"

Na questão 2.B encontrei uma outra formar de fazer sem chamar o 'limit' com a seguinte chamada:

```
mongo> db.produto.find({}, 1, 3).sort({nome: 1, qtd: 1})

Onde: O primeiro parâmetro é o filtro;
      O segundo parâmetro é a projeção (campos ou atributos a serem exibidos) e neste caso equivale a true;
      O terceiro parâmetro a quantidade de registros retornados, no caso são 3 registros.

 
{ "_id" : 1, "nome" : "cpu i5", "qtd" : "15" }
{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }
{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }


Obs: A forma para chamar o limit é

db.produto.find({}).sort({nome: 1, qtd: 1}).limit(3)


{ "_id" : 1, "nome" : "cpu i5", "qtd" : "15" }
{ "_id" : 4, "nome" : "hd externo", "qtd" : 20, "descricao" : { "conexao" : "USB", "armazenamento" : "500GB", "so" : [ "Windows 10", "Windows 8", "Windows 7" ] } }
{ "_id" : 2, "nome" : "memória ram", "qtd" : 10, "descricao" : { "armazenamento" : "8GB", "tipo" : "DDR4" } }

```

Conversão de SQL para mongo. Ajuda em algumas coisas a entender algumas operações.

```
Site: https://www.site24x7.com/tools/sql-to-mongodb.html

Client que tem conversão de SQL para mongo: https://nosqlbooster.com/

```