## Logstash stdin input
	
	vi logstash-test1.conf
	input {
	  stdin{}
	}
	output {
	  stdout{}
	}
	
	bin/logstash -f logstash-test1.conf
	
## Logstash FILE input

	vi logstash-test2.conf
	input {
 	  file {
            path => "/home/logst/logstash/echo.txt"
            start_position => "beginning"
    	  }
	}
	
	filter {
	}

	output {
    	  stdout{}
	}
	
	bin/logstash -f logstash-test2.conf
	
	echo 'Hello Elasticsearch' > echo.txt
	echo 'Hello Logstash' > echo.txt
	echo 'Hello Kibana' > echo.txt
	echo 'Hello Beats' > echo.txt

## Logstash .sincedb 확인

	cd /home/logst/logstash/data/plugins/inputs/file
	
	cat .sincedb_1a0f5de8e51cc489d0251b9de5126892
	256070 0 2049 12 1559137614.09582 /home/logst/logstash/echo.txt
