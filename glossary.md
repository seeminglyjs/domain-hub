# 용어 색인

> 개념 풀이에 나온 용어를 모았습니다 · [전체 색인](README.md)

| 용어 | 뜻 | 나온 글 |
|---|---|---|
| @Cacheable | Spring 에서 메서드 결과를 캐시에 저장하고 다음 호출에 저장된 값을 돌려주게 하는 애너테이션입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| BI 도구(Business Intelligence) | 데이터를 불러와 대시보드와 보고서를 만드는 도구입니다. Tableau · Looker 가 대표적입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| MetricFlow | dbt Semantic Layer 에서 지표 정의를 읽고 요청에 맞는 조인과 집계를 처리하는 엔진입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| ODCS(Open Data Contract Standard) | Bitol 프로젝트가 공개한, 데이터 계약을 YAML 파일로 적는 표준입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| ShedLock | 정기 작업이 여러 서버에서 겹쳐 돌지 않도록 DB · Redis 등에 락을 거는 Java 라이브러리입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| TTL(Time To Live) | 캐시 항목이 저장된 뒤 만료될 때까지의 시간입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| VLDB(Very Large Data Bases) | 데이터베이스 분야의 국제 학회로, 이 글의 확률적 조기 만료 논문이 실린 곳입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| ZooKeeper | 분산 시스템의 설정 · 락 · 리더 선출을 맡는 Apache 오픈소스 조정 서비스입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| dbt | 데이터 웨어하우스 안의 데이터를 SQL 로 가공 · 관리하는 오픈소스 도구로, dbt Semantic Layer 는 그 위에서 지표를 정의하는 기능입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 가비지 컬렉션(Garbage Collection) | JVM 같은 런타임이 쓰지 않는 메모리를 회수하는 작업으로, 이 동안 애플리케이션 스레드가 잠시 멈출 수 있습니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 깨지는 변경(Breaking Change) | 컬럼 삭제나 자료형 변경처럼 기존 소비자가 고치지 않으면 실패하게 되는 변경입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 단조 증가(Monotonically Increasing) | 값이 줄지 않고 늘기만 하는 성질로, 토큰의 앞뒤를 비교하려면 이 성질이 필요합니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 데이터 계약(Data Contract) | 데이터를 만드는 쪽이 쓰는 쪽에게 구조 · 품질 · 갱신 주기 · 책임자를 문서로 약속하고 기계로 검사하는 방식입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 데이터 웨어하우스(Data Warehouse) | 여러 원천의 데이터를 분석용으로 모아 둔 저장소입니다. Snowflake 가 대표적인 클라우드 제품입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 도그파일링(Dog-piling) | 캐시 스탬피드의 다른 이름으로, 여러 요청이 한 자원에 한꺼번에 몰리는 모습을 가리킵니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 도메인 스튜어드(Domain Steward) | 한 업무 영역의 용어와 지표 정의를 승인하고 관리하는 담당자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 리스(Lease) | 정해진 시간이 지나면 저절로 풀리는 락으로, 락을 쥔 서버가 죽어도 영원히 잠기지 않게 합니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 마이그레이션(Migration) | DB 의 테이블 · 컬럼 구조를 바꾸는 스크립트와 그 적용 과정입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 모델 계약(Model Contract) | dbt 에서 모델 결과의 컬럼 이름 · 자료형 · 제약을 약속하고, 맞지 않으면 빌드를 실패시키는 설정입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 분산 락(Distributed Lock) | 여러 서버 가운데 한 번에 하나만 어떤 작업을 하도록 공유 저장소에 거는 잠금입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 서비스 수준 약속(Service-Level Agreement) | 갱신 주기 · 가용성처럼 데이터나 서비스가 지켜야 할 수준을 정한 약속으로, 흔히 SLA 라고 줄여 씁니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 스키마(Schema) | 컬럼 이름 · 자료형 · 빈 값 허용 여부처럼 데이터의 구조를 정한 정의입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 시맨틱 레이어(Semantic Layer) | 원천 테이블과 화면 사이에서 업무 용어와 지표의 정의를 한 곳에 모아 두는 계층입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 온톨로지(Ontology) | 어떤 분야의 개념과 개념 사이 관계를 정해 둔 체계로, 여기서는 업무 용어의 승인된 의미를 담습니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 지표(Metric) | 매출 · 객단가처럼 원천 값을 집계해 업무 판단에 쓰는 숫자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 캐시 미스(Cache Miss) | 요청한 값이 캐시에 없어 원래 저장소에서 다시 읽거나 계산해야 하는 상황입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 커넥션 풀(Connection Pool) | DB 연결을 미리 몇 개 열어 두고 요청마다 빌려 쓰는 구조로, 개수가 정해져 있어 다 쓰이면 나머지 요청이 기다립니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 펜싱 토큰(Fencing Token) | 락을 내줄 때마다 커지는 번호로, 저장소가 이미 본 번호보다 작은 번호의 쓰기를 거절하는 데 씁니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 합의 알고리즘(Consensus Algorithm) | 일부 서버가 죽거나 끊겨도 여러 서버가 같은 값에 동의하게 하는 절차로, Raft · Paxos 가 대표적입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 확률적 조기 만료(Probabilistic Early Expiration) | 만료 시각에 가까울수록 높아지는 확률로 각 요청이 미리 재계산할지 정하게 해 재계산 시점을 흩어 놓는 방법입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
