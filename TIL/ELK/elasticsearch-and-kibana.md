# Elasticsearch와 Kibana

#### Elasticsearch
- 데이터 저장, 검색 및 분석
- 아파치 루씬을 사용해서 구현된 검색엔진
- 아파치 루씬은 풀텍스트 검색 엔진을 만들 수 있는 자바 라이브러리
- 분산 시스템이며 쉽게 스케일 아웃이 가능하고 고가용성을 보장
- 데이터를 샤드 단위로 분리해서 저장하며, 노드(Elasticsearch 실행 프로세스)를 여러개 실행시키면 같은 클러스터로 묶임, 샤드들은 각각의 노드들에 분배되어 저장되고 샤드의 복제본들을 만들어서 복제본은 서로 다른 노드에 저장하기 때문에 시스템 다운이나 네트워크 단절 등으로 유실된 노드가 생기면 다른 살아있는 노드에서 샤드를 복제
- 노드의 수가 줄어들어도 샤드의 수는 변함 없이 무결성을 유지
- JSON 도큐먼트를 기반으로 동작하며, 쿼리와 관리 명령들도 JSON 형식

|          | RDBMS | 검색엔진 |
|:--------:|:--------:|:--------:|
| 데이터 저장 방식 | 정규화 | 역정규화 |
| 전문(Full Text) 검색 속도 | 느림 | 빠름 |
| 의미 검색 | 불가능 | 가능 |
| Join | 가능 | 불가능 |
| 수정 / 삭제 | 빠름 | 느림 |

#### Kibana
- 시각화, 스택 관리 도구

#### Beats, Logstash
- 데이터 수집

---
#### 참고

https://velog.io/@jakeseo_me/%EB%B2%88%EC%97%AD-%EC%97%98%EB%9D%BC%EC%8A%A4%ED%8B%B1%EC%84%9C%EC%B9%98%EC%99%80-%ED%82%A4%EB%B0%94%EB%82%98-%EC%8B%A4%EC%9A%A9%EC%A0%81%EC%9D%B8-%EC%86%8C%EA%B0%9C%EC%84%9C