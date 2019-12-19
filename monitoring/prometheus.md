Prometheus
----------

### Usage stats parameter
  
label 의 검색 조건을 `.*` 를 했을 때와, 생략 했을 때의 성능 비교를 해보고 싶어졌다. 
`Execution Plan` 또는 `Profile` 기능이 있으면 좋겠지만, 발견하지 못해 `stats=true` 통해 짧은 정보를 확인할 수 있었다.
결론적으로 7일 정도의 데이터를 질의 할 때, 10MB 정도의 응답 결과에 있어 실행시간은 크게 차이가 없었다.  
    
```shell script
# logtail_counter_total{role="benefit", type="all", tail="nginx-access"
curl -s http://xxx.xxx.net/api/v1/query_range\?query\=increase\(logtail_counter_total%7Brole%3D%22benefit%22%2C%20type%3D%22all%22%2C%20tail%3D%22nginx-access%22%7D%5B1m%5D\)\&stats\=true\&start\=1576142400\&end\=1576747200\&step=1200 | jq .data.stats                                                                                
{
  "timings": {
    "evalTotalTime": 0.366902293,
    "resultSortTime": 0.000303381,
    "queryPreparationTime": 0.01638436,
    "innerEvalTime": 0.350181167,
    "execQueueTime": 7.41e-07,
    "execTotalTime": 0.366909237
  }
}

# logtail_counter_total{role="benefit", instance_id=~".*", type="all", tail="nginx-access" 
curl -s http://xxx.xxx.net/api/v1/query_range\?query\=increase\(logtail_counter_total%7Brole%3D%22benefit%22%2C%20instance_id%3D\~%22.\*%22%2C%20type%3D%22all%22%2C%20tail%3D%22nginx-access%22%7D%5B1m%5D\)\&stats\=true\&start\=1576142400\&end\=1576747200\&step=1200 | jq .data.stats                                                 
{
  "timings": {
    "evalTotalTime": 0.376544098,
    "resultSortTime": 0.000301584,
    "queryPreparationTime": 0.027984909,
    "innerEvalTime": 0.348227483,
    "execQueueTime": 7.99e-07,
    "execTotalTime": 0.376549879
  }
}

```
