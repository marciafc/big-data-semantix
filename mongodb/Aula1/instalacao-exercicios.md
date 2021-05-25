# Exercícios - Instalação

```


1. Instalação do docker e docker-compose

	Acesso: https://docs.docker.com/get-docker/ 
	
	Ver arquivo PassosInstalacaoDockerLinux.txt
	
	
2. Baixar as imagens do mongo e mongo-express

	docker pull mongo

	docker pull mongo-express

	// Lista todas as imagens baixadas
	docker images 
	
	REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
	mongo                          latest              07630e791de3        13 days ago         449MB
	mongo-express                  latest              51fc3f2af7a1        5 weeks ago         128MB
	// ...
	
	
3. Iniciar o MongoDB através do docker-compose

	Criar pasta 'mongodb'
	
	Copiar arquivo 'docker-compose.yml' para pasta 'mongodb'
	
	cd mongodb
	
	// Visualizando conteúdo docker-compose.yml 
	cat docker-compose.yml 
	
	version: '3.1'

	services:

	  mongo:
		image: mongo
		container_name: mongo
		restart: always
		ports:
		  - 27017:27017
		volumes:
		  - db:/data/db

	  mongo-express:
		image: mongo-express
		container_name: mongo-express
		restart: always
		ports:
		  - 8081:8081

	volumes:
	  db: 	
	
	// Iniciar mongo
	docker-compose up -d
	
	Creating network "mongodb_default" with the default driver
	Creating volume "mongodb_db" with default driver
	Creating mongo         ... done
	Creating mongo-express ... done


4. Listas as imagens em execução

	docker ps
	
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
	9c96eeff355b        mongo-express       "tini -- /docker-ent…"   36 seconds ago      Up 35 seconds       0.0.0.0:8081->8081/tcp     mongo-express
	27c5bda401de        mongo               "docker-entrypoint.s…"   36 seconds ago      Up 35 seconds       0.0.0.0:27017->27017/tcp   mongo


5. Listar os bancos de dados do MongoDB

	$ docker exec -it mongo bash
	
	root@27c5bda401de:/# mongo
	
	> show databases
	admin   0.000GB
	config  0.000GB
	local   0.000GB
	
	> show dbs
	admin   0.000GB
	config  0.000GB
	local   0.000GB
	
// Sair do terminal do mongo	
Control + D

// Sair do container do mongo	
Control + D

// Para os container, remove-os e apaga a network (não apaga os volumes)
docker-compose down

	Stopping mongo-express ... done
	Stopping mongo         ... done
	Removing mongo-express ... done
	Removing mongo         ... done
	Removing network mongodb_default


docker ps -a 


// Ajuda sobre volume
docker volume --help

	Usage:	docker volume COMMAND

	Manage volumes

	Commands:
	  create      Create a volume
	  inspect     Display detailed information on one or more volumes
	  ls          List volumes
	  prune       Remove all unused local volumes
	  rm          Remove one or more volumes

	Run 'docker volume COMMAND --help' for more information on a command.


// Listar volumes
docker volume ls
	DRIVER              VOLUME NAME
	local               mongodb_db
	// ...


// Remove all unused local volumes
docker volume prune


6. Visualizar através do Mongo Express os bancos de dados

	Acesso: http://localhost:8081/



```