# Exercícios - Instalação MongoDB Compass

CRUD através do MongoDB Compass

1) Subir container do mongo via docker

	$ docker-compose up -d

2) Baixar e instalar o 'MongoDB Compass' https://www.mongodb.com/try/download/compass

3) No 'MongoDB Compass', clicar em "Fill in connection fields individually"

4) Conectar ao servidor MongoDB

<img src="mongodb-compass.png">

<img src="mongodb-compass2.png">


Todas as questão devem ser realizadas através do MongoDB Compass

```

1. Criar a collection cliente no banco de dados seu nome


2. Inserir os seguintes documentos:


	nome: Rodrigo, cidade: São José dos Campos, data_cadastro: 10/08/2020
	nome: João, cidade: São Paulo, data_cadastro: 05/08/2020
	
	
3. Renomear a collection para clientes


4. Buscar os documentos da cidade de São Paulo


5. Buscar os documentos da cidade de São Paulo e apresentar apenas o nome e a cidade


6. Atualizar o documento com nome João para cidade: Rio de Janeiro


7. Criar um index para o campo cidade em ordem alfabética


8. Deletar o documento com o nome João


9. Deletar a collection clientes


```