## Elastic Search란?

Apache Lucene 기반의 서치 엔진이다.

Kibana, Logstash, beats 등으로 이루어진 Elastic Stack의 기초가 된다.


## Elastic Search의 특징

### 역인덱싱

정방향 인덱싱을 하는 기존의 RDB와 달리 Elastic Search는 역방향 인덱싱을 이용해, 내용을 통한 빠른 검색을 가능하게 한다.

이 과정에서 Text를 Token 단위로 잘라내는 Tokenizing 과정을 거친다.

- 정방향
    
    
    | Document | Text |
    | --- | --- |
    | document 1 | 배 고 파 |
    | document 2 | 배 아 파 |
    | document 3 | 배 스 킨 |
- 역방향
    
    
    | Token | Documents |
    | --- | --- |
    | 배 | 1, 2, 3 |
    | 파 | 1, 2 |
    | 아 | 2 |
    | 고 | 1 |
    | 스 | 3 |
    | 킨 | 3 |

## Elastic Search의 단위

### Index

ES의 기본적인 단위, 비슷한 특징을 가진 데이터 (=document)의 집합이다.

RDB의 테이블과 비슷한 개념이다.

### Documents, fields

ES는 데이터를 JSON 형태로 저장한다.

```jsx
{
  "_index": "my-first-elasticsearch-index",
  "_id": "DyFpo5EBxE8fzbb95DOa",
  "_version": 1,
  "_seq_no": 0,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "email": "john@smith.com",
    "first_name": "John",
    "last_name": "Smith",
    "info": {
      "bio": "Eco-warrior and defender of the weak",
      "age": 25,
      "interests": [
        "dolphins",
        "whales"
      ]
    },
    "join_date": "2024/05/01"
  }
}
```

위와 같은 형태가 되는데, key:value pair 하나가 각각 한 개의 필드이고, 이런 식으로 여러 개의 필드가 모여 document가 된다. (RDB의 record와 비슷)

필드는 주로 유저가 정의하지만, 몇몇 metadata field가 있다. 

예시: 

- `_index`: document가 저장된 인덱스
- `_id`: document의 ID (유일해야 함)

## Mapping

document를 저장하기 위해선 mapping이라는, 데이터가 어떻게 인덱스화되어 저장될 지 정하는 과정을 거친다. 각 필드 당 타입도 정해줘야 한다.

1. 동적 매핑 :  인덱스에 새로운 필드를 가진 도큐먼트를 넣으면 ES가 자동으로 매핑해줌
2. 명시적 매핑 : 필드 당 데이터 타입을 개발자가 명시함

## 데이터 검색

REST API 를 통한 DB에 직접 접근, 프로그램을 통해 접근, Kibana로 접근 등이 있다.

### Query DSL

기본적인 ES 조작 언어, JSON 스타일로 작성

- Full-text search
    - Document의 내용에서 쿼리 내용과 가장 관련있는 결과를 반환하는 검색으로, 데이터 타입이 text인 필드들이 인덱싱되어 검색 대상이 된다.
    - 검색 과정: 텍스트 변환(토큰화) → 역방향 인덱스 테이블 생성 → 연관도 측정
    - 쿼리 대상도 같은 과정을 거친다.
    - 검색 예시
        
        ```jsx
        GET /_search
        {
          "query": {
            "match": {   <---------- "match" 를 사용한다!
              "message": {
                "query": "this is a test"
              }
            }
          }
        }
        ```
        
- Keyword Search
    - 데이터 타입이 text가 아닌 keyword 에서 검색해 정확히 매칭되는 결과 반환
    - 검색 예시
        
        ```jsx
        #1 mapping 정의
        PUT products
        {
          "mappings": {
            "_doc": {
              "properties": {
                "name": {
                  "type": "keyword"  <------ 이걸 "type": "text" 로 바꾸면 결과가 나온다.
                }
              }
            }
          }
        }
        
        #2 insert value
        POST products/_doc
        {
          "name": "washing machine"
        }
        
        #3 search
        GET products/_search
        {
          "query": {
            "match": {
              "name": "washing"
            }
          }
        }
        // match doc 없음
        ```
        
- Semantic Search
    - kNN 알고리즘을 활용해 벡터 검색으로 쿼리의 의미에 부합하는 결과 반환

### ES|QL

- 8.11 버전에서 신규 도입, Query DSL과 완전히 동기화되진 않음

