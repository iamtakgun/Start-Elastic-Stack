# Elasticsearch URL Search

## javabooks 인덱스, _doc 타입에서 Schildt 검색

    GET /javabooks/_doc/_search?q=Schildt

## javabooks 인덱스 Schildt 검색

    GET /javabooks/_search?q=Schildt

## Multi-Tenancy는 여러 index에서 동시 검색 기능

    index,index로 표시
    javabooks, pythonbooks 인덱스에서 Beginner 검색

    GET /javabooks,pythonbooks/_search?q=Beginner

## 인덱스 지정을 생략, 전체 인덱스에서 Beginner 검색

    GET /_search?q=Beginner


## 전체 인덱스의 title 필드에서 Beginner 검색

    특정 필드 검색하려면 q=필드명:질의어 형식 지정

    GET /_search?q=title:Beginner

## title에 Beginner, Java 을 AND 조건 검색

    %20은 공백 대신 공백의 url인 코딩값

    GET /_search?q=title:Beginner%20AND%20Java

## title에 Beginner, Java 을 OR 조건 검색

    GET /_search?q=title:Beginner%20OR%20Java

## df(default field) 매개 변수를 사용해서 title 필드에서 Edition 검색

    GET /_search?q=Edition&df=title

## default_operator 매개 변수를 사용해서 기본 조건명령어를 AND로 지정

    공백은 OR
    GET /_search?q=title:Edition%20Java&default_operator=AND

## pages 필드를 기준으로 오름차순 정렬

    GET /javabooks/_search?q=title:Edition&sort=pages

## pages 필드를 기준으로 내림차순 정렬

    GET /javabooks/_search?q=title:Edition&sort=pages:desc

# Elasticsearch Request body Search

    소문자로 해야 한다.

    GET /javabooks/_search
    {
        "query" : {
            "term" : { "author" : "schildt" }
        }
    }

## size로 출력 제한

    GET /javabooks/_search
    {
        "size":5
    }


## _source로 title과 beg으로 시작 하는 필드명의 값 표시

    GET /pythonbooks/_search
    {
        "_source": ["title","beg*"]
    }

## Delete By Query로 document 삭제

    POST /javabooks/_delete_by_query
    {
      "query": {
        "term": {
            "author" : "schildt"
        }
      }
    }
