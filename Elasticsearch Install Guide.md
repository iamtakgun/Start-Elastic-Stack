# Elasticsearch Install

## 계정 생성

	sudo useradd elas -m -s /bin/bash
	sudo passwd elas

## sudo 권한

	sudo vi /etc/sudoers
	elas   ALL=(ALL:ALL) ALL
	
## JAVA 설치 : OpenJDK11

	wget https://d3pxv6yz143wms.cloudfront.net/11.0.3.7.1/amazon-corretto-11.0.3.7.1-linux-x64.tar.gz
	tar -xzvf amazon-corretto-11.0.3.7.1-linux-x64.tar.gz
	amazon-corretto-11.0.3.7.1-linux-x64/bin/java -version
	
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
