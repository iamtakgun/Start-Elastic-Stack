
## Apache Access Log Download
    
    cd
    mkdir log_data
    cd log_data
    
    vi apache-sample.log
    50.16.19.13 - - [17/May/2015:10:05:10 +0000] "GET /blog/tags/puppet?flav=rss20 HTTP/1.1" 200 14872 "http://www.semicomplete.com/blog/tags/puppet?flav=rss20" "Tiny Tiny RSS/1.11 (http://tt-rss.org/)"
    
    wget https://github.com/iamtakgun/Start-Elastic-Stack/raw/master/sample/apache-access.log
    
    
## logstash-apache.conf input config
    
    cd /home/elas/logstash6
    
    vi logstash-apache.conf
    
    input {
      file {
        path => "/home/elas/log_data/apache-sample.log"
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
    
## logstash-apache.conf access log 
    
    #path => "/home/elas/log_data/apache-sample.log"
    path => "/home/elas/log_data/apache-access.log"
    
## kibana에서 데이터 확인

    GET apache-*/_count
    
