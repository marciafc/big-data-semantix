# Sqoop - Importação para o Hive e Exportação - BD Employees


```

docker-compose up -d

docker ps

docker exec -it namenode bash


# 1. Importar a tabela employees.titles do MySQL para o diretório /user/aluno/<nome>/data com 1 mapeador.


sqoop import --table titles --connect jdbc:mysql://database/employees --username root --password secret --warehouse-dir /user/aluno/marcia-castagna/data -m 1 


hdfs dfs -ls /user/aluno/marcia-castagna/data/titles

	Found 2 items
	-rw-r--r--   3 root supergroup          0 2021-05-09 02:15 /user/aluno/marcia-castagna/data/titles/_SUCCESS
	-rw-r--r--   3 root supergroup   17718376 2021-05-09 02:15 /user/aluno/marcia-castagna/data/titles/part-m-00000
	
	

# 2. Importar a tabela employees.titles do MySQL para uma tabela Hive no banco de dados seu nome com 1 mapeador.

# Se fizer assim, vai enviar para o default, que é o root (NÃO é o que pede o exercício)
# sqoop import --table titles --connect jdbc:mysql://database/employees --username root --password secret -m 1 --hive-import 

sqoop import --table titles --connect jdbc:mysql://database/employees --username root --password secret -m 1 --hive-import --hive-table  marcia.titles

hdfs dfs -ls /user/root/titles

	ls: `/user/root/titles': No such file or directory

hdfs dfs -ls /user/hive/warehouse/marcia.db

	Found 4 items
	drwxrwxr-x   - root supergroup          0 2021-05-03 01:11 /user/hive/warehouse/marcia.db/pop
	drwxrwxr-x   - root supergroup          0 2021-05-05 03:28 /user/hive/warehouse/marcia.db/pop_parquet
	drwxrwxr-x   - root supergroup          0 2021-05-05 03:39 /user/hive/warehouse/marcia.db/pop_parquet_snappy
	drwxrwxr-x   - root supergroup          0 2021-05-09 02:21 /user/hive/warehouse/marcia.db/titles


hdfs dfs -ls /user/hive/warehouse/marcia.db/titles

	Found 1 items
	-rwxrwxr-x   3 root supergroup   17718376 2021-05-09 02:21 /user/hive/warehouse/marcia.db/titles/part-m-00000

hdfs dfs -cat /user/hive/warehouse/marcia.db/titles/part-m-00000

	// ...
	499996Engineer1996-05-132002-05-13
	499996Senior Engineer2002-05-139999-01-01
	499997Engineer1987-08-301992-08-29
	499997Senior Engineer1992-08-299999-01-01
	499998Senior Staff1998-12-279999-01-01
	499998Staff1993-12-271998-12-27
	499999Engineer1997-11-309999-01-01
	
# Os 10 primeiros registros	
hdfs dfs -cat /user/hive/warehouse/marcia.db/titles/part-m-00000 | head -n 10	

	10001Senior Engineer1986-06-269999-01-01
	10002Staff1996-08-039999-01-01
	10003Senior Engineer1995-12-039999-01-01
	10004Engineer1986-12-011995-12-01
	10004Senior Engineer1995-12-019999-01-01
	10005Senior Staff1996-09-129999-01-01
	10005Staff1989-09-121996-09-12
	10006Senior Engineer1990-08-059999-01-01
	10007Senior Staff1996-02-119999-01-01
	10007Staff1989-02-101996-02-11




# 3. Selecionar os 10 primeiros registros da tabela titles no Hive.

# Sair do namenode (sqoop)
control + d

docker exec -it hive-server bash

beeline -help
beeline -u jdbc:hive2://localhost:10000

use marcia;

select * from titles limit 10;

