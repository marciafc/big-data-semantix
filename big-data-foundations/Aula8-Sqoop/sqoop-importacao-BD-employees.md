# Sqoop - Importação BD Employees

```

docker-compose up -d

docker ps

# sqoop está no namenode
docker exec -it namenode bash


# 1. Pesquisar os 10 primeiros registros da tabela employees do banco de dados employees

# database: é o nome do container onde está o banco de dados relacional
sqoop eval  --connect jdbc:mysql://database/employees --username root --password secret --query "select * from employees limit 10"

---------------------------------------------------------------------------------
| emp_no      | birth_date | first_name     | last_name        | gender | hire_date  | 
---------------------------------------------------------------------------------
| 10001       | 1953-09-02 | Georgi         | Facello          | M | 1986-06-26 | 
| 10002       | 1964-06-02 | Bezalel        | Simmel           | F | 1985-11-21 | 
| 10003       | 1959-12-03 | Parto          | Bamford          | M | 1986-08-28 | 
| 10004       | 1954-05-01 | Chirstian      | Koblick          | M | 1986-12-01 | 
| 10005       | 1955-01-21 | Kyoichi        | Maliniak         | M | 1989-09-12 | 
| 10006       | 1953-04-20 | Anneke         | Preusig          | F | 1989-06-02 | 
| 10007       | 1957-05-23 | Tzvetan        | Zielinski        | F | 1989-02-10 | 
| 10008       | 1958-02-19 | Saniya         | Kalloufi         | M | 1994-09-15 | 
| 10009       | 1952-04-19 | Sumant         | Peac             | F | 1985-02-18 | 
| 10010       | 1963-06-01 | Duangkaew      | Piveteau         | F | 1989-08-24 | 
---------------------------------------------------------------------------------



# 2. Realizar as importações referentes a tabela employees e para validar cada questão,  é necessário visualizar no HDFS*

	#  A. Importar a tabela employees, no warehouse  /user/hive/warehouse/db_test_a
	
	sqoop import --table employees --connect jdbc:mysql://database/employees --username root --password secret --warehouse-dir /user/hive/warehouse/db_test_a
	
	# Validar no hdfs
	hdfs dfs -ls /user/hive/warehouse/db_test_a
	
	Found 1 items
	drwxr-xr-x   - root supergroup          0 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees
	
	
	hdfs dfs -ls /user/hive/warehouse/db_test_a/employees
	
	Found 5 items
	-rw-r--r--   3 root supergroup          0 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/_SUCCESS
	-rw-r--r--   3 root supergroup    4548041 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00000
	-rw-r--r--   3 root supergroup    2550561 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00001
	-rw-r--r--   3 root supergroup    2086360 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00002
	-rw-r--r--   3 root supergroup    4637031 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00003
	
	
	hdfs dfs -cat /user/hive/warehouse/db_test_a/employees/part-m-00000 | head -n 3
	
	10001,1953-09-02,Georgi,Facello,M,1986-06-26
	10002,1964-06-02,Bezalel,Simmel,F,1985-11-21
	10003,1959-12-03,Parto,Bamford,M,1986-08-28
	cat: Unable to write to output stream.	
	
	
	hdfs dfs -cat /user/hive/warehouse/db_test_a/employees/part-m-00001 | head -n 3
	200000,1960-01-11,Selwyn,Koshiba,M,1987-06-05
	200001,1957-09-10,Bedrich,Markovitch,M,1985-11-22
	200002,1961-02-07,Pascal,Benzmuller,F,1986-03-12
	cat: Unable to write to output stream.
	
	hdfs dfs -ls -h /user/hive/warehouse/db_test_a/employees

	Found 5 items
	-rw-r--r--   3 root supergroup          0 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/_SUCCESS
	-rw-r--r--   3 root supergroup      4.3 M 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00000
	-rw-r--r--   3 root supergroup      2.4 M 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00001
	-rw-r--r--   3 root supergroup      2.0 M 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00002
	-rw-r--r--   3 root supergroup      4.4 M 2021-05-05 23:01 /user/hive/warehouse/db_test_a/employees/part-m-00003
	
	
	
	#  B. Importar todos os funcionários do gênero masculino, no warehouse  /user/hive/warehouse/db_test_b
	
	# Consultando para ver nome da coluna 
	sqoop eval  --connect jdbc:mysql://database/employees --username root --password secret --query "select * from employees limit 10"
	
	---------------------------------------------------------------------------------
	| emp_no      | birth_date | first_name     | last_name        | gender | hire_date  | 
	---------------------------------------------------------------------------------
	| 10001       | 1953-09-02 | Georgi         | Facello          | M | 1986-06-26 | 
	| 10002       | 1964-06-02 | Bezalel        | Simmel           | F | 1985-11-21 | 
	| 10003       | 1959-12-03 | Parto          | Bamford          | M | 1986-08-28 | 
	| 10004       | 1954-05-01 | Chirstian      | Koblick          | M | 1986-12-01 | 
	| 10005       | 1955-01-21 | Kyoichi        | Maliniak         | M | 1989-09-12 | 
	| 10006       | 1953-04-20 | Anneke         | Preusig          | F | 1989-06-02 | 
	| 10007       | 1957-05-23 | Tzvetan        | Zielinski        | F | 1989-02-10 | 
	| 10008       | 1958-02-19 | Saniya         | Kalloufi         | M | 1994-09-15 | 
	| 10009       | 1952-04-19 | Sumant         | Peac             | F | 1985-02-18 | 
	| 10010       | 1963-06-01 | Duangkaew      | Piveteau         | F | 1989-08-24 | 
	---------------------------------------------------------------------------------	
	
	# Importando
	sqoop import --table employees --connect jdbc:mysql://database/employees --username root --password secret --where "gender='M'" --warehouse-dir /user/hive/warehouse/db_test_b

	# Visualizando o hdfs
	hdfs dfs -ls -h /user/hive/warehouse/db_test_b/employees
	
	Found 5 items
	-rw-r--r--   3 root supergroup          0 2021-05-05 23:17 /user/hive/warehouse/db_test_b/employees/_SUCCESS
	-rw-r--r--   3 root supergroup      2.6 M 2021-05-05 23:17 /user/hive/warehouse/db_test_b/employees/part-m-00000
	-rw-r--r--   3 root supergroup      1.5 M 2021-05-05 23:17 /user/hive/warehouse/db_test_b/employees/part-m-00001
	-rw-r--r--   3 root supergroup      1.2 M 2021-05-05 23:17 /user/hive/warehouse/db_test_b/employees/part-m-00002
	-rw-r--r--   3 root supergroup      2.7 M 2021-05-05 23:17 /user/hive/warehouse/db_test_b/employees/part-m-00003	
	
	# Leitura dos dados
	hdfs dfs -cat /user/hive/warehouse/db_test_b/employees/part-m-00000 | head -n 5
	
	10001,1953-09-02,Georgi,Facello,M,1986-06-26
	10003,1959-12-03,Parto,Bamford,M,1986-08-28
	10004,1954-05-01,Chirstian,Koblick,M,1986-12-01
	10005,1955-01-21,Kyoichi,Maliniak,M,1989-09-12
	10008,1958-02-19,Saniya,Kalloufi,M,1994-09-15
	cat: Unable to write to output stream.
	
	
	# C. importar o primeiro e o último nome dos funcionários com os campos separados por tabulação, no warehouse  /user/hive/warehouse/db_test_c
	
	sqoop eval  --connect jdbc:mysql://database/employees --username root --password secret --query "select * from employees limit 10"
	
		---------------------------------------------------------------------------------
	| emp_no      | birth_date | first_name     | last_name        | gender | hire_date  | 
	---------------------------------------------------------------------------------
	| 10001       | 1953-09-02 | Georgi         | Facello          | M | 1986-06-26 | 
	| 10002       | 1964-06-02 | Bezalel        | Simmel           | F | 1985-11-21 | 
	| 10003       | 1959-12-03 | Parto          | Bamford          | M | 1986-08-28 | 
	| 10004       | 1954-05-01 | Chirstian      | Koblick          | M | 1986-12-01 | 
	| 10005       | 1955-01-21 | Kyoichi        | Maliniak         | M | 1989-09-12 | 
	| 10006       | 1953-04-20 | Anneke         | Preusig          | F | 1989-06-02 | 
	| 10007       | 1957-05-23 | Tzvetan        | Zielinski        | F | 1989-02-10 | 
	| 10008       | 1958-02-19 | Saniya         | Kalloufi         | M | 1994-09-15 | 
	| 10009       | 1952-04-19 | Sumant         | Peac             | F | 1985-02-18 | 
	| 10010       | 1963-06-01 | Duangkaew      | Piveteau         | F | 1989-08-24 | 
	---------------------------------------------------------------------------------
	
	# Importando
	sqoop import --table employees --connect jdbc:mysql://database/employees --username root --password secret --columns "first_name,last_name" --fields-terminated-by '\t'  --warehouse-dir /user/hive/warehouse/db_test_c
	
	hdfs dfs -cat /user/hive/warehouse/db_test_c/employees/part-m-00000 | head -n 10

	# Só o first_name e o last_name (que foram as únicas colunas importadas)
	# Observar que as colunas estão separadas por tabulação \t
	Georgi	Facello
	Bezalel	Simmel
	Parto	Bamford
	Chirstian	Koblick
	Kyoichi	Maliniak
	Anneke	Preusig
	Tzvetan	Zielinski
	Saniya	Kalloufi
	Sumant	Peac
	Duangkaew	Piveteau
	cat: Unable to write to output stream.
	
	
	
	# D. Importar o primeiro e o último nome dos funcionários com as linhas separadas por " : " e salvar no mesmo diretório da questão 2.C

	// Vai dar erro, pois já existe o diretório (não executou o map reduce)
	// 21/05/05 23:40:39 ERROR tool.ImportTool: Import failed: org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory hdfs://namenode:8020/user/hive/warehouse/db_test_c/employees already exists
	sqoop import --table employees --connect jdbc:mysql://database/employees --username root --password secret --columns "first_name,last_name" --lines-terminated-by ':'  --warehouse-dir /user/hive/warehouse/db_test_c
	
	
	// Usar append (adicionar) OU delete (apaga o que tinha antes)
	// Neste caso, usar delete, pois o que já tem, está com outro formato
	sqoop import --table employees --connect jdbc:mysql://database/employees --username root --password secret --columns "first_name,last_name" --lines-terminated-by ':'  --warehouse-dir /user/hive/warehouse/db_test_c -delete-target-dir

	hdfs dfs -cat /user/hive/warehouse/db_test_c/employees/part-m-00000 | head -n 10
	
	// Trecho do resultado (first_name,last_name - LINHAS SEPARADAS POR : )
	Tonny,Cooman:Mingdong,Hagimont:Duri,Aloia


* Dica para visualizar no HDFS:

$ hdfs dfs -cat /.../db_test/nomeTabela/part-m-00000 | head -n 5



docker-compose stop

```