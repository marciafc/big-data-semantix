# Exercícios - Instalação

```
1. Instalação do docker e docker-compose

Ver PassosInstalacaoDockerLinux.txt


2. Baixar a imagem do redis

Criar pasta 'redis'

Copiar arquivo 'docker-compose.yml' para esta pasta

cd redis

# Visualizar conteúdo do arquivo
cat docker-compose.yml 

version: '3.1'

services:

  redis:
    container_name: redis
    image: redis
    ports:
      - 6379:6379
    volumes:
      - data:/data
    entrypoint: redis-server --appendonly yes
    restart: always      

volumes:
  data:

# Baixar imagem do Redis
docker pull redis



3. Iniciar o Redis através do docker-compose

# Sube exibindo logs
docker-compose up

# Sube rodando em backgroud 
docker-compose up -d

# Visualizar logs do container redis
docker logs redis


4. Listas as imagens em execução

docker ps

# Todos
docker ps -a


5. Verificar a versão do Redis

# Ajuda
docker exec --help

docker exec -i redis ls /
	bin
	boot
	data
	dev
	etc
	home
	lib
	lib64
	media
	mnt
	opt
	proc
	root
	run
	sbin
	srv
	sys
	tmp
	usr
	var


# O "t" é "Allocate a pseudo-TTY"
docker exec -it redis ls /
	bin   data  etc   lib	 media	opt   root  sbin  sys  usr
	boot  dev   home  lib64  mnt	proc  run   srv   tmp  var

# Sem o "i" (interactive)
docker exec -t redis ls /
	bin   data  etc   lib	 media	opt   root  sbin  sys  usr
	boot  dev   home  lib64  mnt	proc  run   srv   tmp  var


# Versão
docker exec -it redis redis-server --version
	Redis server v=6.2.3 sha=00000000:0 malloc=jemalloc-5.1.0 bits=64 build=dc20d908b7b619b4

docker exec -it redis redis-cli --version
	redis-cli 6.2.3


6. Acessar o Redis CLI

// bash == /bin/bash
docker exec -it redis bash 
redis-cli
127.0.0.1:6379>


```