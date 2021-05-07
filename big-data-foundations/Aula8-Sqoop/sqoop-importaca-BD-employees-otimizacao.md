# Sqoop - Importação BD Employees- Otimização

Realizar com uso do MySQL

```

docker-compose up -d

docker ps

docker exec -it database bash


# 1. Criar a tabela cp_titles_date, contendo a cópia da tabela titles com os campos title e to_date

mysql -psecret

use employees;

show tables;

+----------------------+
| Tables_in_employees  |
+----------------------+
| benefits             |
| current_dept_emp     |
| departments          |
| dept_emp             |
| dept_emp_latest_date |
| dept_manager         |
| employees            |
| employees_2          |
| titles               |
+----------------------+
9 rows in set (0.00 sec)


select * from titles limit 10;

+--------+-----------------+------------+------------+
| emp_no | title           | from_date  | to_date    |
+--------+-----------------+------------+------------+
|  10001 | Senior Engineer | 1986-06-26 | 9999-01-01 |
|  10002 | Staff           | 1996-08-03 | 9999-01-01 |
|  10003 | Senior Engineer | 1995-12-03 | 9999-01-01 |
|  10004 | Engineer        | 1986-12-01 | 1995-12-01 |
|  10004 | Senior Engineer | 1995-12-01 | 9999-01-01 |
|  10005 | Senior Staff    | 1996-09-12 | 9999-01-01 |
|  10005 | Staff           | 1989-09-12 | 1996-09-12 |
|  10006 | Senior Engineer | 1990-08-05 | 9999-01-01 |
|  10007 | Senior Staff    | 1996-02-11 | 9999-01-01 |
|  10007 | Staff           | 1989-02-10 | 1996-02-11 |
+--------+-----------------+------------+------------+
10 rows in set (0.01 sec)


create table cp_titles_date select title, to_date from titles;

Query OK, 443308 rows affected (1.30 sec)
Records: 443308  Duplicates: 0  Warnings: 0


select * from cp_titles_date limit 10;

+-----------------+------------+
| title           | to_date    |
+-----------------+------------+
| Senior Engineer | 9999-01-01 |
| Staff           | 9999-01-01 |
| Senior Engineer | 9999-01-01 |
| Engineer        | 1995-12-01 |
| Senior Engineer | 9999-01-01 |
| Senior Staff    | 9999-01-01 |
| Staff           | 1996-09-12 |
| Senior Engineer | 9999-01-01 |
| Senior Staff    | 9999-01-01 |
| Staff           | 1996-02-11 |
+-----------------+------------+
10 rows in set (0.00 sec)



# 2. Pesquisar os 15 primeiros registros da tabela cp_titles_date


select * from cp_titles_date limit 15;

+--------------------+------------+
| title              | to_date    |
+--------------------+------------+
| Senior Engineer    | 9999-01-01 |
| Staff              | 9999-01-01 |
| Senior Engineer    | 9999-01-01 |
| Engineer           | 1995-12-01 |
| Senior Engineer    | 9999-01-01 |
| Senior Staff       | 9999-01-01 |
| Staff              | 1996-09-12 |
| Senior Engineer    | 9999-01-01 |
| Senior Staff       | 9999-01-01 |
| Staff              | 1996-02-11 |
| Assistant Engineer | 2000-07-31 |
| Assistant Engineer | 1990-02-18 |
| Engineer           | 1995-02-18 |
| Senior Engineer    | 9999-01-01 |
| Engineer           | 9999-01-01 |
+--------------------+------------+
15 rows in set (0.00 sec)



# 3. Alterar os registros do campo to_date para null da tabela cp_titles_date, quando o título for igual a Staff	

# Atualizando
update cp_titles_date set to_date = NULL where title = 'Staff';

Query OK, 107391 rows affected (0.84 sec)
Rows matched: 107391  Changed: 107391  Warnings: 0


# Consultando
select * from cp_titles_date limit 15;

+--------------------+------------+
| title              | to_date    |
+--------------------+------------+
| Senior Engineer    | 9999-01-01 |
| Staff              | NULL       |
| Senior Engineer    | 9999-01-01 |
| Engineer           | 1995-12-01 |
| Senior Engineer    | 9999-01-01 |
| Senior Staff       | 9999-01-01 |
| Staff              | NULL       |
| Senior Engineer    | 9999-01-01 |
| Senior Staff       | 9999-01-01 |
| Staff              | NULL       |
| Assistant Engineer | 2000-07-31 |
| Assistant Engineer | 1990-02-18 |
| Engineer           | 1995-02-18 |
| Senior Engineer    | 9999-01-01 |
| Engineer           | 9999-01-01 |
+--------------------+------------+
15 rows in set (0.00 sec)



Realizar com uso do Sqoop - Importações no warehouse /user/hive/warehouse/db_test_<numero_questao> e visualizar no HDFS


# 4. Importar a tabela titles com 8 mapeadores no formato parquet

# Sair do mysql: control + D 

# Sair do database: control + D 

# Acessar namenode
docker exec -it namenode bash

# Importando
sqoop import --table titles --connect jdbc:mysql://database/employees --username root --password secret -m 8 --as-parquetfile --warehouse-dir /user/hive/warehouse/db_test2_4 

# Validando
hdfs dfs -ls -h /user/hive/warehouse/db_test2_4/

Found 1 items
drwxr-xr-x   - root supergroup          0 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles


hdfs dfs -ls -h /user/hive/warehouse/db_test2_4/titles

# .parquet é um arquivo binário

Found 8 items
drwxr-xr-x   - root supergroup          0 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/.metadata
drwxr-xr-x   - root supergroup          0 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/.signals
-rw-r--r--   3 root supergroup    433.1 K 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/197e4fe1-781e-42b5-b3da-942f14ec2434.parquet
-rw-r--r--   3 root supergroup    642.2 K 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/4b97f71c-56c2-42ab-aff7-7335429038f2.parquet
-rw-r--r--   3 root supergroup    640.9 K 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/6cf45f14-c705-49c7-bb4d-86012f661229.parquet
-rw-r--r--   3 root supergroup    491.0 K 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/9debd631-6d12-426c-8186-a508deff2fe0.parquet
-rw-r--r--   3 root supergroup    429.3 K 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/a7016547-f22f-4d03-930b-70ab7e97ec01.parquet
-rw-r--r--   3 root supergroup    581.7 K 2021-05-06 18:53 /user/hive/warehouse/db_test2_4/titles/bb980233-ab66-459b-8ec1-406e116ec9b2.parquet

hdfs dfs -tail /user/hive/warehouse/db_test2_4/titles/197e4fe1-781e-42b5-b3da-942f14ec2434.parquet

�@�]��'iN0��������h(�-���@ߡ�hp���#$�~e,�2��,H�2ԩl_�l��u'L잶
                                                           �A[k�@��+(��Sl�]�2��m��z
               `I\Htitle%emp_no
                               %title%%	from_date%to_date��Lemp_no�����<C�S&��
          title������&��<Technique LeaderAssistant Engineer&��5 from_date������&���c�7n&��'to_date������&��'�nzp���p��@��parquet.avro.schema�{"type":"record","name":"titles","doc":"Sqoop import of titles","fields":[{"name":"emp_no","type":["null","int"],"default":null,"columnName":"emp_no","sqlType":"4"},{"name":"title","type":["null","string"],"default":null,"columnName":"title","sqlType":"12"},{"name":"from_date","type":["null","long"],"default":null,"columnName":"from_date","sqlType":"91"},{"name":"to_date","type":["null","long"],"default":null,"columnName":"to_date","sqlType":"91"}],"tableName":"titles"}
		  
		  
		  
