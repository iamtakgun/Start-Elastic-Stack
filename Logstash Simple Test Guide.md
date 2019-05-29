## Logstash logstash-test1.conf test
	
	vi logstash-test1.conf
	input {
    	  stdin{}
	}
	output {
    	  stdout{}
	}
	
	bin/logstast -f logstash-test1.conf
	
## Logstash Check

	curl http://localhost:9200
	ps -ef | grep java
	netstat -na | grep LISTEN | grep 9200
