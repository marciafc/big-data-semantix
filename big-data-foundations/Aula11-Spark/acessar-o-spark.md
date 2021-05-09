# Acessar o Spark

Adicionar o jar para salvar tabelas Hive:

	curl -O https://repo1.maven.org/maven2/com/twitter/parquet-hadoop-bundle/1.6.0/parquet-hadoop-bundle-1.6.0.jar 	  
	# Caso não tenha o curl, instalar:
		$ sudo apt update
		$ sudo apt upgrade
		$ sudo apt install curl
	
	docker cp parquet-hadoop-bundle-1.6.0.jar  spark:/opt/spark/jars

```
docker-compose up -d

docker ps

docker exec -it spark bash


root@spark:/# ls /opt/spark

	LICENSE  R	    RELEASE  conf  examples  kubernetes  logs	 sbin
	NOTICE	 README.md  bin      data  jars      licenses	 python  yarn


# Configuração do hive, dentro do Spark: hive-site.xml
root@spark:/# ls /opt/spark/conf/

	docker.properties.template  log4j.properties.template	 spark-defaults.conf.template
	fairscheduler.xml.template  metrics.properties.template  spark-env.sh.template
	hive-site.xml		    slaves.template


root@spark:/# ls /opt/spark/jars/

	RoaringBitmap-0.5.11.jar		   logging-interceptor-3.12.0.jar
	aircompressor-0.10.jar			   lz4-java-1.4.0.jar
	antlr4-runtime-4.7.jar			   machinist_2.11-0.6.1.jar
	aopalliance-repackaged-2.4.0-b34.jar	   macro-compat_2.11-1.1.1.jar
	arpack_combined_all-0.1.jar		   mesos-1.4.0-shaded-protobuf.jar
	arrow-format-0.10.0.jar			   metrics-core-3.1.5.jar
	arrow-memory-0.10.0.jar			   metrics-graphite-3.1.5.jar
	arrow-vector-0.10.0.jar			   metrics-json-3.1.5.jar
	automaton-1.11-8.jar			   metrics-jvm-3.1.5.jar
	avro-mapred-1.8.2-hadoop2.jar		   minlog-1.3.0.jar
	breeze-macros_2.11-0.13.2.jar		   mysql-connector-java-5.1.47.jar
	breeze_2.11-0.13.2.jar			   netty-3.9.9.Final.jar
	chill-java-0.9.3.jar			   netty-all-4.1.17.Final.jar
	chill_2.11-0.9.3.jar			   objenesis-2.5.1.jar
	commons-codec-1.10.jar			   okhttp-3.8.1.jar
	commons-compiler-3.0.9.jar		   okio-1.13.0.jar
	commons-crypto-1.0.0.jar		   opencsv-2.3.jar
	commons-lang-2.6.jar			   orc-core-1.5.5-nohive.jar
	commons-lang3-3.5.jar			   orc-mapreduce-1.5.5-nohive.jar
	commons-math3-3.4.1.jar			   orc-shims-1.5.5.jar
	commons-net-3.1.jar			   oro-2.0.8.jar
	compress-lzf-1.0.3.jar			   osgi-resource-locator-1.0.1.jar
	core-1.1.2.jar				   paranamer-2.8.jar
	flatbuffers-1.2.0-3f79e055.jar		   parquet-column-1.10.1.jar
	generex-1.0.1.jar			   parquet-common-1.10.1.jar
	hive-beeline-1.2.1.spark2.jar		   parquet-encoding-1.10.1.jar
	hive-cli-1.2.1.spark2.jar		   parquet-format-2.4.0.jar
	hive-exec-1.2.1.spark2.jar		   parquet-hadoop-1.10.1.jar
	hive-jdbc-1.2.1.spark2.jar		   parquet-hadoop-bundle-1.6.0.jar
	hive-metastore-1.2.1.spark2.jar		   parquet-jackson-1.10.1.jar
	hk2-api-2.4.0-b34.jar			   presto-jdbc-331.jar
	hk2-locator-2.4.0-b34.jar		   py4j-0.10.7.jar
	hk2-utils-2.4.0-b34.jar			   pyrolite-4.13.jar
	hppc-0.7.2.jar				   scala-compiler-2.11.12.jar
	ivy-2.4.0.jar				   scala-library-2.11.12.jar
	jackson-annotations-2.6.7.jar		   scala-parser-combinators_2.11-1.1.0.jar
	jackson-core-2.6.7.jar			   scala-reflect-2.11.12.jar
	jackson-databind-2.6.7.1.jar		   scala-xml_2.11-1.0.5.jar
	jackson-dataformat-yaml-2.6.7.jar	   shapeless_2.11-2.3.2.jar
	jackson-module-jaxb-annotations-2.6.7.jar  snakeyaml-1.15.jar
	jackson-module-paranamer-2.7.9.jar	   snappy-java-1.1.7.1.jar
	jackson-module-scala_2.11-2.6.7.1.jar	   spark-catalyst_2.11-2.4.1.jar
	janino-3.0.9.jar			   spark-core_2.11-2.4.1.jar
	javassist-3.18.1-GA.jar			   spark-graphx_2.11-2.4.1.jar
	javax.annotation-api-1.2.jar		   spark-hive-thriftserver_2.11-2.4.1.jar
	javax.inject-2.4.0-b34.jar		   spark-kubernetes_2.11-2.4.1.jar
	javax.servlet-api-3.1.0.jar		   spark-kvstore_2.11-2.4.1.jar
	javax.ws.rs-api-2.0.1.jar		   spark-launcher_2.11-2.4.1.jar
	jcl-over-slf4j-1.7.16.jar		   spark-mesos_2.11-2.4.1.jar
	jersey-client-2.22.2.jar		   spark-mllib-local_2.11-2.4.1.jar
	jersey-common-2.22.2.jar		   spark-mllib_2.11-2.4.1.jar
	jersey-container-servlet-2.22.2.jar	   spark-network-common_2.11-2.4.1.jar
	jersey-container-servlet-core-2.22.2.jar   spark-network-shuffle_2.11-2.4.1.jar
	jersey-guava-2.22.2.jar			   spark-repl_2.11-2.4.1.jar
	jersey-media-jaxb-2.22.2.jar		   spark-sketch_2.11-2.4.1.jar
	jersey-server-2.22.2.jar		   spark-sql-kafka-0-10_2.11-2.4.1.jar
	joda-time-2.9.3.jar			   spark-sql_2.11-2.4.1.jar
	json4s-ast_2.11-3.5.3.jar		   spark-streaming-kafka-0-8-assembly_2.11-2.3.0.jar
	json4s-core_2.11-3.5.3.jar		   spark-streaming_2.11-2.4.1.jar
	json4s-jackson_2.11-3.5.3.jar		   spark-tags_2.11-2.4.1-tests.jar
	json4s-scalap_2.11-3.5.3.jar		   spark-tags_2.11-2.4.1.jar
	jsr305-1.3.9.jar			   spark-unsafe_2.11-2.4.1.jar
	jtransforms-2.4.0.jar			   spark-warehouse
	jul-to-slf4j-1.7.16.jar			   spark-yarn_2.11-2.4.1.jar
	kafka-clients-2.3.0.jar			   spire-macros_2.11-0.13.0.jar
	kryo-shaded-4.0.2.jar			   spire_2.11-0.13.0.jar
	kubernetes-client-4.1.2.jar		   stream-2.7.0.jar
	kubernetes-model-4.1.2.jar		   univocity-parsers-2.7.3.jar
	kubernetes-model-common-4.1.2.jar	   validation-api-1.1.0.Final.jar
	leveldbjni-all-1.8.jar			   xbean-asm6-shaded-4.8.jar
	libfb303-0.9.3.jar			   zjsonpatch-0.3.0.jar
	libthrift-0.9.3.jar			   zstd-jni-1.3.2-2.jar



# Abrir o Spark
# sc: Spark Context
spark-shell

scala> sc
	res0: org.apache.spark.SparkContext = org.apache.spark.SparkContext@24facb47

# Configurações da sessão
scala> spark
	res1: org.apache.spark.sql.SparkSession = org.apache.spark.sql.SparkSession@25ee3cd6


# Sair do shell do spark
control + D

# Sair do container spark
control + D

docker-compose stop

```