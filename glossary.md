# 용어 색인

> 개념 풀이에 나온 용어를 모았습니다 · [전체 색인](README.md)

| 용어 | 뜻 | 나온 글 |
|---|---|---|
| @Cacheable | Spring 에서 메서드 결과를 캐시에 저장하고 다음 호출에 저장된 값을 돌려주게 하는 애너테이션입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| BI 도구(Business Intelligence) | 데이터를 불러와 대시보드와 보고서를 만드는 도구입니다. Tableau · Looker 가 대표적입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| MetricFlow | dbt Semantic Layer 에서 지표 정의를 읽고 요청에 맞는 조인과 집계를 처리하는 엔진입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| TTL(Time To Live) | 캐시 항목이 저장된 뒤 만료될 때까지의 시간입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| VLDB(Very Large Data Bases) | 데이터베이스 분야의 국제 학회로, 이 글의 확률적 조기 만료 논문이 실린 곳입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| dbt | 데이터 웨어하우스 안의 데이터를 SQL 로 가공 · 관리하는 오픈소스 도구로, dbt Semantic Layer 는 그 위에서 지표를 정의하는 기능입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 데이터 웨어하우스(Data Warehouse) | 여러 원천의 데이터를 분석용으로 모아 둔 저장소입니다. Snowflake 가 대표적인 클라우드 제품입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 도그파일링(Dog-piling) | 캐시 스탬피드의 다른 이름으로, 여러 요청이 한 자원에 한꺼번에 몰리는 모습을 가리킵니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 도메인 스튜어드(Domain Steward) | 한 업무 영역의 용어와 지표 정의를 승인하고 관리하는 담당자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 시맨틱 레이어(Semantic Layer) | 원천 테이블과 화면 사이에서 업무 용어와 지표의 정의를 한 곳에 모아 두는 계층입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 온톨로지(Ontology) | 어떤 분야의 개념과 개념 사이 관계를 정해 둔 체계로, 여기서는 업무 용어의 승인된 의미를 담습니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 지표(Metric) | 매출 · 객단가처럼 원천 값을 집계해 업무 판단에 쓰는 숫자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 캐시 미스(Cache Miss) | 요청한 값이 캐시에 없어 원래 저장소에서 다시 읽거나 계산해야 하는 상황입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 커넥션 풀(Connection Pool) | DB 연결을 미리 몇 개 열어 두고 요청마다 빌려 쓰는 구조로, 개수가 정해져 있어 다 쓰이면 나머지 요청이 기다립니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 확률적 조기 만료(Probabilistic Early Expiration) | 만료 시각에 가까울수록 높아지는 확률로 각 요청이 미리 재계산할지 정하게 해 재계산 시점을 흩어 놓는 방법입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
