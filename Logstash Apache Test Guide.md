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
    
