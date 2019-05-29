## Logstash logstash-test1.conf test
	
	vi logstash-test1.conf
	input {
    	  stdin{}
	}
	output {
    	  stdout{}
	}
	
	bin/logstash -f logstash-test1.conf
	
## Logstash logstash-test2.conf

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
