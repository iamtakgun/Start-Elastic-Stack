
## Apache Access Log Download
    
    cd
    mkdir log_data
    cd log_data
    
    vi apache-access.log
    83.149.9.216 - - [04/Jan/2015:05:13:42 +0000] "GET /presentations/logstash-monitorama-2013/images/kibana-search.png HTTP/1.1" 200   203023 "http://semicomplete.com/presentations/logstash-monitorama-2013/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1700.77 Safari/537.36"

    wget https://github.com/iamtakgun/Start-Elastic-Stack/raw/master/sample/apache-access2.log
    
    
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
    
## logstash-apache.conf filter +grok config

    filter {
      grok {
        match => { "message" => "%{HTTPD_COMBINEDLOG}"}
      }
    }

## logstash-apache.conf filter +mutate remove_field config

    filter {
      grok {
        match => { "message" => "%{HTTPD_COMBINEDLOG}"}
      }
  
      mutate{
        remove_field => [ "auth", "ident" ]
      }
    }
    
## logstash-apache.conf filter +mutate convert config

     filter {
      grok {
        match => { "message" => "%{HTTPD_COMBINEDLOG}"}
      }
  
      mutate{
        remove_field => [ "auth", "ident" ]
        convert => { "bytes" => "integer" }
        convert => { "response" => "integer" }
      }
     }
    
## logstash-apache.conf filter +date config  
  
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
    }

## logstash-apache.conf filter +geoip config 

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
            index => "apache-access-logstash-%{+yyyy-mm-dd}"
        }
        
        stdout{}
    }
    
## kibana에서 데이터 확인

    GET apache-*/_count
    
