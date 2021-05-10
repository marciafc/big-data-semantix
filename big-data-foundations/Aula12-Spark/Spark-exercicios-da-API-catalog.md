# Spark - Exercícios da API Catalog


```
docker-compose up -d

docker ps

docker exec -it spark bash

spark-shell



Realizar os exercícios usando a API Catalog.


# 1. Visualizar todos os banco de dados


scala> spark.catalog.listDatabases

	res0: org.apache.spark.sql.Dataset[org.apache.spark.sql.catalog.Database] = [name: string, description: string ... 1 more field]


scala> spark.catalog.listDatabases.show

	+-------+--------------------+--------------------+
	|   name|         description|         locationUri|
	+-------+--------------------+--------------------+
	|default|Default Hive data...|hdfs://namenode:8...|
	| marcia|                    |hdfs://namenode:8...|
	+-------+--------------------+--------------------+


# Para visualizar por completo
scala> spark.catalog.listDatabases.show(false)

	+-------+---------------------+--------------------------------------------------+
	|name   |description          |locationUri                                       |
	+-------+---------------------+--------------------------------------------------+
	|default|Default Hive database|hdfs://namenode:8020/user/hive/warehouse          |
	|marcia |                     |hdfs://namenode:8020/user/hive/warehouse/marcia.db|
	+-------+---------------------+--------------------------------------------------+



# 2. Definir o banco de dados "seu-nome" como principal

scala> spark.catalog.setCurrentDatabase("marcia")



# 3. Visualizar todas as tabelas do banco de dados "seu-nome"

scala> spark.catalog.listTables.show

	# isTemporary se é view

	+------------------+--------+--------------------+---------+-----------+
	|              name|database|         description|tableType|isTemporary|
	+------------------+--------+--------------------+---------+-----------+
	|        nascimento|  marcia|                null| EXTERNAL|      false|
	|               pop|  marcia|                null|  MANAGED|      false|
	|       pop_parquet|  marcia|                null|  MANAGED|      false|
	|pop_parquet_snappy|  marcia|                null|  MANAGED|      false|
	|        tab_alunos|  marcia|                null|  MANAGED|      false|
	|   tab_juros_selic|  marcia|                null|  MANAGED|      false|
	|            titles|  marcia|Imported by sqoop...|  MANAGED|      false|
	+------------------+--------+--------------------+---------+-----------+



# 4. Visualizar as colunas da tabela tab_alunos

scala> spark.catalog.listColumns("tab_alunos")

	res5: org.apache.spark.sql.Dataset[org.apache.spark.sql.catalog.Column] = [name: string, description: string ... 4 more fields]


scala> spark.catalog.listColumns("tab_alunos").show

	+-----------------+-----------+--------+--------+-----------+--------+
	|             name|description|dataType|nullable|isPartition|isBucket|
	+-----------------+-----------+--------+--------+-----------+--------+
	|      id_discente|       null|     int|    true|      false|   false|
	|             nome|       null|  string|    true|      false|   false|
	|     ano_ingresso|       null|     int|    true|      false|   false|
	| periodo_ingresso|       null|     int|    true|      false|   false|
	|            nivel|       null|  string|    true|      false|   false|
	|id_forma_ingresso|       null|     int|    true|      false|   false|
	|         id_curso|       null|     int|    true|      false|   false|
	+-----------------+-----------+--------+--------+-----------+--------+



# 5.  Visualizar os 10 primeiros registos da tabela "tab_alunos" com uso do spark.sql

# com uso do spark.sql

scala> spark.sql("select * from tab_alunos limit 10").show

	+-----------+--------------------+------------+----------------+-----+-----------------+--------+
	|id_discente|                nome|ano_ingresso|periodo_ingresso|nivel|id_forma_ingresso|id_curso|
	+-----------+--------------------+------------+----------------+-----+-----------------+--------+
	|      18957|ABELARDO DA SILVA...|        2017|               1|    G|            62151|   76995|
	|        553| ABIEL GODOY MARIANO|        2015|            null|    M|          2081113|    3402|
	|      17977|ABIGAIL ANTUNES S...|        2017|               1|    T|          2081111|  759711|
	|      16613|ABIGAIL FERNANDA ...|        2017|            null|    M|            37350|    1222|
	|      17398|ABIGAIL JOSIANE R...|        2017|            null|    M|            37350|    5041|
	|      26880|ABIMAEL CHRISTOPF...|        2019|               1|    T|          2081115| 1913420|
	|      28508|   ABNER NUNES PERES|        2019|               1|    G|            37350|  181097|
	|      18693|ACSA PEREIRA RODR...|        2017|               1|    G|            62151|   77498|
	|      28071|ACSA ROBALO DOS S...|        2019|               1|    T|          2081115| 3996007|
	|      21968|AÇUCENA CARVALHO ...|        2018|               0|    N|          2081113| 2399357|
	+-----------+--------------------+------------+----------------+-----+-----------------+--------+


scala> spark.read.table("tab_alunos").show(10)

	+-----------+--------------------+------------+----------------+-----+-----------------+--------+
	|id_discente|                nome|ano_ingresso|periodo_ingresso|nivel|id_forma_ingresso|id_curso|
	+-----------+--------------------+------------+----------------+-----+-----------------+--------+
	|      18957|ABELARDO DA SILVA...|        2017|               1|    G|            62151|   76995|
	|        553| ABIEL GODOY MARIANO|        2015|            null|    M|          2081113|    3402|
	|      17977|ABIGAIL ANTUNES S...|        2017|               1|    T|          2081111|  759711|
	|      16613|ABIGAIL FERNANDA ...|        2017|            null|    M|            37350|    1222|
	|      17398|ABIGAIL JOSIANE R...|        2017|            null|    M|            37350|    5041|
	|      26880|ABIMAEL CHRISTOPF...|        2019|               1|    T|          2081115| 1913420|
	|      28508|   ABNER NUNES PERES|        2019|               1|    G|            37350|  181097|
	|      18693|ACSA PEREIRA RODR...|        2017|               1|    G|            62151|   77498|
	|      28071|ACSA ROBALO DOS S...|        2019|               1|    T|          2081115| 3996007|
	|      21968|AÇUCENA CARVALHO ...|        2018|               0|    N|          2081113| 2399357|
	+-----------+--------------------+------------+----------------+-----+-----------------+--------+
	only showing top 10 rows



control + d

control + d

docker-compose stop

```