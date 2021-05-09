# Spark - Exercícios de DataFrame

```
docker-compose up -d

docker ps



# 1. Enviar o diretório local “/input/exercises-data/juros_selic” para o HDFS em "/user/aluno/<nome>/data"

docker exec -it namenode bash

root@namenode:/# hdfs dfs -put /input/exercises-data/juros_selic/  /user/aluno/marcia-castagna/data/


root@namenode:/# hdfs dfs -ls  /user/aluno/marcia-castagna/data/juros_selic

	Found 3 items
	-rw-r--r--   3 root supergroup       7954 2021-05-09 19:36 /user/aluno/marcia-castagna/data/juros_selic/juros_selic
	-rw-r--r--   3 root supergroup      14621 2021-05-09 19:36 /user/aluno/marcia-castagna/data/juros_selic/juros_selic.json
	-rw-r--r--   3 root supergroup      12900 2021-05-09 19:36 /user/aluno/marcia-castagna/data/juros_selic/juros_selic.wsdl


#  2. Criar o DataFrame jurosDF para ler o arquivo no HDFS "/user/aluno/<nome>/data/juros_selic/juros_selic.json"

# Sair do namenode (hdfs)
control + d


# Acessar container do Spark
docker exec -it spark bash

# Inicializar spark shell
spark-shell


spark.<tecla TAB> mostra outros comandos
spark.read.<tecla TAB> mostra outros comandos


# val constante
val jurosDF = spark.read.json("/user/aluno/marcia-castagna/data/juros_selic/juros_selic.json")

	jurosDF: org.apache.spark.sql.DataFrame = [data: string, valor: string]
	
	

# 3. Visualizar o Schema do jurosDF

# No Scala, pode chamar o método COM ou SEM parênteses

scala> jurosDF.printSchema
	root
	 |-- data: string (nullable = true)
	 |-- valor: string (nullable = true)


scala> jurosDF.printSchema()
	root
	 |-- data: string (nullable = true)
	 |-- valor: string (nullable = true)



# 4. Mostrar os 5 primeiros registros do jurosDF

scala> jurosDF.show(5)
	+----------+-----+
	|      data|valor|
	+----------+-----+
	|01/06/1986| 1.27|
	|01/07/1986| 1.95|
	|01/08/1986| 2.57|
	|01/09/1986| 2.94|
	|01/10/1986| 1.96|
	+----------+-----+
	only showing top 5 rows


# Se não informar um valor, o padrão é 20
scala> jurosDF.show

	+----------+-----+
	|      data|valor|
	+----------+-----+
	|01/06/1986| 1.27|
	|01/07/1986| 1.95|
	|01/08/1986| 2.57|
	|01/09/1986| 2.94|
	|01/10/1986| 1.96|
	|01/11/1986| 2.37|
	|01/12/1986| 5.47|
	|01/01/1987|11.00|
	|01/02/1987|19.61|
	|01/03/1987|11.95|
	|01/04/1987|15.30|
	|01/05/1987|24.63|
	|01/06/1987|18.02|
	|01/07/1987| 8.91|
	|01/08/1987| 8.09|
	|01/09/1987| 7.99|
	|01/10/1987| 9.45|
	|01/11/1987|12.92|
	|01/12/1987|14.38|
	|01/01/1988|16.78|
	+----------+-----+
	only showing top 20 rows


# Com o 'false', mostra a informação por completo (caso estivesse com "..." - qdo o conteúdo não cabe)
jurosDF.show(5, false)
	+----------+-----+
	|data      |valor|
	+----------+-----+
	|01/06/1986|1.27 |
	|01/07/1986|1.95 |
	|01/08/1986|2.57 |
	|01/09/1986|2.94 |
	|01/10/1986|1.96 |
	+----------+-----+
	only showing top 5 rows



# 5. Contar a quantidade de registros do jurosDF

scala> jurosDF.count()
	res6: Long = 393
	
	

# 6. Criar o DataFrame jurosDF10 para filtrar apenas os registros com o campo "valor" maior que 10

# Quando coloca o .show() 'jurosDF10' não é um DataFrame, é a visualização, é um Unit
scala> val jurosDF10 = jurosDF.where("valor > 10").show()

	+----------+-----+
	|      data|valor|
	+----------+-----+
	|01/01/1987|11.00|
	|01/02/1987|19.61|
	|01/03/1987|11.95|
	|01/04/1987|15.30|
	|01/05/1987|24.63|
	|01/06/1987|18.02|
	|01/11/1987|12.92|
	|01/12/1987|14.38|
	|01/01/1988|16.78|
	|01/02/1988|18.35|
	|01/03/1988|16.59|
	|01/04/1988|20.25|
	|01/05/1988|18.65|
	|01/06/1988|20.17|
	|01/07/1988|24.69|
	|01/08/1988|22.63|
	|01/09/1988|26.25|
	|01/10/1988|29.79|
	|01/11/1988|28.41|
	|01/12/1988|30.24|
	+----------+-----+
	only showing top 20 rows

	jurosDF10: Unit = ()


# Agora é um Dataset
# Um DataFrame dependendo da operação, pode virar um Dataset (tem um schema)
scala> val jurosDF10 = jurosDF.where("valor > 10")

	jurosDF10: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [data: string, valor: string]

# Agora não aparece Unit
scala> jurosDF10.show(10)

	+----------+-----+
	|      data|valor|
	+----------+-----+
	|01/01/1987|11.00|
	|01/02/1987|19.61|
	|01/03/1987|11.95|
	|01/04/1987|15.30|
	|01/05/1987|24.63|
	|01/06/1987|18.02|
	|01/11/1987|12.92|
	|01/12/1987|14.38|
	|01/01/1988|16.78|
	|01/02/1988|18.35|
	+----------+-----+
	only showing top 10 rows



# 7. Salvar o DataFrame jurosDF10  como tabela Hive "<nome>.tab_juros_selic"

scala> jurosDF10.write.saveAsTable("marcia.tab_juros_selic")

// Será salvo em /user/hive/warehouse/marcia.db/tab_juros_selic
// Se já tivesse um diretório com esse nome, daria erro


# 8. Criar o DataFrame jurosHiveDF para ler a tabela "<nome>.tab_juros_selic"

# Spark tem acesso ao metastore, não precisa sair dele (não precisa entrar no Hive para fazer isso)

scala> val jurosHiveDF =  spark.read.table("marcia.tab_juros_selic")

	jurosHiveDF: org.apache.spark.sql.DataFrame = [data: string, valor: string]



# 9. Visualizar o Schema do jurosHiveDF

scala> jurosHiveDF.printSchema

	root
	 |-- data: string (nullable = true)
	 |-- valor: string (nullable = true)



# 10. Mostrar os 5 primeiros registros do jurosHiveDF

scala> jurosHiveDF.show(5)

	+----------+-----+
	|      data|valor|
	+----------+-----+
	|01/01/1987|11.00|
	|01/02/1987|19.61|
	|01/03/1987|11.95|
	|01/04/1987|15.30|
	|01/05/1987|24.63|
	+----------+-----+
	only showing top 5 rows



# 11. Salvar o DataFrame jurosHiveDF no HDFS no diretório “/user/aluno/nome/data/save_juros” no formato parquet

// jurosHiveDF.write.save OU jurosHiveDF.write.parquet
scala> jurosHiveDF.write.save("/user/aluno/marcia-castagna/data/save_juros")



# 12. Visualizar o save_juros no HDFS


# Sair do shell
control + D


# Sair do Spark
control + D

# Acessar namenode (hdfs)
docker exec -it namenode bash


hdfs dfs -ls /user/aluno/marcia-castagna/data/save_juros

	// Todo arquivo no Spark é comprimido, por isso está como extensão '.snappy.parquet' (otimiza)
	Found 2 items
	-rw-r--r--   2 root supergroup          0 2021-05-09 20:13 /user/aluno/marcia-castagna/data/save_juros/_SUCCESS
	-rw-r--r--   2 root supergroup       1419 2021-05-09 20:13 /user/aluno/marcia-castagna/data/save_juros/part-00000-3f587a92-dcb5-44c9-b83c-69eba79f8a2f-c000.snappy.parquet


# Visualizando a tabela Hive criada no exercício 7
hdfs dfs -ls /user/hive/warehouse/marcia.db/tab_juros_selic
 
 	// Também salva como '.snappy.parquet'
	Found 2 items
	-rw-r--r--   2 root supergroup          0 2021-05-09 20:04 /user/hive/warehouse/marcia.db/tab_juros_selic/_SUCCESS
	-rw-r--r--   2 root supergroup       1419 2021-05-09 20:04 /user/hive/warehouse/marcia.db/tab_juros_selic/part-00000-035c75f9-b2f2-425d-9c88-4ce10e6a876b-c000.snappy.parquet 

// Pode entrar no Hive e fazer um select em tab_juros_selic, é uma tabela interna, com compressão snappy e formato parquet
// Spark salva otimizado para o Hive e para o HDFS, facilitando o trabalho



# 13. Criar o DataFrame jurosHDFS para ler o diretório do "save_juros" da questão 8

control + d

docker exec -it spark bash

spark-shell


// spark.read.load OU spark.read.parquet
scala> val jurosHDFS = spark.read.load("/user/aluno/marcia-castagna/data/save_juros")

	jurosHDFS: org.apache.spark.sql.DataFrame = [data: string, valor: string]


// Também pode estar assim (local padrão que o hdfs está tendo acesso)
scala> val jurosHDFS = spark.read.load("hdfs://namenode:8020/user/aluno/marcia-castagna/data/save_juros")

	jurosHDFS: org.apache.spark.sql.DataFrame = [data: string, valor: string]


scala> val jurosHDFS = spark.read.parquet("/user/aluno/marcia-castagna/data/save_juros") 

	jurosHDFS: org.apache.spark.sql.DataFrame = [data: string, valor: string]



# 14. Visualizar o Schema do jurosHDFS
  
scala> jurosHDFS.printSchema

	root
	 |-- data: string (nullable = true)
	 |-- valor: string (nullable = true)  
  


# 15. Mostrar os 5 primeiros registros do jurosHDFS

scala> jurosHDFS.show(5)

	+----------+-----+
	|      data|valor|
	+----------+-----+
	|01/01/1987|11.00|
	|01/02/1987|19.61|
	|01/03/1987|11.95|
	|01/04/1987|15.30|
	|01/05/1987|24.63|
	+----------+-----+
	only showing top 5 rows



control + d

control + d

docker-compose stop


```