
## Apache Access Log Download

    cd /home/elas/logstash6
    
    wget https://github.com/iamtakgun/Start-Elastic-Stack/raw/master/sample/logs.gz
    wget https://github.com/iamtakgun/Start-Elastic-Stack/raw/master/sample/logs2.gz
    
    gzip -d logs.gz
    gzip -d logs2.gz
    
    mv logs apache-access.log
    mv logs apache-access2.log
    
## logstash-apache.conf input config
    
    cd /home/elas/logstash6
    
    vi logstash-apache.conf
    
    input {
      file {
        path => "/home/elas/log_data/apache-access.log"
        start_position => "beginning"
        sincedb_path => "/dev/null"
      }
    }
    
    output {
        stdout{}
    }

## Logstash Start

    bin/logstash -f ./logstash-apache.conf --config.reload.automatic
    
    * --config.reload.automatic : 재기동 없이 conf 반영 
    
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
            index => "apache-access-logstash-%{+yyyy-mm-dd.hhmmss}"
        }
        
        stdout{}
    }
