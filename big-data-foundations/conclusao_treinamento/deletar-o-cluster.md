# Deletar o Cluster


```

# Parar o container
docker-compose stop

docker ps -a


# Parar e deletar o container
# Qlq alteração no container será perdida
# Não apaga os volumes nem network
docker-compose down


# Listar volumes
docker volume ls

# Listar network
docker network ls


Para apagar tudo, rodar scripts:

- remove_containers.sh: remove containers, volumes(dados) e network (conexão entre os dados)

- remove_images: remove todas as imagens

- kill_all_process.sh: tudo que o remove_containers.sh faz em uma única linha

- clean_all.sh: limpa tudo
	- all stopped containers
	- all networks not used by at least one container
	- all images without at least one container associated to them
	- all build cache


Exemplo para executar os scripts acima:

	cd big-data-semantix/big-data-foundations/Aula2-Hadoop/docker-scripts
	./remove_containers.sh



```