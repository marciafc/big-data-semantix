# Exercícios Comandos HDFS

```
# 1) Iniciar o cluster de Big Data
git clone https://github.com/rodrigo-reboucas/docker-bigdata.git
cd docker-bigdata
docker ps 
docker ps -a
docker-compose up -d

# 2) Baixar os dados dos exercícios do treinamento
cd input
sudo git clone https://github.com/rodrigo-reboucas/exercises-data.git

docker ps 
ls 
ls exercises-data/

# 3) Acessar o container do namenode
docker exec -it namenode bash

# Todas as pastas do hdfs
hdfs dfs -ls -R /

# Pasta user
hdfs dfs -ls -R /user

# 4) Criar a estrutura de pastas apresentada ao lado pelo comando: $ hdfs dfs -ls -R /

user/aluno/
user/aluno/<nome>
user/aluno/<nome>/data
user/aluno/<nome>/recover
user/aluno/<nome>/delete


# -p para criar a estrutura
hdfs dfs -mkdir -p /user/aluno/marcia-castagna/data

hdfs dfs -ls -R /user/aluno

# Não precisa -p pois já tem a estrutura
hdfs dfs -mkdir /user/aluno/marcia-castagna/recover
hdfs dfs -mkdir /user/aluno/marcia-castagna/delete

ls

ls /input/

# 5) Enviar a pasta "/input/exercises-data/escola" e o arquivo "/input/exercises-data/entrada1.txt" para data
hdfs dfs -put /input/exercises-data/escola/   /user/aluno/marcia-castagna/data
hdfs dfs -ls -R /user/aluno/marcia-castagna/data

hdfs dfs -put /input/exercises-data/entrada1.txt   /user/aluno/marcia-castagna/data
hdfs dfs -ls -R /user/aluno/marcia-castagna/data

# Só a pasta e o arquivo
hdfs dfs -ls  /user/aluno/marcia-castagna/data

# 6) Mover o arquivo "entrada1.txt" para recover
hdfs dfs -mv /user/aluno/marcia-castagna/data/entrada1.txt  /user/aluno/marcia-castagna/recover
hdfs dfs -ls /user/aluno/marcia-castagna/recover

# Contém o arquivo "entrada1.txt"
hdfs dfs -ls /user/aluno/marcia-castagna/recover

# Não tem mais o arquivo movido
hdfs dfs -ls /user/aluno/marcia-castagna/data

# 7) Baixar o arquivo do hdfs “escola/alunos.json” para o sistema local /


# 8) Deletar a pasta recover
# Se for pasta, usar -R (sem o -R, vai dar erro "Is a directory")
hdfs dfs -rm -R /user/aluno/marcia-castagna/recover

# 9) Deletar permanentemente o delete
hdfs dfs -rm  -skipTrash -R /user/aluno/marcia-castagna/delete

# 10) Procurar o arquivo "alunos.csv" dentro do /user
# Cluster em produção é lento
hdfs dfs -find /user -name alunos.csv

#  OU -iname (para não considerar sensitive o nome do arquivo: alunos.csv ou Alunos.csv)
hdfs dfs -find /user -iname Alunos.csv

# 11) Mostrar o último 1KB do arquivo "alunos.csv"
hdfs dfs -tail /user/aluno/marcia-castagna/data/escola/alunos.csv

# 12) Mostrar as 2 primeiras linhas do arquivo "alunos.csv"
hdfs dfs -cat /user/aluno/marcia-castagna/data/escola/alunos.csv | head -n 2

# 13) Verificação de soma das informações do arquivo "alunos.csv"
hdfs dfs -checksum /user/aluno/marcia-castagna/data/escola/alunos.csv

# 14) Criar um arquivo em branco com o nome de "test" no data
# Data e hora que a ação aconteceu
hdfs dfs -touchz /user/aluno/marcia-castagna/data/test
hdfs dfs -ls  /user/aluno/marcia-castagna/data/

# 15) Alterar o fator de replicação do arquivo “test” para 2
hdfs dfs -setrep 2 /user/aluno/marcia-castagna/data/test 

# Agora o "test" tem 2 réplicas
hdfs dfs -ls /user/aluno/marcia-castagna/data
# -rw-r--r--   2 root supergroup          0 2021-05-02 19:01 /user/aluno/marcia-castagna/data/test

hdfs dfs -help stat

# 16) Ver as informações do arquivo “alunos.csv”
hdfs dfs -stat %r /user/aluno/marcia-castagna/data/escola/alunos.csv
# 3

# Tamanho do bloco
hdfs dfs -stat %o /user/aluno/marcia-castagna/data/escola/alunos.csv

# 134217728 (divide por 1024)
# padrao é sempre 128 megas

# 17) Exibir o espaço livre (df) do data e o uso do disco (du)

hdfs dfs -df /user/aluno/marcia-castagna/data/

Filesystem                    Size      Used     Available  Use%
hdfs://namenode:8020  250438021120  31096832  105200214016    0%

# Para "humanos entenderem" ( -h ) -> de todo cluster
hdfs dfs -df -h  /user/aluno/marcia-castagna/data/

Filesystem               Size    Used  Available  Use%
hdfs://namenode:8020  233.2 G  29.7 M     98.0 G    0%

# Uso do disco
hdfs dfs -du -h  /user/aluno/marcia-castagna/data/

29.2 M  /user/aluno/marcia-castagna/data/escola
0       /user/aluno/marcia-castagna/data/test

# Uso do disco de todo cluster 
hdfs dfs -du -h  /
0       /tmp
29.2 M  /user


docker-compose stop

```

## Comandos HDFS

[HDFSCommands](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html)



