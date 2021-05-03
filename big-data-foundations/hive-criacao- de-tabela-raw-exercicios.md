# Criação de Tabela Raw

tabela raw ou tabela bruta = mapeando o arquivo original


```
# 1. Enviar o arquivo local "/input/exercises-data/populacaoLA/populacaoLA.csv" para o diretório no HDFS "/user/aluno/<nome>/data/populacao"

docker-compose up -d 

docker exec -it namenode bash

ls /input/exercises-data/populacaoLA/
README.md  populacaoLA.csv  populacaoLA.json

# Criar diretório
hdfs dfs -mkdir /user/aluno/marcia-castagna/data/populacao

# Copiando o arquivo "/input/exercises-data/populacaoLA/populacaoLA.csv" para o diretório no HDFS "/user/aluno/<nome>/data/populacao"
hdfs dfs -put /input/exercises-data/populacaoLA/populacaoLA.csv  /user/aluno/marcia-castagna/data/populacao

# Confirmando se copiou
hdfs dfs -ls /user/aluno/marcia-castagna/data/populacao
Found 1 items
-rw-r--r--   3 root supergroup      12183 2021-05-03 01:11 /user/aluno/marcia-castagna/data/populacao/populacaoLA.csv

# Visualizando o conteúdo do arquivo
hdfs dfs -cat /user/aluno/marcia-castagna/data/populacao/populacaoLA.csv

# Visualizando as 3 primeiras linhas do arquivo
hdfs dfs -cat /user/aluno/marcia-castagna/data/populacao/populacaoLA.csv | head -n 3

Zip Code,Total Population,Median Age,Total Males,Total Females,Total Households,Average Household Size
91371,1,73.5,0,1,1,1
90001,57110,26.6,28468,28642,12971,4.4


# 2. Listar os bancos de dados no Hive

# Sair do namenode
control + D

# Acessar o Hive
docker exec -it hive-server bash

# Acessar o beeline
beeline --help
beeline -u jdbc:hive2://localhost:10000

SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/hive/lib/log4j-slf4j-impl-2.6.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/hadoop-2.7.4/share/hadoop/common/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Connecting to jdbc:hive2://localhost:10000
Connected to: Apache Hive (version 2.3.2)
Driver: Hive JDBC (version 2.3.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
Beeline version 2.3.2 by Apache Hive

# 3. Criar o banco de dados <nome>

show databases;

0: jdbc:hive2://localhost:10000> show databases;
+----------------+
| database_name  |
+----------------+
| default        |
+----------------+
1 row selected (0.965 seconds)


# 4. Criar a Tabela Hive no BD <nome>

# Criando database
create database marcia;

No rows affected (0.765 seconds)

# Exbindo databases
0: jdbc:hive2://localhost:10000> show databases;
+----------------+
| database_name  |
+----------------+
| default        |
| marcia         |
+----------------+
2 rows selected (0.024 seconds)

# Selecionando o database
use marcia;


Tabela interna: pop

Campos:
	zip_code - int
	total_population - int
	median_age - float
	total_males - int
	total_females - int
	total_households - int
	average_household_size - float

Propriedades
	Delimitadores: Campo ',' | Linha '\n'
	Sem Partição
	Tipo do arquivo: Texto
	tblproperties("skip.header.line.count"="1") -> pula a linha do cabeçalho, uma no caso
	
# Criar a tabela	
# Se nao fez 'use marcia;', pode usar 'create table marcia.pop'
# Ir digitando e dando enter, linha a linha (se der erro, só ajustar/refazer a linha com erro):
create table pop (
	zip_code int,
	total_population int,
	median_age float,
	total_males int,
	total_females int,
	total_households int,
	average_household_size float
)	
row format delimited
fields terminated by ','
lines terminated by '\n'
stored as textfile
tblproperties("skip.header.line.count"="1");

No rows affected (0.361 seconds)



# 5. Visualizar a descrição da tabela pop

desc pop;

+-------------------------+------------+----------+
|        col_name         | data_type  | comment  |
+-------------------------+------------+----------+
| zip_code                | int        |          |
| total_population        | int        |          |
| median_age              | float      |          |
| total_males             | int        |          |
| total_females           | int        |          |
| total_households        | int        |          |
| average_household_size  | float      |          |
+-------------------------+------------+----------+
7 rows selected (0.12 seconds)


desc formatted pop;

+-------------------------------+----------------------------------------------------+-----------------------------+
|           col_name            |                     data_type                      |           comment           |
+-------------------------------+----------------------------------------------------+-----------------------------+
| # col_name                    | data_type                                          | comment                     |
|                               | NULL                                               | NULL                        |
| zip_code                      | int                                                |                             |
| total_population              | int                                                |                             |
| median_age                    | float                                              |                             |
| total_males                   | int                                                |                             |
| total_females                 | int                                                |                             |
| total_households              | int                                                |                             |
| average_household_size        | float                                              |                             |
|                               | NULL                                               | NULL                        |
| # Detailed Table Information  | NULL                                               | NULL                        |
| Database:                     | marcia                                             | NULL                        |
| Owner:                        | root                                               | NULL                        |
| CreateTime:                   | Mon May 03 01:34:29 UTC 2021                       | NULL                        |
| LastAccessTime:               | UNKNOWN                                            | NULL                        |
| Retention:                    | 0                                                  | NULL                        |
| Location:                     | hdfs://namenode:8020/user/hive/warehouse/marcia.db/pop | NULL                        |
| Table Type:                   | MANAGED_TABLE                                      | NULL                        |
| Table Parameters:             | NULL                                               | NULL                        |
|                               | COLUMN_STATS_ACCURATE                              | {\"BASIC_STATS\":\"true\"}  |
|                               | numFiles                                           | 0                           |
|                               | numRows                                            | 0                           |
|                               | rawDataSize                                        | 0                           |
|                               | skip.header.line.count                             | 1                           |
|                               | totalSize                                          | 0                           |
|                               | transient_lastDdlTime                              | 1620005669                  |
|                               | NULL                                               | NULL                        |
| # Storage Information         | NULL                                               | NULL                        |
| SerDe Library:                | org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe | NULL                        |
| InputFormat:                  | org.apache.hadoop.mapred.TextInputFormat           | NULL                        |
| OutputFormat:                 | org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat | NULL                        |
| Compressed:                   | No                                                 | NULL                        |
| Num Buckets:                  | -1                                                 | NULL                        |
| Bucket Columns:               | []                                                 | NULL                        |
| Sort Columns:                 | []                                                 | NULL                        |
| Storage Desc Params:          | NULL                                               | NULL                        |
|                               | field.delim                                        | ,                           |
|                               | line.delim                                         | \n                          |
|                               | serialization.format                               | ,                           |
+-------------------------------+----------------------------------------------------+-----------------------------+

hdfs://namenode:8020/user/hive/warehouse/marcia.db/pop

	/user/hive/warehouse/ -> local default
	marcia.db -> usa o nome do database para criar o nome da pasta (incluindo .db) 
	pop -> nome da tabela


MANAGED_TABLE: tabela gerenciada (tabela interna)
EXTERNAL_TABLE: tabela externa


org.apache.hadoop.mapred - map reduce

org.apache.hadoop.mapred.TextInputFormat - entrada de texto

field.delim: delimitador dos campos

line.delim: delimitador de linhas

serialization.format: formato de serialização de dados

# sair do beeline
control + D

# sair do Hive server
control + D

docker-compose stop

```