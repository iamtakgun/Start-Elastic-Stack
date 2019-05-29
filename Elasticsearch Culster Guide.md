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
     
     "version" : 347,  : Version 이 가장 낮은 Node가 Master Node 다운 시 다른 Master 선정할 때

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
