
# Metricbeat Use

## Metricbeat  Install

	wget https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.2-linux-x86_64.tar.gz
  	tar -zxvf metricbeat-6.7.2-linux-x86_64.tar.gz

## Metricbeat modules.d list Check

	  metricbeat -c ./metricbeat.yml modules list

## Metricbeat System 데이터 수집
	  
	  ./metricbeat -e -c ./metricbeat.yml

## Kibana Dashboard 생성

  	./metricbeat -c ./metricbeat.yml setup --dashboards


## Apache Install / Start
	
	sudo apt-get update
	sudo apt-get install apache2
	
	systemctl [status,stop,start] apache2
	
	ps -ef | grep apache2
	netstat -na | grep LISTEN | grep 80
	
## Metricbeat Apache 설정

	./metricbeat -c ./metricbeat.yml modules [enable,disable] apache
	./metricbeat -c ./metricbeat.yml modules list
	
## Metricbeat Apache 데이터 수집
	  
	  ./metricbeat -e -c ./metricbeat.yml
	
