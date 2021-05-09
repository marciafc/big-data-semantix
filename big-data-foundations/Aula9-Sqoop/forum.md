```
Usamos o SQOOP para fazer a ingestão de dados
  - importar dados de um Banco de dados relacional para o HDFS, ou 
  - exportar dados do HDFS para um banco de dados relacional.

Com os dados no sistema de arquivos do HDFS... 
  - podemos usar o Hive para consultar e analisas esses dados, facilitando nossa escrita de jobs mapReduce usando SQL.

Eu tava me confundindo um pouco pois parecia que o Hive fazia o mesmo papel que o Mysql fazia, mas no caso do nosso estudo, 
o Mysql seria uma fonte de dados local da empresa, não otimizado. 

O SQOOP nos auxilia captar esses dados do Mysql para o HDFS, e o Hive nos auxilia a ler esses dados do HDFS.

A principal diferença entre o MySQL e o Hive, é a escala. Tente pesquisar em terabytes de dados em um banco de dados relacional, 
o custo é superior e o desempenho é inferior do que se trabalhar com o Hive.

```