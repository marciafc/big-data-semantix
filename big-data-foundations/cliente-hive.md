# Cliente Hive 

```
docker-compose up -d

docker ps 

# Container do Hive
docker exec -it hive-server bash

beeline --help


#1. Connect using simple authentication to HiveServer2 on localhost:10000
#    $ beeline -u jdbc:hive2://localhost:10000 username password	

# Kerberos: segurança, para dar permissão

# Conectando com beeline
beeline -u jdbc:hive2://localhost:10000

# Exibe databases
0: jdbc:hive2://localhost:10000> show databases;

+----------------+
| database_name  |
+----------------+
| default        |
+----------------+
1 row selected (0.924 seconds)


# Fechar conexão (sai do beeline)
control + D

# Sair do Hive
control + D

docker-compose stop

```