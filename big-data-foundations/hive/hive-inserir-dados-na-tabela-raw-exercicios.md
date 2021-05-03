# Hive - Inserir Dados na Tabela Raw

```
# Subir cluster big data
docker-compose up -d

# Acessar o hive server
docker exec -it hive-server bash

# Conectar o beeline
beeline -u jdbc:hive2://localhost:10000

# Mostrar database 
show databases;

# Selecionando o database
use marcia;

# Visualizando todas as tabelas
show tables;

+-----------+
| tab_name  |
+-----------+
| pop       |
+-----------+
1 row selected (0.084 seconds)


# 1.Visualizar a descrição da tabela pop do banco de dados <nome>

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
7 rows selected (0.066 seconds)


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
39 rows selected (0.093 seconds)


# 2.Selecionar os 10 primeiros registros da tabela pop

select * from pop limit 10;

+---------------+-----------------------+-----------------+------------------+--------------------+-----------------------+-----------------------------+
| pop.zip_code  | pop.total_population  | pop.median_age  | pop.total_males  | pop.total_females  | pop.total_households  | pop.average_household_size  |
+---------------+-----------------------+-----------------+------------------+--------------------+-----------------------+-----------------------------+
+---------------+-----------------------+-----------------+------------------+--------------------+-----------------------+-----------------------------+
No rows selected (1.206 seconds)


# 3.Carregar o arquivo do HDFS "/user/aluno/<nome>/data/população/populacaoLA.csv" para a tabela Hive pop

# Já foi realizado esse passo (copiar arq no hdfs) em outro exercício

# Não precisa passar o 'local' no comando abaixo, pois já está no hdfs
# Pode passar o caminho completo do arquivo OU somente o diretório (tudo que estiver lá dentro ele vai ler)
# 'overwrite' subscreve se já tiver alguma informação

load data inpath '/user/aluno/marcia-castagna/data/populacao' overwrite into table pop;


# 4.Selecionar os 10 primeiros registros da tabela pop

select * from pop limit 10;

+---------------+-----------------------+-----------------+------------------+--------------------+-----------------------+-----------------------------+
| pop.zip_code  | pop.total_population  | pop.median_age  | pop.total_males  | pop.total_females  | pop.total_households  | pop.average_household_size  |
+---------------+-----------------------+-----------------+------------------+--------------------+-----------------------+-----------------------------+
| 91371         | 1                     | 73.5            | 0                | 1                  | 1                     | 1.0                         |
| 90001         | 57110                 | 26.6            | 28468            | 28642              | 12971                 | 4.4                         |
| 90002         | 51223                 | 25.5            | 24876            | 26347              | 11731                 | 4.36                        |
| 90003         | 66266                 | 26.3            | 32631            | 33635              | 15642                 | 4.22                        |
| 90004         | 62180                 | 34.8            | 31302            | 30878              | 22547                 | 2.73                        |
| 90005         | 37681                 | 33.9            | 19299            | 18382              | 15044                 | 2.5                         |
| 90006         | 59185                 | 32.4            | 30254            | 28931              | 18617                 | 3.13                        |
| 90007         | 40920                 | 24.0            | 20915            | 20005              | 11944                 | 3.0                         |
| 90008         | 32327                 | 39.7            | 14477            | 17850              | 13841                 | 2.33                        |
| 90010         | 3800                  | 37.8            | 1874             | 1926               | 2014                  | 1.87                        |
+---------------+-----------------------+-----------------+------------------+--------------------+-----------------------+-----------------------------+
10 rows selected (0.193 seconds)


#  5.Contar a quantidade de registros da tabela pop


select count(*) from pop;

WARNING: Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.

+------+
| _c0  |
+------+
| 319  |
+------+
1 row selected (1.806 seconds)

# Sair do beeline

# Sair do Hive server

docker-compose stop

```