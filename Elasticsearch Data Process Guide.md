# indices 처리

## index 생성

     PUT /twitter

## index 확인

     GET /twitter

## document 생성

     PUT /twitter/_doc/1
     {
         "user" : "kimchy",
         "post_date" : "2019-05-26T19:39:00",
         "message" : "trying out Elasticsearch"
     }

## op_type으로 같은 ID가 있으면 강제 ERROR

     PUT /twitter/_doc/1?op_type=create
     {
         "user" : "kimchy",
         "post_date" : "2019-05-27T14:00:00",
         "message" : "trying out Elasticsearch"
     }

## document 조회

     GET /twitter/_doc/1


## ID 지정 없이 document 생성

     PUT /twitter/_doc
     {
         "user" : "kimchy",
         "post_date" : "2019-05-26T19:39:00",
         "message" : "trying out Elasticsearch"
     }
     
     "error": "Incorrect HTTP method for uri [/twitter/_doc?pretty] and method [PUT], allowed:[POST]","status":405

## document 조회 "kimchy"

     GET /twitter/_doc/_search?q=kimchy

## document 변경

     POST /twitter/_doc/1/_update
     {
       "doc" : {
         "message" : "learn Elastic Stack"
       }
     }
     
## document 변경 확인

     GET /twitter/_doc/1

## 스냅샷 생성

     repository를 생성한다.
     elasticsearch.yml에 먼저 path.repo: [""] 설정
     
     PUT /_snapshot/my_backup
     {
       "type": "fs",
       "settings": {
           "compress": true,
           "location": "/home/elasticsearch/elastic/backup"
       }
     }

## 스냅샷을 실행한다.

     PUT _snapshot/my_backup/111_20190527
     {
        "indices": "twitter",
        "ignore_unavailable": true,
        "include_global_state": false
     }

## 스냅샷 상황

     GET _snapshot/my_backup/111_20190527/_status

## document 삭제

     DELETE /twitter/_doc/1

## document 삭제 확인

     GET /twitter/_doc/1

## index 삭제
     
     DELETE /twitter

## index 삭제 확인
     
     GET /twitter

## 데이터 복원하기

     POST _snapshot/my_backup/111_20190527/_restore
     {
       "indices": "twitter"
     }

## _bulk 단일 기능

     POST _bulk
     {"index":{"_index" : "persons","_id":"1", "_type":"person"}}
     {"name":"John Doe"}
     {"index":{"_index" : "persons","_id":"2", "_type":"person"}}
     {"name":"Jane Doe" }

## _bulk 인덱스, 타입 설정

     POST /lectures/lecture/_bulk
     { "index" : { "_id" : "1" } }
     { "title" : "Elasticsearch", "teacher" : "Tak", "hour" : 8 } 
     { "index" : {  "_id" : "2" } } 
     { "title" : "Logstash and beats", "author" : "Tak", "hour" : 4}

## _bulk 복합 설정

     POST /lectures/lecture/_bulk
     { "delete" : { "_id" : "1" } }
     { "update" : {  "_id" : "2" } } 
     { "doc" : {"title":"Elasticsearch, Logstash and beats", "author" : "Tak", "hour" : 12}}
