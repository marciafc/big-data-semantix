# Acessar o HBase

```
docker-compose up -d

docker ps


Container 'hbase-master' é onde acessa o HBase

docker exec -it hbase-master bash


root@hbase-master:/# hbase version
HBase 1.2.6

hbase shell
hbase(main):001:0> status
1 active master, 0 backup masters, 1 servers, 1 dead, 2.0000 average load

hbase(main):002:0> version
1.2.6, rUnknown, Mon May 29 02:25:32 CDT 2017


# Comando de ajuda
table_help
Help for table-reference commands.


# Informação sobre usuário
hbase(main):004:0> whoami
root (auth:SIMPLE)
    groups: root


hbase(main):005:0> help
HBase Shell, version 1.2.6, rUnknown, Mon May 29 02:25:32 CDT 2017
Type 'help "COMMAND"', (e.g. 'help "get"' -- the quotes are necessary) for help on a specific command.

# Listar tabelas
hbase(main):006:0> help "list"
List all tables in hbase. Optional regular expression parameter could
be used to filter the output. Examples:

  hbase> list
  hbase> list 'abc.*'
  hbase> list 'ns:abc.*'
  hbase> list 'ns:.*'
  

# Ajuda sobre outros comandos
hbase(main):006:0> help "<nome do comando>"

control + D

docker-compose stop


```