# Sqoop - Importação BD Sakila – Carga Incremental


```

docker-compose up -d

docker exec -it database bash

mysql -psecret

show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| employees          |
| hue                |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
+--------------------+
7 rows in set (0.00 sec)


use sakila;

show tables;

+----------------------------+
| Tables_in_sakila           |
+----------------------------+
| actor                      |
| actor_info                 |
| address                    |
| category                   |
| city                       |
| country                    |
| customer                   |
| customer_list              |
| film                       |
| film_actor                 |
| film_category              |
| film_list                  |
| film_text                  |
| inventory                  |
| language                   |
| nicer_but_slower_film_list |
| payment                    |
| rental                     |
| sales_by_film_category     |
| sales_by_store             |
| staff                      |
| staff_list                 |
| store                      |
+----------------------------+
23 rows in set (0.00 sec)



select * from rental limit 5;

+-----------+---------------------+--------------+-------------+---------------------+----------+---------------------+
| rental_id | rental_date         | inventory_id | customer_id | return_date         | staff_id | last_update         |
+-----------+---------------------+--------------+-------------+---------------------+----------+---------------------+
|         1 | 2005-05-24 22:53:30 |          367 |         130 | 2005-05-26 22:04:30 |        1 | 2006-02-15 21:30:53 |
|         2 | 2005-05-24 22:54:33 |         1525 |         459 | 2005-05-28 19:40:33 |        1 | 2006-02-15 21:30:53 |
|         3 | 2005-05-24 23:03:39 |         1711 |         408 | 2005-06-01 22:12:39 |        1 | 2006-02-15 21:30:53 |
|         4 | 2005-05-24 23:04:41 |         2452 |         333 | 2005-06-03 01:43:41 |        2 | 2006-02-15 21:30:53 |
|         5 | 2005-05-24 23:05:21 |         2079 |         222 | 2005-06-02 04:33:21 |        1 | 2006-02-15 21:30:53 |
+-----------+---------------------+--------------+-------------+---------------------+----------+---------------------+
5 rows in set (0.00 sec)



=========================================================================================================

Realizar com uso do MySQL


# 1. Criar a tabela cp_rental_append, contendo a cópia da tabela rental com os campos rental_id e rental_date

create table cp_rental_append select rental_id, rental_date from rental;

select * from cp_rental_append limit 5;

+-----------+---------------------+
| rental_id | rental_date         |
+-----------+---------------------+
|         1 | 2005-05-24 22:53:30 |
|         2 | 2005-05-24 22:54:33 |
|         3 | 2005-05-24 23:03:39 |
|         4 | 2005-05-24 23:04:41 |
|         5 | 2005-05-24 23:05:21 |
+-----------+---------------------+



# 2 .Criar a tabela cp_rental_id e cp_rental_date, contendo a cópia da tabela cp_rental_append


create table cp_rental_id select * from cp_rental_append;

create table cp_rental_date select * from cp_rental_append;


select * from cp_rental_id limit 5;
+-----------+---------------------+
| rental_id | rental_date         |
+-----------+---------------------+
|         1 | 2005-05-24 22:53:30 |
|         2 | 2005-05-24 22:54:33 |
|         3 | 2005-05-24 23:03:39 |
|         4 | 2005-05-24 23:04:41 |
|         5 | 2005-05-24 23:05:21 |
+-----------+---------------------+


select * from cp_rental_date limit 5;
+-----------+---------------------+
| rental_id | rental_date         |
+-----------+---------------------+
|         1 | 2005-05-24 22:53:30 |
|         2 | 2005-05-24 22:54:33 |
|         3 | 2005-05-24 23:03:39 |
|         4 | 2005-05-24 23:04:41 |
|         5 | 2005-05-24 23:05:21 |
+-----------+---------------------+



=========================================================================================================

Realizar com uso do Sqoop - Importações no warehouse /user/hive/warehouse/db_test3 e visualizar no HDFS

# 3. Importar as tabelas cp_rental_append, cp_rental_id e cp_rental_date com 1 mapeador

# Sair do mysql
control + D

# Sair do database
control + D

# hdfs
docker exec -it namenode bash


# Banco de dados sakila, tabela cp_rental_append
sqoop import --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1  --table cp_rental_append

21/05/07 03:13:27 INFO mapreduce.ImportJobBase: Transferred 427.8623 KB in 5.6602 seconds (75.5914 KB/sec)
21/05/07 03:13:27 INFO mapreduce.ImportJobBase: Retrieved 16044 records.


# Banco de dados sakila, tabela cp_rental_id
sqoop import --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1  --table cp_rental_id


# Banco de dados sakila, tabela cp_rental_date
sqoop import --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1  --table cp_rental_date
 
 
# Validando no hdfs

	hdfs dfs -ls -R /user/hive/warehouse/db_test3
	
	drwxr-xr-x   - root supergroup          0 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append/_SUCCESS
	-rw-r--r--   3 root supergroup     438131 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append/part-m-00000
	
	drwxr-xr-x   - root supergroup          0 2021-05-07 03:16 /user/hive/warehouse/db_test3/cp_rental_date
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:16 /user/hive/warehouse/db_test3/cp_rental_date/_SUCCESS
	-rw-r--r--   3 root supergroup     438131 2021-05-07 03:16 /user/hive/warehouse/db_test3/cp_rental_date/part-m-00000
	
	drwxr-xr-x   - root supergroup          0 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id/_SUCCESS
	-rw-r--r--   3 root supergroup     438131 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id/part-m-00000
	
	
	hdfs dfs -ls -h -R /user/hive/warehouse/db_test3
	
	drwxr-xr-x   - root supergroup          0 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append/_SUCCESS
	-rw-r--r--   3 root supergroup    427.9 K 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append/part-m-00000
	
	drwxr-xr-x   - root supergroup          0 2021-05-07 03:16 /user/hive/warehouse/db_test3/cp_rental_date
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:16 /user/hive/warehouse/db_test3/cp_rental_date/_SUCCESS
	-rw-r--r--   3 root supergroup    427.9 K 2021-05-07 03:16 /user/hive/warehouse/db_test3/cp_rental_date/part-m-00000
	
	drwxr-xr-x   - root supergroup          0 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id/_SUCCESS
	-rw-r--r--   3 root supergroup    427.9 K 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id/part-m-00000	
	
	# cp_rental_append
	hdfs dfs -tail /user/hive/warehouse/db_test3/cp_rental_append/part-m-00000
	
	2-14 15:16:03.0
	13753,2006-02-14 15:16:03.0
	11754,2006-02-14 15:16:03.0
	// ...
	11676,2006-02-14 15:16:03.0
	14616,2006-02-14 15:16:03.0
	11739,2006-02-14 15:16:03.0
	
	# cp_rental_date
	hdfs dfs -tail /user/hive/warehouse/db_test3/cp_rental_date/part-m-00000
	
	# cp_rental_id
	hdfs dfs -tail /user/hive/warehouse/db_test3/cp_rental_id/part-m-00000
 
 
=========================================================================================================

Realizar com uso do MySQL

# 4. Executar o sql /db-sql/sakila/insert_rental.sql no container do database

	# Sair do namenode: control + D

	$ docker exec -it database bash

	$ cd /db-sql/sakila
	
	// Conteúdo do arquivo insert_rental.sql *
	cat insert_rental.sql

	$ mysql -psecret < insert_rental.sql

	ERROR 1054 (42S22) at line 25: Unknown column 'cp_rental_date' in 'order clause'
	
	# Atualizar os pacotes
	apt-get update
	
	apt-get install apt-file
	apt-file update
	
	apt-get install nano
	apt-get install vim
	
	nano insert_rental.sql
	
	mysql -psecret < insert_rental.sql

	
	rental_id	rental_date
	15458	2006-02-14 15:16:03
	13421	2006-02-14 15:16:03
	15542	2006-02-14 15:16:03
	15294	2006-02-14 15:16:03
	13428	2006-02-14 15:16:03
	12988	2006-02-14 15:16:03
	12786	2006-02-14 15:16:03
	13952	2006-02-14 15:16:03
	12574	2006-02-14 15:16:03
	14915	2006-02-14 15:16:03
	rental_id	rental_date
	16055	2005-08-23 22:59:20
	16054	2005-08-23 22:57:40
	16053	2005-08-23 22:56:10
	16052	2005-08-23 22:55:20
	16051	2005-08-23 22:54:30
	16050	2005-08-23 22:52:50
	16049	2005-08-23 22:40:00
	16049	2005-08-23 22:50:12
	16048	2005-08-23 22:30:00
	16048	2005-08-23 22:43:07
	
 

=============================================================================================================

// * Conteúdo do arquivo insert_rental.sql

use sakila;
DROP TABLE IF EXISTS cp_rental_append, cp_rental_date, cp_rental_id;
create table cp_rental_append select rental_id, rental_date from rental;
create table cp_rental_date select * from cp_rental_append;
create table cp_rental_id select * from cp_rental_date;

insert into cp_rental_date values(16048,'2005-08-23 22:30:00');
insert into cp_rental_date values(16049,'2005-08-23 22:40:00');
insert into cp_rental_date values(16050,'2005-08-23 22:52:50');
insert into cp_rental_date values(16051,'2005-08-23 22:54:30');
insert into cp_rental_date values(16052,'2005-08-23 22:55:20');
insert into cp_rental_date values(16054,'2005-08-23 22:57:40');
insert into cp_rental_date values(16053,'2005-08-23 22:56:10');
insert into cp_rental_date values(16055,'2005-08-23 22:59:20');

insert into cp_rental_id values(16048,'2005-08-23 22:30:00');
insert into cp_rental_id values(16049,'2005-08-23 22:40:00');
insert into cp_rental_id values(16050,'2005-08-23 22:52:50');
insert into cp_rental_id values(16051,'2005-08-23 22:54:30');
insert into cp_rental_id values(16052,'2005-08-23 22:55:20');
insert into cp_rental_id values(16054,'2005-08-23 22:57:40');
insert into cp_rental_id values(16053,'2005-08-23 22:56:10');
insert into cp_rental_id values(16055,'2005-08-23 22:59:20');

select * from cp_rental_date ORDER By cp_rental_date DESC limit 10;

select * from cp_rental_id ORDER By cp_rental_date DESC limit 10;


=========================================================================================================

Realizar com uso do Sqoop - Importações no warehouse /user/hive/warehouse/db_test3 e visualizar no HDFS

# 5. Atualizar a tabela cp_rental_append no HDFS anexando os novos arquivos

	# sair do mysql
	# sair do database

	docker exec -it namenode bash
	
	// Não vai executar pois o diretório já existe
	sqoop import --table cp_rental_append --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1 
	// ERROR tool.ImportTool: Import failed: org.apache.hadoop.mapred.FileAlreadyExistsException: 
	// Output directory hdfs://namenode:8020/user/hive/warehouse/db_test3/cp_rental_append already exists
	

	# sqoop import --table cp_rental_append --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1 --incremental append
	# Não precisa do --incremental append, só anexar com --append
	sqoop import --table cp_rental_append --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1 --append

	

# 6. Atualizar a tabela cp_rental_id no HDFS de acordo com o último registro de rental_id, adicionando apenas os novos dados.

	sqoop eval --connect jdbc:mysql://database/sakila --username root --password secret --query "select * from cp_rental_append order by rental_id desc limit 5"
	
			-------------------------------------
		| rental_id   | rental_date         | 
		-------------------------------------
		| 16049       | 2005-08-23 22:50:12.0 | 
		| 16048       | 2005-08-23 22:43:07.0 | 
		| 16047       | 2005-08-23 22:42:48.0 | 
		| 16046       | 2005-08-23 22:26:47.0 | 
		| 16045       | 2005-08-23 22:25:26.0 | 
		-------------------------------------
		
		# Último registro 16049

	
	sqoop import --table cp_rental_id --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1 --incremental append --check-column rental_id --last-value 16049
	
	

# 7. Atualizar a tabela cp_rental_date no HDFS de acordo com o último registro de rental_date, atualizando os registros a partir desta data.

	# Campo para mesclar: usar --merge-key
	sqoop import --table cp_rental_date --connect jdbc:mysql://database/sakila --username root --password secret --warehouse-dir /user/hive/warehouse/db_test3 -m 1 --incremental lastmodified --merge-key rental_id --check-column rental_date --last-value '2005-08-23 22:50:12.0'
	
	# SQL no console:
	Executing query: SELECT `rental_id`, `rental_date` FROM `cp_rental_date` AS `cp_rental_date` 
	WHERE ( `rental_date` >= '2005-08-23 22:50:12.0' AND `rental_date` < '2021-05-09 00:12:44.0' ) AND ( 1=1 ) AND ( 1=1 )


	hdfs dfs -ls -h -R /user/hive/warehouse/db_test3
	
	# append
	# Duplicou: append acumula
	# Dois registros de 427.9
	drwxr-xr-x   - root supergroup          0 2021-05-08 23:49 /user/hive/warehouse/db_test3/cp_rental_append
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append/_SUCCESS
	-rw-r--r--   3 root supergroup    427.9 K 2021-05-07 03:13 /user/hive/warehouse/db_test3/cp_rental_append/part-m-00000
	-rw-r--r--   3 root supergroup    427.9 K 2021-05-08 23:49 /user/hive/warehouse/db_test3/cp_rental_append/part-m-00001
	
	# Atualizar em relação a uma data
	# Em relação ao campo id que era o referencial, atualizou alguns registros de acordo com a data
	# Tamanho ficou maior: 428.0 K
	drwxr-xr-x   - root supergroup          0 2021-05-09 00:12 /user/hive/warehouse/db_test3/cp_rental_date
	-rw-r--r--   3 root supergroup          0 2021-05-09 00:12 /user/hive/warehouse/db_test3/cp_rental_date/_SUCCESS
	-rw-r--r--   3 root supergroup    428.0 K 2021-05-09 00:12 /user/hive/warehouse/db_test3/cp_rental_date/part-r-00000
	
	# Inserir somente os últimos dados em relação a um  id (na verdade, algum valor sequencial)
	# incremental append: insere os novos dados
	# O arquivo part-m-00001 só tem os últimos registros 
	drwxr-xr-x   - root supergroup          0 2021-05-09 00:01 /user/hive/warehouse/db_test3/cp_rental_id
	-rw-r--r--   3 root supergroup          0 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id/_SUCCESS
	-rw-r--r--   3 root supergroup    427.9 K 2021-05-07 03:15 /user/hive/warehouse/db_test3/cp_rental_id/part-m-00000
	-rw-r--r--   3 root supergroup        168 2021-05-09 00:01 /user/hive/warehouse/db_test3/cp_rental_id/part-m-00001	
	
	
	# Validando no hdfs
	hdfs dfs -cat /user/hive/warehouse/db_test3/cp_rental_id/part-m-00001	
	
	16050,2005-08-23 22:52:50.0
	16051,2005-08-23 22:54:30.0
	16052,2005-08-23 22:55:20.0
	16054,2005-08-23 22:57:40.0
	16053,2005-08-23 22:56:10.0
	16055,2005-08-23 22:59:20.0
	

	control + D
	
	docker-compose stop


```