+----------------+------------------+-------------------+-----------------+
| titles.emp_no  |   titles.title   | titles.from_date  | titles.to_date  |
+----------------+------------------+-------------------+-----------------+
| 10001          | Senior Engineer  | 1986-06-26        | 9999-01-01      |
| 10002          | Staff            | 1996-08-03        | 9999-01-01      |
| 10003          | Senior Engineer  | 1995-12-03        | 9999-01-01      |
| 10004          | Engineer         | 1986-12-01        | 1995-12-01      |
| 10004          | Senior Engineer  | 1995-12-01        | 9999-01-01      |
| 10005          | Senior Staff     | 1996-09-12        | 9999-01-01      |
| 10005          | Staff            | 1989-09-12        | 1996-09-12      |
| 10006          | Senior Engineer  | 1990-08-05        | 9999-01-01      |
| 10007          | Senior Staff     | 1996-02-11        | 9999-01-01      |
| 10007          | Staff            | 1989-02-10        | 1996-02-11      |
+----------------+------------------+-------------------+-----------------+
10 rows selected (1.331 seconds)



#  4. Deletar os registros da tabela employees.titles do MySQL e verificar se foram apagados, através do Sqoop

# Sair do beeline e do hive-server

docker exec -it namenode bash

sqoop eval --connect jdbc:mysql://database/employees --username root --password secret --query "select * from  titles limit 10"

	----------------------------------------------------------------
	| emp_no      | title                | from_date  | to_date    | 
	----------------------------------------------------------------
	| 10001       | Senior Engineer      | 1986-06-26 | 9999-01-01 | 
	| 10002       | Staff                | 1996-08-03 | 9999-01-01 | 
	| 10003       | Senior Engineer      | 1995-12-03 | 9999-01-01 | 
	| 10004       | Engineer             | 1986-12-01 | 1995-12-01 | 
	| 10004       | Senior Engineer      | 1995-12-01 | 9999-01-01 | 
	| 10005       | Senior Staff         | 1996-09-12 | 9999-01-01 | 
	| 10005       | Staff                | 1989-09-12 | 1996-09-12 | 
	| 10006       | Senior Engineer      | 1990-08-05 | 9999-01-01 | 
	| 10007       | Senior Staff         | 1996-02-11 | 9999-01-01 | 
	| 10007       | Staff                | 1989-02-10 | 1996-02-11 | 
	----------------------------------------------------------------


sqoop eval --connect jdbc:mysql://database/employees --username root --password secret --query "truncate table titles"

	----------------------------------------------------------------
	| emp_no      | title                | from_date  | to_date    | 
	----------------------------------------------------------------
	----------------------------------------------------------------|


# 5. Exportar os dados do diretório /user/hive/warehouse/<nome>.db/data/titles para a tabela do MySQL  employees.titles.

sqoop export --table titles --connect jdbc:mysql://database/employees --username root --password secret --export-dir /user/aluno/marcia-castagna/data/titles



# 6. Selecionar os 10 primeiros registros registros da tabela employees.titles do MySQL.


sqoop eval --connect jdbc:mysql://database/employees --username root --password secret --query "select * from  titles limit 10"

----------------------------------------------------------------
| emp_no      | title                | from_date  | to_date    | 
----------------------------------------------------------------
| 10001       | Senior Engineer      | 1986-06-26 | 9999-01-01 | 
| 10002       | Staff                | 1996-08-03 | 9999-01-01 | 
| 10003       | Senior Engineer      | 1995-12-03 | 9999-01-01 | 
| 10004       | Engineer             | 1986-12-01 | 1995-12-01 | 
| 10004       | Senior Engineer      | 1995-12-01 | 9999-01-01 | 
| 10005       | Senior Staff         | 1996-09-12 | 9999-01-01 | 
| 10005       | Staff                | 1989-09-12 | 1996-09-12 | 
| 10006       | Senior Engineer      | 1990-08-05 | 9999-01-01 | 
| 10007       | Senior Staff         | 1996-02-11 | 9999-01-01 | 
| 10007       | Staff                | 1989-02-10 | 1996-02-11 | 
----------------------------------------------------------------


control + d

docker-compose stop

```