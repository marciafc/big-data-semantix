# Exercícios - Pub/Sub


```

// Abrir dois terminais, lado a lado: um será o SUBSCRIBE (assinante) e o outro o PUBLISH (envia as mensagens)

docker exec -it redis bash 
redis-cli

docker exec -it redis bash 
redis-cli


1. Criar um assinante para receber as mensagens do canal noticias:sp

// Em um dos terminais (o SUBSCRIBE)
127.0.0.1:6379> SUBSCRIBE noticias:sp 

Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "noticias:sp"
3) (integer) 1


2. Criar um editor para enviar as seguintes mensagens no canal noticias:sp

	Msg 1
	Msg 2
	Msg 3
	
// No outro terminal (o PUBLISH)
127.0.0.1:6379> PUBLISH noticias:sp 'Msg 1'
(integer) 1

127.0.0.1:6379> PUBLISH noticias:sp 'Msg 2'
(integer) 1

127.0.0.1:6379> PUBLISH noticias:sp 'Msg 3'
(integer) 1

// Como ficou o primeiro terminal (que é o SUBSCRIBE):

1) "subscribe"
2) "noticias:sp"
3) (integer) 1
1) "message"
2) "noticias:sp"
3) "Msg 1"
1) "message"
2) "noticias:sp"
3) "Msg 2"
1) "message"
2) "noticias:sp"
3) "Msg 3"

	
3. Cancelar o assinante do canal noticias:sp

// No terminal do SUBSCRIBE, o comando unsubscribe não funciona pq está só recebendo
// Cancelar com control + C
unsubscribe
^C
root@8290163b4899:/data#


4. Criar um assinante para receber as mensagens dos canais com o padrão noticias:*

// No terminal do PSUBSCRIBE (é o SUBSCRIBE, só que agora com um Pattern), acessar o redis-cli novamente
root@8290163b4899:/data# redis-cli

127.0.0.1:6379> PSUBSCRIBE noticias:*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "noticias:*"
3) (integer) 1


5. Criar um editor para enviar as seguintes mensagens no canal noticias:rj

	Msg 4
	Msg 5
	Msg 6
	
// No outro terminal (o PUBLISH) enviar as mensagens
127.0.0.1:6379> PUBLISH noticias:rj 'Msg 4'
(integer) 1
127.0.0.1:6379> PUBLISH noticias:rj 'Msg 5'
(integer) 1
127.0.0.1:6379> PUBLISH noticias:rj 'Msg 6'
(integer) 1


// Como ficou o primeiro terminal (que é o PSUBSCRIBE):

127.0.0.1:6379> PSUBSCRIBE noticias:*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "noticias:*"
3) (integer) 1
1) "pmessage"
2) "noticias:*"
3) "noticias:rj"
4) "Msg 4"
1) "pmessage"
2) "noticias:*"
3) "noticias:rj"
4) "Msg 5"
1) "pmessage"
2) "noticias:*"
3) "noticias:rj"
4) "Msg 6"


```