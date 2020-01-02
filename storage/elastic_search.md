Elastic Search
--------------

### Introduce ES 

- schemaless and document-oriented
- 아파치 루씬 기반
- 실시간 분석 (refresh_interval)
- 분산 시스템
- 멀티 테넌시
- Inverted Index

### 구성

- Document
- Type
- Index
- Mapping
- Index Template

### Analyzer

필드의 데이터에서 용어를 추출하고 색인을 만든다.

1. Character filters: >= 0
2. Tokenizer: >= 1
3. Token filters: >= 0

자동완성을 위해 token filter 에서 edge_ngram 을 사용한다.

keyword 타입은 noop analyzer 가 적용이 된다.
scaled_float 은 저장공간의 효울을 높인다. 값을 가져와서 계산 한 뒤에 리턴하는 구조

### Query
Tip: 성능 향상을 위해 Filter Context 에서 실행할 수 있도록 하자.

#### Low Level Query

- Range
- Exist
- Term
- Bool
    - must, should 는 Query Context, filter, must_not 은 Filter Context
    - Filter Context 에 실행이 될 때는 `constant_score` 로 감쌀 필요가 없다 

Query Context
- score 를 계산한다.

Filter Context
- score 를 계산하지 않고 모든 결과에 score 를 1로 한다. (constant_score)  

#### High Level Query

Match 
- Low Level 쿼리로 변환된다.
- 기본 동작은 or operator 가 동작한다. 
- or 에서 minimum_should_match 를 활용할 수 있다. 
- Fuzziness

Match Phrase
- 연속된 문자열의 매칭을 사용한다. 

Multi Match
- 여러 필드에 대해서 검색한다.

### Deployment Architecture
- master
- coordinate node
- data node
- ingest node

# References

- [Use Cases · Elastic Stack Success Stories | Elastic](https://www.elastic.co/use-cases)
- [Running a 400+ Node Elasticsearch Cluster](https://underthehood.meltwater.com/blog/2018/02/06/running-a-400+-node-es-cluster/)
- [How many nodes can a cluster have?](https://discuss.elastic.co/t/how-many-nodes-can-a-cluster-have/156187)
- [Annotation by Elasticsearch in Grafana](https://grafana.com/docs/grafana/latest/features/datasources/elasticsearch/#annotations)
