# Preparar BD

```

docker-compose up -d

docker ps

ls input/exercises-data/db-sql/
Changelog                      images                  load_employees.dump  load_titles.dump  show_elapsed.sql
employees_partitioned_5.1.sql  load_departments.dump   load_salaries1.dump  objects.sql       sql_test.sh
employees_partitioned.sql      load_dept_emp.dump      load_salaries2.dump  README.md         test_employees_md5.sql
employees.sql                  load_dept_manager.dump  load_salaries3.dump  sakila            test_employees_sha.sql


ls input/exercises-data/db-sql/sakila/
insert_rental.sql  README.md  sakila-mv-data.sql  sakila-mv-schema.sql

# Container 'database' é onde está o banco de dados
# database:/ o barra indica a raiz
1. Copiar os dados do local para o contêiner database

	$ docker cp input/exercises-data/db-sql/ database:/

2. Acessar o contêiner database

	$ docker exec -it database bash
	
ls /db-sql/

Changelog		   employees_partitioned_5.1.sql  load_dept_manager.dump  load_salaries3.dump  show_elapsed.sql
README.md		   images			  load_employees.dump	  load_titles.dump     sql_test.sh
employees.sql		   load_departments.dump	  load_salaries1.dump	  objects.sql	       test_employees_md5.sql
employees_partitioned.sql  load_dept_emp.dump		  load_salaries2.dump	  sakila	       test_employees_sha.sql	


3. Instalar Banco de Dados de testes	

	3.1 Diretório /db-sql - BD employees (Já existe - só fazer se NÃO estiver usando o ambiente do curso)		
  		
		$ cd /db-sql  
		
		# Conectar mysql e executar script
  		$ mysql -psecret < employees.sql

	3.2 Diretório /db-sql/sakila - BD sakila
	    
		# Sair do mysql, caso esteja nele
		mysql> control + D
		
		$ cd /db-sql/sakila/
		
		# Conectar mysql e executar os scripts (schema e dados do banco)
		$ mysql -psecret < sakila-mv-schema.sql
		$ mysql -psecret < sakila-mv-data.sql
		
		mysql -psecret 
		use employees;
		show tables;
		
		+----------------------+
		| Tables_in_employees  |
		+----------------------+
		| current_dept_emp     |
		| departments          |
		| dept_emp             |
		| dept_emp_latest_date |
		| dept_manager         |
		| employees            |
		| employees_2          |
		| titles               |
		+----------------------+
		8 rows in set (0.00 sec)

		select * from employees limit 10;
		
		+--------+------------+------------+-----------+--------+------------+
		| emp_no | birth_date | first_name | last_name | gender | hire_date  |
		+--------+------------+------------+-----------+--------+------------+
		|  10001 | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
		|  10002 | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
		|  10003 | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
		|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |
		|  10005 | 1955-01-21 | Kyoichi    | Maliniak  | M      | 1989-09-12 |
		|  10006 | 1953-04-20 | Anneke     | Preusig   | F      | 1989-06-02 |
		|  10007 | 1957-05-23 | Tzvetan    | Zielinski | F      | 1989-02-10 |
		|  10008 | 1958-02-19 | Saniya     | Kalloufi  | M      | 1994-09-15 |
		|  10009 | 1952-04-19 | Sumant     | Peac      | F      | 1985-02-18 |
		|  10010 | 1963-06-01 | Duangkaew  | Piveteau  | F      | 1989-08-24 |
		+--------+------------+------------+-----------+--------+------------+
		10 rows in set (0.00 sec)

		mysql> use sakilla;
	
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
		23 rows in set (0.01 sec)

		select * from rental limit 10;
	
		+-----------+---------------------+--------------+-------------+---------------------+----------+---------------------+
		| rental_id | rental_date         | inventory_id | customer_id | return_date         | staff_id | last_update         |
		+-----------+---------------------+--------------+-------------+---------------------+----------+---------------------+
		|         1 | 2005-05-24 22:53:30 |          367 |         130 | 2005-05-26 22:04:30 |        1 | 2006-02-15 21:30:53 |
		|         2 | 2005-05-24 22:54:33 |         1525 |         459 | 2005-05-28 19:40:33 |        1 | 2006-02-15 21:30:53 |
		|         3 | 2005-05-24 23:03:39 |         1711 |         408 | 2005-06-01 22:12:39 |        1 | 2006-02-15 21:30:53 |
		|         4 | 2005-05-24 23:04:41 |         2452 |         333 | 2005-06-03 01:43:41 |        2 | 2006-02-15 21:30:53 |
		|         5 | 2005-05-24 23:05:21 |         2079 |         222 | 2005-06-02 04:33:21 |        1 | 2006-02-15 21:30:53 |
		|         6 | 2005-05-24 23:08:07 |         2792 |         549 | 2005-05-27 01:32:07 |        1 | 2006-02-15 21:30:53 |
		|         7 | 2005-05-24 23:11:53 |         3995 |         269 | 2005-05-29 20:34:53 |        2 | 2006-02-15 21:30:53 |
		|         8 | 2005-05-24 23:31:46 |         2346 |         239 | 2005-05-27 23:33:46 |        2 | 2006-02-15 21:30:53 |
		|         9 | 2005-05-25 00:00:40 |         2580 |         126 | 2005-05-28 00:22:40 |        1 | 2006-02-15 21:30:53 |
		|        10 | 2005-05-25 00:02:21 |         1824 |         399 | 2005-05-31 22:44:21 |        2 | 2006-02-15 21:30:53 |
		+-----------+---------------------+--------------+-------------+---------------------+----------+---------------------+
		10 rows in set (0.00 sec)


# Conectar ao mysql
      # '-h localhost' não precisa informar, já está em localhost
	  # '-u root' tb não, já está como root
	  # só precisa informar a senha com -p (-psecret)
	  mysql -h localhost -u root -psecret
	  # OU
	  mysql -psecret
	
	  //...
	  mysql>	
	
	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| employees          |
	| hue                |
	| mysql              |
	| performance_schema |
	| sys                |
	+--------------------+
	6 rows in set (0.01 sec)	
	
	

# Caso ocorra algum problema com os databases

mysql> drop database employees;

E então executar o passo 3.1


mysql> drop database sakila;

E então executar o passo 3.2


```

Após sair do mysql e do container 'database', acessar 'namenode'

```
docker exec -it namenode bash

# Importando do database para o datanode (próximas aulas)
# root@namenode:/# sqoop import database... -> hdfs(datanode)

```
