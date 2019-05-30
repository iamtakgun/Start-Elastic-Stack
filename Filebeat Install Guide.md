
# Filebeat Install

## Filebeat Install

	wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.7.2-linux-x86_64.tar.gz
  tar -zxvf filebeat-6.7.2-linux-x86_64.tar.gz

## Filebeat modules.d use

  cd /home/elas/filebeat6
  
  ./filebeat -c ./filebeat.yml modules enable apache2
  
  ./filebeat -c ./filebeat.yml modules list
  
  vi modules.d/apache2.yml
  
  - module: apache2
  # Access logs
  access:
    enabled: true
    # Set custom paths for the log files. If left empty,
    # Filebeat will choose the paths depending on your OS.
    var.paths: ["/home/elas/log_data/apache-access.log"]
  # Error logs
  error:
    enabled: false
    # Set custom paths for the log files. If left empty,
    # Filebeat will choose the paths depending on your OS.
    #var.paths:
  
  cd ..
  
  vi filebeat.yml
  # Kibana
  host: "localhost:5601"
  
  # kibana, Elasticsearch Setting
  ./filebeat -e -c ./filebeat.yml setup --modules apache2
  
  ./filebeat -e -c ./filebeat.yml
  
  
  
