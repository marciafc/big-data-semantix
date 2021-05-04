# Hive - Criação de Tabela Particionada

```

docker-compose up -d

# Pode fazer assim e ir informando os comandos, OU diretamente, como a seguir (sem o uso do bash)
docker exec -it namenode bash


# Sistema de arquivo local
docker exec -it namenode ls /

bin	   employees.java  hadoop-data	lib64	      opt   run.sh  tmp
boot	   entrypoint.sh   home		media	      proc  sbin    usr
derby.log  etc		   input	metastore_db  root  srv     var
dev	   hadoop	   lib		mnt	      run   sys



# Sistema de arquivo do hdfs
docker exec -it namenode hdfs dfs -ls /

Found 2 items
drwxrwxr-x   - root supergroup          0 2021-05-02 17:36 /tmp
drwxr-xr-x   - root supergroup          0 2021-05-02 17:39 /user



# 1. Criar o diretório "/user/aluno/<nome>/data/nascimento" no HDFS

docker exec -it namenode hdfs dfs -mkdir /user/aluno/marcia-castagna/data/nascimento

# Confirmando a criação do diretório 'nascimento'
docker exec -it namenode hdfs dfs -ls /user/aluno/marcia-castagna/data/

Found 3 items
drwxr-xr-x   - root supergroup          0 2021-05-02 17:49 /user/aluno/marcia-castagna/data/escola
drwxr-xr-x   - root supergroup          0 2021-05-04 03:36 /user/aluno/marcia-castagna/data/nascimento
-rw-r--r--   2 root supergroup          0 2021-05-02 19:01 /user/aluno/marcia-castagna/data/test



# 2. Criar e usar o Banco de dados <nome>

// Já foi criado em outro exercício

docker exec -it hive-server bash
	
beeline -help

beeline -u jdbc:hive2://localhost:10000

show databases;	

+----------------+
| database_name  |
+----------------+
| default        |
| marcia         |
+----------------+
2 rows selected (0.932 seconds)


# 3. Criar uma tabela externa no Hive com os parâmetros:

	a) Tabela: nascimento

	b) Campos: nome (String), sexo (String) e frequencia (int)

	c) Partição: ano

	d) Delimitadores:

		i) Campo ‘,’

		ii)  Linha ‘\n’

	e) Salvar

		i) Tipo do arquivo: texto

		ii) HDFS: '/user/aluno/<nome>/data/nascimento’


show databases;	

use marcia;

show tables;
+-----------+
| tab_name  |
+-----------+
| pop       |
+-----------+
1 row selected (0.062 seconds)


# Criando tabela externa
# A partição vai ser um diretório (campo ano - não fica na estrutura da tabela)
create external table nascimento (
    nome string,
	sexo string,
	frequencia int
)
partitioned by (ano int)    
row format delimited
fields terminated by ','
lines terminated by '\n'
stored as textfile
location '/user/aluno/marcia-castagna/data/nascimento';

No rows affected (0.811 seconds)


# Em alguns cluster, pode estar assim:
#    location 'hdfs://namenode:8020/user/aluno/marcia-castagna/data/nascimento';


# Verificando as tabelas
show tables;

+-------------+
|  tab_name   |
+-------------+
| nascimento  |
| pop         |
+-------------+
2 rows selected (0.08 seconds)


# 4.Adicionar partição ano=2015

# Irá criar diretório ano=2015
alter table nascimento add partition(ano = 2015);

# Tabela está vazia (só está mapeando um diretório que está vazio)
select * from nascimento;

# Validação: 
  -> Listar a pasta /user/aluno/marcia-castagna/data/nascimento para validar se criou o diretório ano=2015
  
 # Sair do beeline e do hive (2x control + D)

 # Acessar hdfs
 docker exec -it namenode bash

 hdfs dfs -ls /user/aluno/marcia-castagna/data/nascimento

 Found 1 items
 drwxr-xr-x   - root supergroup          0 2021-05-04 05:18 /user/aluno/marcia-castagna/data/nascimento/ano=2015



# 5.Enviar o arquivo local "input/exercises-data/names/yob2015.txt" para o HDFS no diretório /user/aluno/<nome>/data/nascimento/ano=2015

ls /input/exercises-data/names

# Crianças que nasceram de 1880 até 2017:

NationalReadMe.pdf  yob1893.txt  yob1907.txt  yob1921.txt  yob1935.txt	yob1949.txt  yob1963.txt  yob1977.txt  yob1991.txt  yob2005.txt
yob1880.txt	    yob1894.txt  yob1908.txt  yob1922.txt  yob1936.txt	yob1950.txt  yob1964.txt  yob1978.txt  yob1992.txt  yob2006.txt
yob1881.txt	    yob1895.txt  yob1909.txt  yob1923.txt  yob1937.txt	yob1951.txt  yob1965.txt  yob1979.txt  yob1993.txt  yob2007.txt
yob1882.txt	    yob1896.txt  yob1910.txt  yob1924.txt  yob1938.txt	yob1952.txt  yob1966.txt  yob1980.txt  yob1994.txt  yob2008.txt
yob1883.txt	    yob1897.txt  yob1911.txt  yob1925.txt  yob1939.txt	yob1953.txt  yob1967.txt  yob1981.txt  yob1995.txt  yob2009.txt
yob1884.txt	    yob1898.txt  yob1912.txt  yob1926.txt  yob1940.txt	yob1954.txt  yob1968.txt  yob1982.txt  yob1996.txt  yob2010.txt
yob1885.txt	    yob1899.txt  yob1913.txt  yob1927.txt  yob1941.txt	yob1955.txt  yob1969.txt  yob1983.txt  yob1997.txt  yob2011.txt
yob1886.txt	    yob1900.txt  yob1914.txt  yob1928.txt  yob1942.txt	yob1956.txt  yob1970.txt  yob1984.txt  yob1998.txt  yob2012.txt
yob1887.txt	    yob1901.txt  yob1915.txt  yob1929.txt  yob1943.txt	yob1957.txt  yob1971.txt  yob1985.txt  yob1999.txt  yob2013.txt
yob1888.txt	    yob1902.txt  yob1916.txt  yob1930.txt  yob1944.txt	yob1958.txt  yob1972.txt  yob1986.txt  yob2000.txt  yob2014.txt
yob1889.txt	    yob1903.txt  yob1917.txt  yob1931.txt  yob1945.txt	yob1959.txt  yob1973.txt  yob1987.txt  yob2001.txt  yob2015.txt
yob1890.txt	    yob1904.txt  yob1918.txt  yob1932.txt  yob1946.txt	yob1960.txt  yob1974.txt  yob1988.txt  yob2002.txt  yob2016.txt
yob1891.txt	    yob1905.txt  yob1919.txt  yob1933.txt  yob1947.txt	yob1961.txt  yob1975.txt  yob1989.txt  yob2003.txt  yob2017.txt
yob1892.txt	    yob1906.txt  yob1920.txt  yob1934.txt  yob1948.txt	yob1962.txt  yob1976.txt  yob1990.txt  yob2004.txt

# Colocar dentro da partição ( ano=2015 )
hdfs dfs -put /input/exercises-data/names/yob2015.txt  /user/aluno/marcia-castagna/data/nascimento/ano=2015


# 6.Selecionar os 10 primeiros registros da tabela nascimento no Hive

# Acessar beeline
docker exec -it hive-server bash
beeline -u jdbc:hive2://localhost:10000

select * from marcia.nascimento limit 10;

select ano from marcia.nascimento limit 10;

# ano é um atributo, só que é uma partição

+------------------+------------------+------------------------+-----------------+
| nascimento.nome  | nascimento.sexo  | nascimento.frequencia  | nascimento.ano  |
+------------------+------------------+------------------------+-----------------+
| Emma             | F                | 20435                  | 2015            |
| Olivia           | F                | 19669                  | 2015            |
| Sophia           | F                | 17402                  | 2015            |
| Ava              | F                | 16361                  | 2015            |
| Isabella         | F                | 15594                  | 2015            |
| Mia              | F                | 14892                  | 2015            |
| Abigail          | F                | 12390                  | 2015            |
| Emily            | F                | 11780                  | 2015            |
| Charlotte        | F                | 11390                  | 2015            |
| Harper           | F                | 10291                  | 2015            |
+------------------+------------------+------------------------+-----------------+


# 7.Repita o processo do 4 ao 6 para os anos de 2016 e 2017.

# Alterar tabela para criar as partições
alter table marcia.nascimento add partition(ano = 2016);
alter table marcia.nascimento add partition(ano = 2017);


# sair do beeline: control + D
# sair do hive: control + D


# acessar hdfs - namenode
docker exec -it namenode bash


# Enviar arquivo com o ano x na partição x
hdfs dfs -put /input/exercises-data/names/yob2016.txt  /user/aluno/marcia-castagna/data/nascimento/ano=2016
hdfs dfs -put /input/exercises-data/names/yob2017.txt  /user/aluno/marcia-castagna/data/nascimento/ano=2017


# Listando os arquivos do nascimento
hdfs dfs -ls /user/aluno/marcia-castagna/data/nascimento


Found 3 items
drwxr-xr-x   - root supergroup          0 2021-05-04 05:35 /user/aluno/marcia-castagna/data/nascimento/ano=2015
drwxr-xr-x   - root supergroup          0 2021-05-04 05:56 /user/aluno/marcia-castagna/data/nascimento/ano=2016
drwxr-xr-x   - root supergroup          0 2021-05-04 05:56 /user/aluno/marcia-castagna/data/nascimento/ano=2017

# sair do namenode: control + D

# acessar beeline
docker exec -it hive-server bash
beeline -u jdbc:hive2://localhost:10000
use marcia;

select * from marcia.nascimento where ano=2015 limit 10;
+------------------+------------------+------------------------+-----------------+
| nascimento.nome  | nascimento.sexo  | nascimento.frequencia  | nascimento.ano  |
+------------------+------------------+------------------------+-----------------+
| Emma             | F                | 20435                  | 2015            |
| Olivia           | F                | 19669                  | 2015            |
| Sophia           | F                | 17402                  | 2015            |
| Ava              | F                | 16361                  | 2015            |
| Isabella         | F                | 15594                  | 2015            |
| Mia              | F                | 14892                  | 2015            |
| Abigail          | F                | 12390                  | 2015            |
| Emily            | F                | 11780                  | 2015            |
| Charlotte        | F                | 11390                  | 2015            |
| Harper           | F                | 10291                  | 2015            |
+------------------+------------------+------------------------+-----------------+


select * from marcia.nascimento where ano=2016 limit 10;
+------------------+------------------+------------------------+-----------------+
| nascimento.nome  | nascimento.sexo  | nascimento.frequencia  | nascimento.ano  |
+------------------+------------------+------------------------+-----------------+
| Emma             | F                | 19471                  | 2016            |
| Olivia           | F                | 19327                  | 2016            |
| Ava              | F                | 16283                  | 2016            |
| Sophia           | F                | 16112                  | 2016            |
| Isabella         | F                | 14772                  | 2016            |
| Mia              | F                | 14415                  | 2016            |
| Charlotte        | F                | 13080                  | 2016            |
| Abigail          | F                | 11747                  | 2016            |
| Emily            | F                | 10957                  | 2016            |
| Harper           | F                | 10773                  | 2016            |
+------------------+------------------+------------------------+-----------------+


select * from marcia.nascimento where ano=2017 limit 10;
+------------------+------------------+------------------------+-----------------+
| nascimento.nome  | nascimento.sexo  | nascimento.frequencia  | nascimento.ano  |
+------------------+------------------+------------------------+-----------------+
| Emma             | F                | 19738                  | 2017            |
| Olivia           | F                | 18632                  | 2017            |
| Ava              | F                | 15902                  | 2017            |
| Isabella         | F                | 15100                  | 2017            |
| Sophia           | F                | 14831                  | 2017            |
| Mia              | F                | 13437                  | 2017            |
| Charlotte        | F                | 12893                  | 2017            |
| Amelia           | F                | 11800                  | 2017            |
| Evelyn           | F                | 10675                  | 2017            |
| Abigail          | F                | 10551                  | 2017            |
+------------------+------------------+------------------------+-----------------+


docker-compose stop

```

Dica: Particionar olhando para o WHERE (cuidando a frequência)

Pode ter mais de uma participação (ano e mês, por exemplo) - cuidar com arquivos muitos pequenos (menor de 128MB)

Muitos teras: criar outras participações ou dividir por buckets