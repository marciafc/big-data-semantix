# Instalação Hadoop

```
# 1. Instalação do docker e docker-compose

	Docker: https://docs.docker.com/get-docker/ 
	
	Docker-compose: https://docs.docker.com/compose/install/
	
	
	docker --version
	Docker version 20.10.6, build 370c289
	
	docker-compose --version
	docker-compose version 1.25.0, build unknown


# 2. Executar os seguintes comandos, para baixar as imagens do Cluster de Big Data:

	git clone https://github.com/rodrigo-reboucas/docker-bigdata.git
	
	cd docker-bigdata
	
	docker-compose pull
	

# 3. Iniciar o cluster Hadoop através do docker-compose

	docker-compose up -d


# 4. Listas os containers em execução

	docker ps

	CONTAINER ID   IMAGE                    COMMAND                  CREATED      STATUS                            PORTS                                                                                                                NAMES
	cab88feee5dd   fjardim/hive             "entrypoint.sh /bin/…"   8 days ago   Up 2 seconds                      0.0.0.0:10000->10000/tcp, :::10000->10000/tcp, 10002/tcp                                                             hive-server
	441953c8ff94   fjardim/hive             "entrypoint.sh /opt/…"   8 days ago   Up 3 seconds                      10000/tcp, 0.0.0.0:9083->9083/tcp, :::9083->9083/tcp, 10002/tcp                                                      hive_metastore
	d1e73f9718e8   fjardim/hive-metastore   "/docker-entrypoint.…"   8 days ago   Up 4 seconds                      5432/tcp                                                                                                             hive-metastore-postgresql
	36f36f2ac756   fjardim/datanode         "/entrypoint.sh /run…"   8 days ago   Up 4 seconds (health: starting)   0.0.0.0:50075->50075/tcp, :::50075->50075/tcp                                                                        datanode
	a61ffdac4ad9   fjardim/hbase-master     "/entrypoint.sh /run…"   8 days ago   Up 4 seconds                      16000/tcp, 0.0.0.0:16010->16010/tcp, :::16010->16010/tcp                                                             hbase-master
	126728dd97c8   fjardim/jupyter-spark    "/opt/docker/bin/ent…"   8 days ago   Up 4 seconds                      0.0.0.0:4040-4043->4040-4043/tcp, :::4040-4043->4040-4043/tcp, 0.0.0.0:8889->8889/tcp, :::8889->8889/tcp, 8899/tcp   spark
	32ebcc6f3d9c   fjardim/namenode_sqoop   "/entrypoint.sh /run…"   8 days ago   Up 5 seconds (health: starting)   0.0.0.0:50070->50070/tcp, :::50070->50070/tcp                                                                        namenode
	f2ff3b5e5924   fjardim/mysql            "docker-entrypoint.s…"   8 days ago   Up 6 seconds                      33060/tcp, 0.0.0.0:33061->3306/tcp, :::33061->3306/tcp                                                               database
	c0dc225b9581   fjardim/zookeeper        "/bin/sh -c '/usr/sb…"   8 days ago   Up 5 seconds                      22/tcp, 2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, :::2181->2181/tcp                                                zookeeper


# 5. Verificar os logs dos containers do docker-compose em execução

	docker-compose logs
	
	docker-compose logs -f


# 6. Verificar os logs do container namenode

	docker logs namenode
	
	docker logs namenode -f


# 7.  Acessar o container namenode

	docker exec -it namenode bash


# 8. Listar  os diretórios do container namenode

	docker exec -it namenode ls /

	bin		       derby.log       home	     opt     sys
	boot		       dev	       input	     proc    titles.java
	codegen_titles.java    employees.java  lib	     root    tmp
	cp_rental_append.java  entrypoint.sh   lib64	     run     usr
	cp_rental_date.java    etc	       media	     run.sh  var
	cp_rental_id.java      hadoop	       metastore_db  sbin
	cp_titles_date.java    hadoop-data     mnt	     srv


	docker exec -i namenode ls /
	
	bin
	boot
	codegen_titles.java
	cp_rental_append.java
	cp_rental_date.java
	cp_rental_id.java
	cp_titles_date.java
	derby.log
	dev
	employees.java
	entrypoint.sh
	etc
	hadoop
	hadoop-data
	home
	input
	lib
	lib64
	media
	metastore_db
	mnt
	opt
	proc
	root
	run
	run.sh
	sbin
	srv
	sys
	titles.java
	tmp
	usr
	var

# 9. Parar os containers do Cluster de Big Data
	
	docker-compose stop

	docker ps
	
	docker ps -a
	
```


## Links

	- [Docker Big Data - fork - exercício 2](https://github.com/marciafc/docker-bigdata)
	
	- [Dados dos exercícios- fork](https://github.com/marciafc/exercises-data)
	
	- [Link para instalação do Docker Desktop no Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)