# 5. Importar a tabela titles com 8 mapeadores no formato parquet e compressão snappy

sqoop import --table titles --connect jdbc:mysql://database/employees --username root --password secret -m 8 --as-parquetfile --warehouse-dir /user/hive/warehouse/db_test2_5 --compress --compression-codec org.apache.hadoop.io.compress.SnappyCodec


hdfs dfs -ls -h /user/hive/warehouse/db_test2_5/titles

Found 8 items
drwxr-xr-x   - root supergroup          0 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/.metadata
drwxr-xr-x   - root supergroup          0 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/.signals
-rw-r--r--   3 root supergroup    642.2 K 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/65302f67-bf5d-4058-b0ed-3fe3783d5387.parquet
-rw-r--r--   3 root supergroup    429.3 K 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/80e87ad0-39fc-4f94-8f88-960698f6eb25.parquet
-rw-r--r--   3 root supergroup    491.0 K 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/87ad634c-968f-4106-966f-d7f99ba062da.parquet
-rw-r--r--   3 root supergroup    433.1 K 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/9ab934c5-5bc4-463e-82a9-264161afc576.parquet
-rw-r--r--   3 root supergroup    581.7 K 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/f8d2f654-5db4-4462-8399-e9cd0779d9bf.parquet
-rw-r--r--   3 root supergroup    640.9 K 2021-05-06 19:03 /user/hive/warehouse/db_test2_5/titles/fbe93536-f306-49b3-b5a8-82be9c1698be.parquet


