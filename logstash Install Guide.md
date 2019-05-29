
# Logstash Install

## 계정 생성

	sudo useradd logst -m -s /bin/bash
	sudo passwd logst

## sudo 권한

	sudo vi /etc/sudoers
	logst   ALL=(ALL:ALL) ALL
	
## JAVA Download/Unzip : OpenJDK11

	wget https://d3pxv6yz143wms.cloudfront.net/11.0.3.7.1/amazon-corretto-11.0.3.7.1-linux-x64.tar.gz
	tar -xzvf amazon-corretto-11.0.3.7.1-linux-x64.tar.gz
	amazon-corretto-11.0.3.7.1-linux-x64/bin/java -version

## JAVA_HOME 설정

	vi .profile
	export JAVA_HOME=~/amazon-corretto-11.0.3.7.1-linux-x64
	
	$JAVA_HOME/bin/java -version

## Logstash Install

	wget https://artifacts.elastic.co/downloads/logstash/logstash-6.7.2.tar.gz
  	tar -zxvf logstash-6.7.2.tar.gz

## Logstash start/stop

	echo 'bin/elasticsearch -d -p es.pid' > start.sh
	echo 'kill -s kill `cat es.pid`' > stop.sh
	chmod 755 start.sh stop.sh

## Logstash Check

	curl http://localhost:9200
	ps -ef | grep java
	netstat -na | grep LISTEN | grep 9200
