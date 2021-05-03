# Fórum

```
meu container hive-server estava caindo toda vez que executava o comando "show databases;".

Olhando o docker stats, percebi que os containers estão com limit de 500mb. O hive-server em stand-by estava utilizando em média 74% de toda a memória disponível, quando executava o comando do beeline atingia 100% e o container baixava.

Caso aconteça com alguém, apenas  aumentei o limit dentro do docker-compose.yml e deu certo. (Welles Aquino)

```

Diferença entre tabela estática ou dinâmica, em quais contextos elas são aplicadas e porque. 

```
Na verdade não é tabela estática ou dinânica e sim partição estática ou dinâmica. 

A diferença é que na estática, toda vez que você for carregar ou inserir um dado na tabela, você precisa especificar o valor na partição, ou seja, se você tem uma partição país, 
você precisa especificar no comando do input ou insert o pais do determinado dado. 

No caso do dinâmico, ele já insere os dados na partição de acordo com o valor que vem no arquivo ou nos dados da tabela source você não precisa escolher o valor.

Quando usar uma ou outra:

- O estático quando você já sabe o valor e já sabe o destino facilita, por que na hora da leitura do arquivo você não precisa ler ele todo, 
você seleciona por exemplo o país que você quer copiar, e ele vai selecionar somente o nome do país. 

- O dinâmico o próprio nome diz, quando você está carregando diversas informações e não dá ra você ficar pegando valor a valor e ficar inserindo, 
em fluxo de ETL por exemplo, dessa forma, ele lê toda informação e vai jogando cada bloco de dados em suas respectivas partições.

```

Compilado de comandos para hdfs

```

Comandos HDFS do Hadoop

hadoop fs –help  mostra todos os comando do hadoop com uma pequena
explicação.

hadoop fs –ls  lista o conteúdo de uma diretório dentro do HDFS

hadoop fs –ls –R  lista o conteúdo do hadoop recursivamentehadoop fs –du  mostra uso de disco, em bytes, para todos os arquivos que
correspondem ao caminho; os nomes de arquivos são relatadas com o protocolo
completo HDFS prefixo.

hadoop fs –du –s  como -du, mas imprime um resumo da utilização do disco de
todos os arquivos/diretórios no path.

hadoop mv <src> <dest>  move o arquivo ou diretório indicado pelo src para dest,
dentro HDFS.

hadoop fs -cp <src> <dest>  copia o arquivo ou diretório identificado pelo src para
dest, dentro HDFS.

hadoop fs -rm <path>  remove o arquivo ou diretório vazio identificados pelo
caminho.

hadoop fs -rmr <path>  remove o arquivo ou diretório identificado pelo caminho.
Recursivamente exclui qualquer criança as entradas (ou seja, arquivos ou
subdiretórios do caminho).

hadoop fs -put <localSrc> <dest>  copia o arquivo ou diretório no sistema de
arquivos local identificado por localSrc ao dest dentro do DFS.

hadoop fs -copyFromLocal <localSrc> <dest>  idêntico ao anterior

moveFromLocal <localSrc> <dest>  copia o arquivo ou diretório no sistema de
arquivos local identificado por localSrc ao dest a HDFS, e, em seguida, o exclui a
cópia local de sucesso.

hadoop fs -get [-crc] <src> <localDest>  copia o arquivo ou diretório em HDFS
identificados pelo src para o caminho do sistema de arquivos local identificado pelo
localDest.

hadoop fs -getmerge <src> <localDest> recupera todos os arquivos que
correspondem ao caminho src, HDFS, e copia-os para um único arquivo mesclado no
sistema de arquivos local identificado por localDest.

cat <filen-ame>  exibe o conteúdo do arquivo no stdout.

hadoop –fs copyToLocal <src> <localDest>  idêntico ao -get

hadoop fs -moveToLocal <src> <localDest>  funciona como a obter, mas exclui o
HDFS cópia de sucesso.

mkdir <path>  cria um diretório chamado caminho HDFS. Cria os diretórios pais no
caminho que estão faltando (por exemplo, mkdir -p em Linux).

haddop fs -touchz <path>  cria um arquivo no caminho que contém o tempo atual
como um carimbo. Não se um arquivo já existente no caminho, a menos que o arquivo
já está tamanho 0.

haddop fs -test -[ezd] <path>  retorna 1 se existe caminho; possui comprimento
zero; ou é um diretório ou 0 caso contrário.haddop fs -stat [format] <path>  imprime as informações sobre caminho. Formato
é uma string que aceita tamanho do arquivo em blocos ( %b), ficheiro (%n), tamanho
de bloco ( %s), a replicação ( %r), e data de modificação ( %y, %Y).

hadoop fs -tail [-f] <file2name>  mostra os últimos 1KB de arquivo no stdout.

hadoop fs -chmod [-R] mode,mode,... <path>...  muda as permissões de arquivo
associado a um ou mais objetos identificados pelo caminho .... Executa as alterações
recursivamente com R. mode é um 3-dígito octal mode, ou {augo}+/-{rwxX}. Não
assume se não é especificado e não aplique uma umask selecionados nas.

hadoop chown [-R] [owner][:[group]] <path>...  define a propriedade usuário e/ou
grupo de arquivos ou diretórios identificados pelo caminho .... Define proprietário
recursivamente se -R é especificado.

hadoop chgrp [-R] group <path>...  define o grupo proprietário de arquivos ou
diretórios identificados pelo caminho .... Grupo conjuntos recursivamente se -R é
especificado.

Comandos de Cluster hadoop

hadoop version  verifica a versão instalada e rodando do hadoop

hadoop envvars  mostras as variáveis de ambiente do hadoop

hadoop classpath  mostra os caminhos(paths) de instalação do hadoop

hadoop fsck path  executa a ferramenta de utilidade de verificação do cluster.

hadoop dfsadmin –report  executa o hdfs dfsadmin client

```

