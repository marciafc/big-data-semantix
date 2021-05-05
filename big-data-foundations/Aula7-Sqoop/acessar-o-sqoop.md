# Acessar o Sqoop

docker-compose up -d 

docker ps


-> sqoop está no namenode

```

docker exec -it namenode bash

# Versão
root@namenode:/# sqoop version
// ...
Sqoop 1.4.7
git commit id 2328971411f57f0cb683dfb79d19d4d19d185dd8
Compiled by maugli on Thu Dec 21 15:59:58 STD 2017


# Help
root@namenode:/# sqoop help

# Help como fazer import
sqoop help import

control + d

docker-compose stop

```