# 6. Importar a tabela cp_titles_date com 4 mapeadores (erro)

	# -m 4 informar é opcional, pois é o padrão
	sqoop import --table cp_title_date --connect jdbc:mysql://database/employees --username root --password secret -m 4  --warehouse-dir /user/hive/warehouse/db_test2_6
	
	21/05/06 19:17:55 ERROR manager.SqlManager: Error executing statement: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'employees.cp_title_date' doesn't exist
	com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'employees.cp_title_date' doesn't exist
		// ...
	21/05/06 19:17:55 ERROR tool.ImportTool: Import failed: java.io.IOException: No columns to generate for ClassWriter
		// ...
	
	
	# Listando as tabelas com sqoop
	sqoop list-tables --connect jdbc:mysql://database/employees --username root --password secret
	
	cp_titles_date
	current_dept_emp
	departments
	dept_emp
	dept_emp_latest_date
	dept_manager
	employees
	employees_2
	titles
	
	
	# Agora sim, com nome ok 
	sqoop import --table cp_titles_date --connect jdbc:mysql://database/employees --username root --password secret -m 4  --warehouse-dir /user/hive/warehouse/db_test2_6
	
	
	 # Não tem campo primário
	 
	 cp_titles_date
		+-----------------+------------+
		| title           | to_date    |


	21/05/06 19:22:24 ERROR tool.ImportTool: Import failed: No primary key could be found for table cp_titles_date. 
	Please specify one with --split-by or perform a sequential import with '-m 1'.
	
	

	A. Importar a tabela cp_titles_date com 4 mapeadores divididos pelo campo título no warehouse /user/hive/warehouse/db_test2_title
			
	# Agora import correto	
	# Ou cria direório novo para não dar erro ou usa delete-target-dir
	sqoop import --table cp_titles_date --connect jdbc:mysql://database/employees --username root --password secret -m 4  --warehouse-dir /user/hive/warehouse/db_test2_title  --split-by title
	
	Caused by: Generating splits for a textual index column allowed only in case of "-Dorg.apache.sqoop.splitter.allow_text_splitter=true" property passed as a parameter
	
	
	sqoop import -Dorg.apache.sqoop.splitter.allow_text_splitter=true --table cp_titles_date --connect jdbc:mysql://database/employees --username root --password secret -m 4  --warehouse-dir /user/hive/warehouse/db_test2_title  --split-by title
	
	
	21/05/06 19:34:15 INFO mapreduce.ImportJobBase: Transferred 22.8818 MB in 5.9855 seconds (3.8229 MB/sec)
	21/05/06 19:34:15 INFO mapreduce.ImportJobBase: Retrieved 443308 records.

	

	B. Importar a tabela cp_titles_date com 4 mapeadores divididos pelo campo data no warehouse /user/hive/warehouse/db_test2_date
	
	# -Dorg.apache.sqoop.splitter.allow_text_splitter=true só precisou por causa da configuração do cluster
	sqoop import -Dorg.apache.sqoop.splitter.allow_text_splitter=true --table cp_titles_date --connect jdbc:mysql://database/employees --username root --password secret -m 4  --warehouse-dir /user/hive/warehouse/db_test2_date  --split-by to_date


	21/05/06 19:38:07 INFO mapreduce.ImportJobBase: Transferred 15.3773 MB in 7.0724 seconds (2.1743 MB/sec)
	21/05/06 19:38:07 INFO mapreduce.ImportJobBase: Retrieved 335917 records.	
	
	
	
	C. Qual a diferença dos registros nulos entre as duas importações?
	
	################ DATE ################
	
	hdfs dfs -count -h /user/hive/warehouse/db_test2_date
	
	# 2 diretórios, 5 arquivos e 7,7 megas
  	2            5              7.7 M /user/hive/warehouse/db_test2_date
	
	
	
	hdfs dfs -ls -h -R /user/hive/warehouse/db_test2_date
	
	drwxr-xr-x   - root supergroup          0 2021-05-06 19:38 /user/hive/warehouse/db_test2_date/cp_titles_date
	-rw-r--r--   3 root supergroup          0 2021-05-06 19:38 /user/hive/warehouse/db_test2_date/cp_titles_date/_SUCCESS
	-rw-r--r--   3 root supergroup      2.6 M 2021-05-06 19:38 /user/hive/warehouse/db_test2_date/cp_titles_date/part-m-00000
	-rw-r--r--   3 root supergroup          0 2021-05-06 19:38 /user/hive/warehouse/db_test2_date/cp_titles_date/part-m-00001
	-rw-r--r--   3 root supergroup          0 2021-05-06 19:38 /user/hive/warehouse/db_test2_date/cp_titles_date/part-m-00002
	-rw-r--r--   3 root supergroup      5.1 M 2021-05-06 19:38 /user/hive/warehouse/db_test2_date/cp_titles_date/part-m-00003	


	################ TITLE ################
	
	hdfs dfs -count -h /user/hive/warehouse/db_test2_title
	
	2            7              8.8 M /user/hive/warehouse/db_test2_title
	
	
	
	hdfs dfs -ls -h -R /user/hive/warehouse/db_test2_title
	
	drwxr-xr-x   - root supergroup          0 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date
	-rw-r--r--   3 root supergroup          0 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date/_SUCCESS
	-rw-r--r--   3 root supergroup          0 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00000
	-rw-r--r--   3 root supergroup    443.2 K 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00001
	-rw-r--r--   3 root supergroup      2.2 M 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00002
	-rw-r--r--   3 root supergroup        456 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00003
	-rw-r--r--   3 root supergroup      5.8 M 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00004
	-rw-r--r--   3 root supergroup    414.5 K 2021-05-06 19:34 /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00005
	

	################ db_test2_date ################
	
	hdfs dfs -cat  /user/hive/warehouse/db_test2_date/cp_titles_date/part-m-00000 | head -n 10

	// Olhar todos arquivos, NENHUM vai ter Staff pq fez split pelo 'to_date' que é NULL
	// Questão 3: update cp_titles_date set to_date = NULL where title = 'Staff'
	
	Engineer,1995-12-01
	Assistant Engineer,2000-07-31
	Assistant Engineer,1990-02-18
	Engineer,1995-02-18
	Engineer,2000-12-18
	Senior Staff,1993-08-22
	Engineer,1995-04-03
	Technique Leader,2002-07-15
	Technique Leader,1997-10-15
	Engineer,2001-03-19


	################ db_test2_title ################
	
	# Olhar todos arquivos, algum vai ter Staff, null 
	hdfs dfs -cat  /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00004 | head -n 10
	
	Senior Engineer,9999-01-01
	Staff,null
	Senior Engineer,9999-01-01
	Senior Engineer,9999-01-01
	Senior Staff,9999-01-01
	Staff,null
	Senior Engineer,9999-01-01
	Senior Staff,9999-01-01
	Staff,null
	Senior Engineer,9999-01-01	
	
	# Nada
	hdfs dfs -cat  /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00000 | head -n 10
	
	# Assistant Engineer
	hdfs dfs -cat  /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00001 | head -n 10
	Assistant Engineer,2000-07-31
	Assistant Engineer,1990-02-18
	Assistant Engineer,9999-01-01
	Assistant Engineer,1992-02-26
	Assistant Engineer,2002-01-02
	Assistant Engineer,1999-01-15
	Assistant Engineer,1999-01-04
	Assistant Engineer,9999-01-01
	Assistant Engineer,1995-06-08
	Assistant Engineer,1991-05-17
	cat: Unable to write to output stream.
	
	# Engineer
	hdfs dfs -cat  /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00002 | head -n 10
	Engineer,1995-12-01
	Engineer,1995-02-18
	Engineer,9999-01-01
	Engineer,2000-12-18
	Engineer,9999-01-01
	Engineer,1995-04-03
	Engineer,9999-01-01
	Engineer,9999-01-01
	Engineer,9999-01-01
	Engineer,2001-03-19
	cat: Unable to write to output stream.
	
	# Manager
	hdfs dfs -cat  /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00003 | head -n 10
	Manager,1991-10-01
	Manager,9999-01-01
	Manager,1989-12-17
	Manager,9999-01-01
	Manager,1992-03-21
	Manager,9999-01-01
	Manager,1988-09-09
	Manager,1992-08-02
	Manager,1996-08-30
	Manager,9999-01-01
	
	# Senior Engineer
	hdfs dfs -cat  /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00004 | head -n 10
	Senior Engineer,9999-01-01
	Staff,null
	Senior Engineer,9999-01-01
	Senior Engineer,9999-01-01
	Senior Staff,9999-01-01
	Staff,null
	Senior Engineer,9999-01-01
	Senior Staff,9999-01-01
	Staff,null
	Senior Engineer,9999-01-01
	cat: Unable to write to output stream.
	
	# Technique Leader
	hdfs dfs -cat  /user/hive/warehouse/db_test2_title/cp_titles_date/part-m-00005 | head -n 10
	Technique Leader,2002-07-15
	Technique Leader,1997-10-15
	Technique Leader,1993-03-24
	Technique Leader,9999-01-01
	Technique Leader,9999-01-01
	Technique Leader,9999-01-01
	Technique Leader,9999-01-01
	Technique Leader,9999-01-01
	Technique Leader,9999-01-01
	Technique Leader,9999-01-01
	cat: Unable to write to output stream.
	

```