## 과정

1. 인덱스 만들기
    
    ```jsx
    PUT /books
    ```
    
2. document 인덱스에 더하기 
    1. 동적 매핑 사용
    
    ```jsx
    POST books/_doc
    {
      "name": "Snow Crash",
      "author": "Neal Stephenson",
      "release_date": "1992-06-01",
      "page_count": 470
    }
    
    POST /books/_doc
    {
      "name": "The Great Gatsby",
      "author": "F. Scott Fitzgerald",
      "release_date": "1925-04-10",
      "page_count": 180,
      "language": "EN"  <--- 이런 식으로 새로운 필드도 타입 명시 없이 추가할 수 있다.
    }
    ```
    
    - 실제 저장되는 타입
        
        ```jsx
        GET /books/_mapping
        
        {
          "books": {
            "mappings": {
              "properties": {
                "author": {
                  "type": "text",
                  "fields": {
                    "keyword": {
                      "type": "keyword",
                      "ignore_above": 256
                    }
                  }
                },
                "name": {
                  "type": "text",
                  "fields": {
                    "keyword": {
                      "type": "keyword",
                      "ignore_above": 256
                    }
                  }
                },
                "language": {
                  "type": "text",
                  "fields": {
                    "keyword": {
                      "type": "keyword",
                      "ignore_above": 256
                    }
                  }
                },
                "page_count": {
                  "type": "long"
                },
                "release_date": {
                  "type": "date"
                }
              }
            }
          }
        }
        
        ```
        
    
    b. 동적 매핑 대신 명시적 매핑 후 도큐먼트 더하기
    
    ```jsx
    PUT /my-explicit-mappings-books
    {
      "mappings": {
        "dynamic": false,  
        "properties": {  
          "name": { "type": "text" },
          "author": { "type": "text" },
          "release_date": { "type": "date", "format": "yyyy-MM-dd" },
          "page_count": { "type": "integer" }
        }
      }
    }
    ```
    
3. 검색하기
    
    ```jsx
    GET books/_search
    {
      "query": {
        "match": {
          "name": "brave"
        }
      }
    }
    ```
    
    - 결과
        
        ```jsx
        {
          "took": 9,
          "timed_out": false,
          "_shards": {
            "total": 5,
            "successful": 5,
            "skipped": 0,
            "failed": 0
          },
          "hits": {
            "total": {
              "value": 1,
              "relation": "eq"
            },
            "max_score": 0.6931471, 
            "hits": [
              {
                "_index": "books",
                "_id": "CwICQpIBO6vvGGiC_3Ls",
                "_score": 0.6931471,
                "_source": {
                  "name": "Brave New World",
                  "author": "Aldous Huxley",
                  "release_date": "1932-06-01",
                  "page_count": 268
                }
              }
            ]
          }
        }
        ```
        

## RBD와 비교

| 항목 | **Elasticsearch** | **RDB(관계형 데이터베이스)** |
| --- | --- | --- |
| **데이터 구조** | 비정형, 유연한 스키마 
(JSON 기반 데이터 저장) | 테이블 기반 정형 스키마 
(데이터 저장 위해 스키마에 부합해야 함) |
| **풀텍스트 검색 속도** | 빠름 (풀텍스트 검색 최적화된 역인덱스) | LIKE문 사용으로 느릴 수 있음 |
| **쿼리 방식** | Query DSL | SQL (표준 쿼리 언어) |
| **트랜잭션 지원** | 미지원 (NoSQL 특성) | 지원, ACID 보장 (정합성 유지) |
| **Create** | JSON 기반으로 빠름 | 트랜잭션, 인덱스 최적화 등 더 느릴 수 있음 |
| **Read** | id 기반: O(1)
텍스트 기반 : 매우 빠름 | id 기반 : O(log N)
텍스트 기반 : 느림 (인덱스 따라 다름) |
| **Delete** | 느림 (소프트 삭제 후 백그라운드에서 진행) | 빠름 (삭제 즉시 진행) |
| **Update** | 느림 (기존 문서 삭제 후 수정된 내용으로 재생성 필요) | 빠름 (필드 단위 업데이트 가능) |
| **Join** | 미지원, JSON 문서 중첩하는 Nested 형식으로 저장할 수 있으나 비효율적임 | 지원, 인덱스 통한 최적화 가능, 효율적임 |
