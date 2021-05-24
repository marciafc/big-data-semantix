# Exercícios - Configuração

```

//Acessar redis-cli
root@8290163b4899:/data# redis-cli
127.0.0.1:6379>


1. Visualizar todos os parâmetros de configuração

CONFIG GET *

// ...
331) "notify-keyspace-events"
332) ""
333) "bind"
334) ""
335) "oom-score-adj-values"
336) "0 200 800"


2. Verificar o parâmetro "appendonly"

// appendonly yes -> persistência nos dados
CONFIG GET appendonly
1) "appendonly"
2) "yes"


3. Remover a persistência dos dados, alterando o parâmetro "appendonly" para "no"

CONFIG set appendonly no
OK

CONFIG GET appendonly
1) "appendonly"
2) "no"


4. Verificar o parâmetro "save"

CONFIG GET save
1) "save"
2) "3600 1 300 100 60 10000"


5. Adicionar a persistência dos dados, para a cada 2 minutos (120 segundos) se pelo menos 500 chaves forem alteradas, adicionando o parâmetro "save" para "120 500"

// Para mais de um argumento, informar com aspas
CONFIG set save '120 500'
OK

CONFIG GET save
1) "save"
2) "120 500"


6. Verificar os parâmetros "maxmemory*"

config get maxmemory*

1) "maxmemory-policy"
2) "noeviction"
3) "maxmemory-samples"
4) "5"
5) "maxmemory-eviction-tenacity"
6) "10"
7) "maxmemory"
8) "0"


7. Permitir que o Redis remova automaticamente os dados antigos à medida
que você adiciona novos dados, com uso da politica "allkeys-lru", quando chegar a 1mb de memória.

CONFIG set maxmemory-policy allkeys-lru
OK

CONFIG set maxmemory 1mb
OK

config get maxmemory*

1) "maxmemory-policy"
2) "allkeys-lru"
3) "maxmemory-samples"
4) "5"
5) "maxmemory-eviction-tenacity"
6) "10"
7) "maxmemory"
8) "1048576"

// Sair do redis-cli
control + C

// Sair do container do redis
control + D


// Encerrar
docker-compose stop 

// Quando retornar, volta as configurações iniciais
docker-compose start
docker exec -it redis bash
redis-cli

config get maxmemory*

1) "maxmemory-policy"
2) "noeviction"
3) "maxmemory-samples"
4) "5"
5) "maxmemory-eviction-tenacity"
6) "10"
7) "maxmemory"
8) "0"


```