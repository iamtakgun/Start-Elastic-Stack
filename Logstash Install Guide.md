
# Logstash Install

## 계정 생성

	sudo useradd logst -m -s /bin/bash
	sudo passwd logst

## sudo 권한

	sudo vi /etc/sudoers
	logst   ALL=(ALL:ALL) ALL
	
## JAVA Download/Unzip : Open JDK8
	
	sudo apt-get install unzip
	unzip jdk-8u212-ojdkbuild-linux-x64.zip
	mv jdk-8u212-ojdkbuild-linux-x64 jdk8
	
## JAVA_HOME 설정

	vi .profile
	export JAVA_HOME=~/jdk8
	
	$JAVA_HOME/bin/java -version

## Logstash Install

	wget https://artifacts.elastic.co/downloads/logstash/logstash-6.7.2.tar.gz
  	tar -zxvf logstash-6.7.2.tar.gz
