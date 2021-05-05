# Hive - Criação de Tabelas Otimizadas

```

docker-compose up -d

docker exec -it hive-server bash
beeline -u jdbc:hive2://localhost:10000

show databases;


# 1. Usar o banco de dados <nome>

use marcia;


# 2. Selecionar os 10 primeiros registros da tabela pop

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


# 3. Criar a tabela pop_parquet no formato parquet para ler os dados da tabela pop

create table pop_parquet (
	zip_code int,
	total_population int,
	median_age float,
	total_males int,
	total_females int,
	total_households int,
	average_household_size float   
)
stored as parquet;

# usar 'stored as parquet' OU ao invés de só 'parquet', todo nome do apache.hadoop...parquet
# isso vai depender da config do cluster


# 4. Inserir os dados da tabela pop na pop_parquet

insert into pop_parquet select * from pop;


# 5. Contar os registros da tabela pop_parquet

select count(*) from pop;
+------+
| _c0  |
+------+
| 319  |
+------+


select count(*) from pop_parquet;
+------+
| _c0  |
+------+
| 319  |
+------+


# 6. Selecionar os 10 primeiros registros da tabela pop_parquet

select * from pop_parquet limit 10;

+-----------------------+-------------------------------+-------------------------+--------------------------+----------------------------+-------------------------------+-------------------------------------+
| pop_parquet.zip_code  | pop_parquet.total_population  | pop_parquet.median_age  | pop_parquet.total_males  | pop_parquet.total_females  | pop_parquet.total_households  | pop_parquet.average_household_size  |
+-----------------------+-------------------------------+-------------------------+--------------------------+----------------------------+-------------------------------+-------------------------------------+
| 91371                 | 1                             | 73.5                    | 0                        | 1                          | 1                             | 1.0                                 |
| 90001                 | 57110                         | 26.6                    | 28468                    | 28642                      | 12971                         | 4.4                                 |
| 90002                 | 51223                         | 25.5                    | 24876                    | 26347                      | 11731                         | 4.36                                |
| 90003                 | 66266                         | 26.3                    | 32631                    | 33635                      | 15642                         | 4.22                                |
| 90004                 | 62180                         | 34.8                    | 31302                    | 30878                      | 22547                         | 2.73                                |
| 90005                 | 37681                         | 33.9                    | 19299                    | 18382                      | 15044                         | 2.5                                 |
| 90006                 | 59185                         | 32.4                    | 30254                    | 28931                      | 18617                         | 3.13                                |
| 90007                 | 40920                         | 24.0                    | 20915                    | 20005                      | 11944                         | 3.0                                 |
| 90008                 | 32327                         | 39.7                    | 14477                    | 17850                      | 13841                         | 2.33                                |
| 90010                 | 3800                          | 37.8                    | 1874                     | 1926                       | 2014                          | 1.87                                |
+-----------------------+-------------------------------+-------------------------+--------------------------+----------------------------+-------------------------------+-------------------------------------+
10 rows selected (0.197 seconds)



# 7. Criar a tabela pop_parquet_snappy no formato parquet com compressão Snappy para ler os dados da tabela pop

create table pop_parquet_snappy (
	zip_code int,
	total_population int,
	median_age float,
	total_males int,
	total_females int,
	total_households int,
	average_household_size float   
)
stored as parquet
tblproperties('parquet.compress'='SNAPPY');

# ao invés de SNAPPY, pode ser necessário por org.apache.hadoop... todo caminho
# antigamente era compressed (depende da versão do Hive



desc formatted pop_parquet_snappy;
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
| CreateTime:                   | Wed May 05 03:36:22 UTC 2021                       | NULL                        |
| LastAccessTime:               | UNKNOWN                                            | NULL                        |
| Retention:                    | 0                                                  | NULL                        |
| Location:                     | hdfs://namenode:8020/user/hive/warehouse/marcia.db/pop_parquet_snappy | NULL                        |
| Table Type:                   | MANAGED_TABLE                                      | NULL                        |
| Table Parameters:             | NULL                                               | NULL                        |
|                               | COLUMN_STATS_ACCURATE                              | {\"BASIC_STATS\":\"true\"}  |
|                               | numFiles                                           | 0                           |
|                               | numRows                                            | 0                           |
|                               | parquet.compress                                   | SNAPPY                      |
|                               | rawDataSize                                        | 0                           |
|                               | totalSize                                          | 0                           |
|                               | transient_lastDdlTime                              | 1620185782                  |
|                               | NULL                                               | NULL                        |
| # Storage Information         | NULL                                               | NULL                        |
| SerDe Library:                | org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe | NULL                        |
| InputFormat:                  | org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat | NULL                        |
| OutputFormat:                 | org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat | NULL                        |
| Compressed:                   | No                                                 | NULL                        |
| Num Buckets:                  | -1                                                 | NULL                        |
| Bucket Columns:               | []                                                 | NULL                        |
| Sort Columns:                 | []                                                 | NULL                        |
| Storage Desc Params:          | NULL                                               | NULL                        |
|                               | serialization.format                               | 1                           |
+-------------------------------+----------------------------------------------------+-----------------------------+
37 rows selected (0.129 seconds)


Formato de entrada: org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe

Formato de compressão:  parquet.compress                                   | SNAPPY    


# 8. Inserir os dados da tabela pop na pop_parquet_snappy

insert into pop_parquet_snappy select * from pop;


# 9. Contar os registros da tabela pop_parquet_snappy

select count(*) from pop_parquet_snappy;

+------+
| _c0  |
+------+
| 319  |
+------+

# 10. Selecionar os 10 primeiros registros da tabela pop_parquet_snappy

select * from pop_parquet_snappy limit 10;

+------------------------------+--------------------------------------+--------------------------------+---------------------------------+-----------------------------------+--------------------------------------+--------------------------------------------+
| pop_parquet_snappy.zip_code  | pop_parquet_snappy.total_population  | pop_parquet_snappy.median_age  | pop_parquet_snappy.total_males  | pop_parquet_snappy.total_females  | pop_parquet_snappy.total_households  | pop_parquet_snappy.average_household_size  |
+------------------------------+--------------------------------------+--------------------------------+---------------------------------+-----------------------------------+--------------------------------------+--------------------------------------------+
| 91371                        | 1                                    | 73.5                           | 0                               | 1                                 | 1                                    | 1.0                                        |
| 90001                        | 57110                                | 26.6                           | 28468                           | 28642                             | 12971                                | 4.4                                        |
| 90002                        | 51223                                | 25.5                           | 24876                           | 26347                             | 11731                                | 4.36                                       |
| 90003                        | 66266                                | 26.3                           | 32631                           | 33635                             | 15642                                | 4.22                                       |
| 90004                        | 62180                                | 34.8                           | 31302                           | 30878                             | 22547                                | 2.73                                       |
| 90005                        | 37681                                | 33.9                           | 19299                           | 18382                             | 15044                                | 2.5                                        |
| 90006                        | 59185                                | 32.4                           | 30254                           | 28931                             | 18617                                | 3.13                                       |
| 90007                        | 40920                                | 24.0                           | 20915                           | 20005                             | 11944                                | 3.0                                        |
| 90008                        | 32327                                | 39.7                           | 14477                           | 17850                             | 13841                                | 2.33                                       |
| 90010                        | 3800                                 | 37.8                           | 1874                            | 1926                              | 2014                                 | 1.87                                       |
+------------------------------+--------------------------------------+--------------------------------+---------------------------------+-----------------------------------+--------------------------------------+--------------------------------------------+
10 rows selected (0.139 seconds)


# 11. Comparar as tabelas pop, pop_parquet e pop_parquet_snappy no HDFS.

Avaliar o armazenamento

docker exec -it namenode bash

# Tabela interna está no caminho default do Hive ( /user/hive/warehouse/ )
hdfs dfs -ls /user/hive/warehouse/marcia.db

Found 3 items
drwxrwxr-x   - root supergroup          0 2021-05-03 01:11 /user/hive/warehouse/marcia.db/pop
drwxrwxr-x   - root supergroup          0 2021-05-05 03:28 /user/hive/warehouse/marcia.db/pop_parquet
drwxrwxr-x   - root supergroup          0 2021-05-05 03:39 /user/hive/warehouse/marcia.db/pop_parquet_snappy



hdfs dfs -ls -R /user/hive/warehouse/marcia.db

drwxrwxr-x   - root supergroup          0 2021-05-03 01:11 /user/hive/warehouse/marcia.db/pop
-rwxrwxr-x   3 root supergroup      12183 2021-05-03 01:11 /user/hive/warehouse/marcia.db/pop/populacaoLA.csv
drwxrwxr-x   - root supergroup          0 2021-05-05 03:28 /user/hive/warehouse/marcia.db/pop_parquet
-rwxrwxr-x   3 root supergroup       9477 2021-05-05 03:28 /user/hive/warehouse/marcia.db/pop_parquet/000000_0
drwxrwxr-x   - root supergroup          0 2021-05-05 03:39 /user/hive/warehouse/marcia.db/pop_parquet_snappy
-rwxrwxr-x   3 root supergroup       9477 2021-05-05 03:39 /user/hive/warehouse/marcia.db/pop_parquet_snappy/000000_0



hdfs dfs -ls -R -h /user/hive/warehouse/marcia.db

drwxrwxr-x   - root supergroup          0 2021-05-03 01:11 /user/hive/warehouse/marcia.db/pop
-rwxrwxr-x   3 root supergroup     11.9 K 2021-05-03 01:11 /user/hive/warehouse/marcia.db/pop/populacaoLA.csv
drwxrwxr-x   - root supergroup          0 2021-05-05 03:28 /user/hive/warehouse/marcia.db/pop_parquet
-rwxrwxr-x   3 root supergroup      9.3 K 2021-05-05 03:28 /user/hive/warehouse/marcia.db/pop_parquet/000000_0
drwxrwxr-x   - root supergroup          0 2021-05-05 03:39 /user/hive/warehouse/marcia.db/pop_parquet_snappy
-rwxrwxr-x   3 root supergroup      9.3 K 2021-05-05 03:39 /user/hive/warehouse/marcia.db/pop_parquet_snappy/000000_0



hdfs dfs -du -h /user/hive/warehouse/marcia.db

11.9 K  /user/hive/warehouse/marcia.db/pop
9.3 K   /user/hive/warehouse/marcia.db/pop_parquet
9.3 K   /user/hive/warehouse/marcia.db/pop_parquet_snappy

```