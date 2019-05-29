# Cluster 요약

     GET /_cluster/health
     
     "status" : "green"
     "number_of_nodes" : 1,
     "number_of_data_nodes" : 1,
     "active_primary_shards" : 5,
     "active_shards" : 10,
     
# 설정 기준의 정보

## Cluster
     
     GET _cluster/state
     
     "version" : 347,  **Version 이 가장 낮은 Node가 Master Node 다운 시 Master로 

## Node
     
     전체 노드
     GET _nodes
     
     특정 IP 노드
     GET _nodes/1.1.1.1,2.2.2.2
   
     특정 노트 ID
     GET _nodes/fuL86F0DSyqAHDyZdQR6zQ
     
     호출받은 노드
     GET _nodes/_local
     
     "os", "process", "jvm", "thread_pool", "settings"(elasticsearch.yml 설정)

# Cat API

Console 출력 기반 API

     GET _cat
     GET _cat/master?v


# Rolling Restart

장애 아닌 상황에서 재기동 시 샤드 재분배로 인해 생기는 부하 고려

     PUT _cluster/settings
     {
          "transient" : {
               "cluster.routing.allocation.enable" : "none"
          }
     }
     
     Elasticsearch restart
     
     PUT _cluster/settings
     {
          "transient" : {
               "cluster.routing.allocation.enable" : null
          }
     }
  
 
 # Dynamic index settings
 
 number_of_replicas - 운영중에 리플리카 샤드 갯수를 변경
 ** number_of_shards는 Static 설정으로 변경 불가(Template 방식 필요)
 
     PUT twitter/_settings
     {
          "index" : {
               "number_of_replicas" : 0
          }
     }
 
# refresh_interval
 
commit point 를 생성하는 주기
 
     PUT twitter/_settings
     {
          "index" : {
               "refresh_interval" : "2s"
          }
     }
     
# Template

인덱스가 생성될 때 사용자 정의된 세팅으로 자동 매핑
     
     인덱스가 생성될 때 패턴이 매칭되는 인덱스는 해당 정의 사용
     order가 높은 번호가 낮은 번호를 override & merge

     PUT _template/mytemplate
     {
          "index_patterns": ["te*", "bar*"],
          "order" : 0,
          "settings": {
               "number_of_shards": 0
          }
     }

# Reindex

인덱스를 복사하는 API

     POST _reindex
     {
       "source" {
         "index": "twitter"
       },
       "dest": {
         "index": "twitter1"
       }
     }

# aliases

인덱스에 별칭을 부여하는 API

     POST /_aliases
     {
          "actions": [
               { "add": { "indices": ["twitter", "twitter1"], "alias":"twitters" } }
          ]
     }

# segment
segment 를 강제로 병합 API
인덱싱이 끝난 인덱스는 하나의 segment 로 merge

     POST /_forcemerge?max_num_segments=1

# Index open/close
close 인덱스는 read/write 불가
클러스터 전체 샤드에서 제외
라우팅 disabled

     POST twitter/_close
     POST twitter/_open
