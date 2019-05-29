# Elasticsearch Install

## 계정 생성

	sudo useradd elas -m -s /bin/bash
	sudo passwd elas

## sudo 권한

	sudo vi /etc/sudoers
	elas   ALL=(ALL:ALL) ALL
	
## JAVA Download/Unzip : OpenJDK8
	
	sudo apt-get install unzip
	unzip jdk-8u212-ojdkbuild-linux-x64.zip
	mv jdk-8u212-ojdkbuild-linux-x64 jdk8
	
## JAVA_HOME 설정

	vi .profile
	export JAVA_HOME=~/jdk8
	
	$JAVA_HOME/bin/java -version

## Elasticsearch Install

	wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.7.2.tar.gz
	tar -xzvf elasticsearch-6.7.2.tar.gz

## Elasticsearch start/stop

	echo 'bin/elasticsearch -d -p es.pid' > start.sh
	echo 'kill -s kill `cat es.pid`' > stop.sh
	chmod 755 start.sh stop.sh

## Elasticsearch Check

	curl http://localhost:9200
	ps -ef | grep java
	netstat -na | grep LISTEN | grep 9200
