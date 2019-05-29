
## Logstash Start

    bin/logstash –f ./sincedb_test.conf --config.reload.automatic
    
    * --config.reload.automatic : 재기동 없이 conf 반영 

## logstash-apache.conf input config

    vi logstash-apache.conf
    
    input {
      file {
        path => "/home/logst/logstash/apache-access.log"
        start_position => "beginning"
        sincedb_path => "/dev/null"
      }
    }
    
## logstash-apache.conf filter config

    filter {
      grok {
        match => { "message" => "%{HTTPD_COMBINEDLOG}"}
      }
  
      mutate{
        remove_field => [ "auth", "ident" ]
        convert => { "bytes" => "integer" }
        convert => { "response" => "integer" }
      }
  
      date{
        match => ["timestamp","dd/MMM/yyyy:HH:mm:SS Z"]
        target => "timestamp"
      }
  
      geoip {
        source => "clientip"
      }
    }
    
    * geoip : Geolite2 City DBMS(Maxmind Product)
    
## logstash-apache.conf output config

    output {
        elasticsearch{
            hosts => ["localhost:9200"]
            index => "apache-access-logstash-%{+yyyy.MM.dd}"
        }
        
        stdout{}
    }
