# 용어 색인

> 개념 풀이에 나온 용어를 모았습니다 · [전체 색인](README.md)

| 용어 | 뜻 | 나온 글 |
|---|---|---|
| @Cacheable | Spring 에서 메서드 결과를 캐시에 저장하고 다음 호출에 저장된 값을 돌려주게 하는 애너테이션입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| BI 도구(Business Intelligence) | 데이터를 불러와 대시보드와 보고서를 만드는 도구입니다. Tableau · Looker 가 대표적입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| Marquez | OpenLineage 이벤트를 받아 메타데이터를 모으고 보여 주는 LF AI & Data 재단의 참조 구현입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| MetricFlow | dbt Semantic Layer 에서 지표 정의를 읽고 요청에 맞는 조인과 집계를 처리하는 엔진입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| ODCS(Open Data Contract Standard) | Bitol 프로젝트가 공개한, 데이터 계약을 YAML 파일로 적는 표준입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| OpenLineage | 작업 실행 중에 계보 정보를 주고받는 형식을 정한 오픈 표준으로, LF AI & Data 재단의 졸업 프로젝트입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| ShedLock | 정기 작업이 여러 서버에서 겹쳐 돌지 않도록 DB · Redis 등에 락을 거는 Java 라이브러리입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| TTL(Time To Live) | 캐시 항목이 저장된 뒤 만료될 때까지의 시간입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| Unity Catalog | Databricks 의 데이터 · 권한 · 계보 관리 기능으로, Databricks 에서 실행한 쿼리의 계보를 자동으로 남깁니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| VLDB(Very Large Data Bases) | 데이터베이스 분야의 국제 학회로, 이 글의 확률적 조기 만료 논문이 실린 곳입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| WAL(Write-Ahead Log) | PostgreSQL 이 데이터 파일을 바꾸기 전에 변경 내용을 먼저 적어 두는 기록 파일로, 계속 보관하면 시점 복구에 씁니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| ZooKeeper | 분산 시스템의 설정 · 락 · 리더 선출을 맡는 Apache 오픈소스 조정 서비스입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| dbt | 데이터 웨어하우스 안의 데이터를 SQL 로 가공 · 관리하는 오픈소스 도구로, dbt Semantic Layer 는 그 위에서 지표를 정의하는 기능입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 가비지 컬렉션(Garbage Collection) | JVM 같은 런타임이 쓰지 않는 메모리를 회수하는 작업으로, 이 동안 애플리케이션 스레드가 잠시 멈출 수 있습니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 깨지는 변경(Breaking Change) | 컬럼 삭제나 자료형 변경처럼 기존 소비자가 고치지 않으면 실패하게 되는 변경입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 다음 최선 행동(Next Best Action) | 고객마다 지금 할 수 있는 행동 후보 가운데 규칙과 점수로 하나를 골라 실행하는 의사결정 방식입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 단조 증가(Monotonically Increasing) | 값이 줄지 않고 늘기만 하는 성질로, 토큰의 앞뒤를 비교하려면 이 성질이 필요합니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 데이터 계보(Data Lineage) | 데이터가 어느 원천에서 어떤 작업을 거쳐 지금의 테이블 · 컬럼이 되었는지 기록한 연결 정보입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 데이터 계약(Data Contract) | 데이터를 만드는 쪽이 쓰는 쪽에게 구조 · 품질 · 갱신 주기 · 책임자를 문서로 약속하고 기계로 검사하는 방식입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 데이터 웨어하우스(Data Warehouse) | 여러 원천의 데이터를 분석용으로 모아 둔 저장소입니다. Snowflake 가 대표적인 클라우드 제품입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 데이터셋(Dataset) | 테이블이나 파일처럼 작업이 읽고 쓰는 데이터 묶음입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 도그파일링(Dog-piling) | 캐시 스탬피드의 다른 이름으로, 여러 요청이 한 자원에 한꺼번에 몰리는 모습을 가리킵니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 도메인 스튜어드(Domain Steward) | 한 업무 영역의 용어와 지표 정의를 승인하고 관리하는 담당자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 리스(Lease) | 정해진 시간이 지나면 저절로 풀리는 락으로, 락을 쥔 서버가 죽어도 영원히 잠기지 않게 합니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 마이그레이션(Migration) | DB 의 테이블 · 컬럼 구조를 바꾸는 스크립트와 그 적용 과정입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 모델 계약(Model Contract) | dbt 에서 모델 결과의 컬럼 이름 · 자료형 · 제약을 약속하고, 맞지 않으면 빌드를 실패시키는 설정입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 복구 시간 목표(Recovery Time Objective) | RTO 라고 줄여 쓰며, 서비스가 끊긴 때부터 다시 열릴 때까지 허용할 수 있는 최대 시간입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 복구 시점 목표(Recovery Point Objective) | RPO 라고 줄여 쓰며, 마지막 복구 지점 이후 잃어도 되는 데이터의 최대 시간 범위입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 분산 락(Distributed Lock) | 여러 서버 가운데 한 번에 하나만 어떤 작업을 하도록 공유 저장소에 거는 잠금입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 서비스 수준 약속(Service-Level Agreement) | 갱신 주기 · 가용성처럼 데이터나 서비스가 지켜야 할 수준을 정한 약속으로, 흔히 SLA 라고 줄여 씁니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 성향(Propensity) | 고객이 특정 제안을 받아들일 것으로 모델이 예측한 확률입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 스키마(Schema) | 컬럼 이름 · 자료형 · 빈 값 허용 여부처럼 데이터의 구조를 정한 정의입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 시맨틱 레이어(Semantic Layer) | 원천 테이블과 화면 사이에서 업무 용어와 지표의 정의를 한 곳에 모아 두는 계층입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 시점 복구(Point-in-Time Recovery) | PITR 이라고 줄여 쓰며, 기본 백업에 변경 기록을 원하는 시각까지 재생해 그 시각의 상태로 되돌리는 복구 방식입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 실행(Run) | 작업이 한 번 돈 기록으로, 시작 · 완료 · 실패 같은 상태를 가집니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 온톨로지(Ontology) | 어떤 분야의 개념과 개념 사이 관계를 정해 둔 체계로, 여기서는 업무 용어의 승인된 의미를 담습니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 웜 스탠바이(Warm Standby) | 작은 규모로 계속 돌아가는 사본을 복구 지역에 두고, 장애 때 규모만 키우는 재해 복구 방식입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 자격 조건(Eligibility) | 법이나 업무상 반드시 지켜야 하는 조건으로, 어긋나는 후보를 가장 먼저 제외합니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 재해 복구(Disaster Recovery) | 서버 · 데이터센터 · 지역 단위의 큰 장애 뒤에 서비스와 데이터를 되살리는 계획과 절차입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 적용성 조건(Applicability) | 고객이 이미 가진 상품처럼 지금 상황에 맞지 않는 후보를 제외하는 조건입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 적합성 조건(Suitability) | 고객에게 실제로 도움이 되는 제안인지를 보고 맞지 않는 후보를 제외하는 조건입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 접촉 정책(Contact Policy) | 노출 · 클릭 · 무시 횟수를 기간별로 세어 같은 행동을 일정 기간 보이지 않게 하는 규칙입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 중재(Arbitration) | 걸러 내기를 통과한 후보에 우선순위 점수를 매겨 맨 위 행동을 고르는 단계입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 지표(Metric) | 매출 · 객단가처럼 원천 값을 집계해 업무 판단에 쓰는 숫자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 캐시 미스(Cache Miss) | 요청한 값이 캐시에 없어 원래 저장소에서 다시 읽거나 계산해야 하는 상황입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 커넥션 풀(Connection Pool) | DB 연결을 미리 몇 개 열어 두고 요청마다 빌려 쓰는 구조로, 개수가 정해져 있어 다 쓰이면 나머지 요청이 기다립니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 컬럼 수준 계보(Column-level Lineage) | 출력 컬럼 하나가 어느 입력 컬럼에서 어떤 변환을 거쳐 왔는지까지 기록한 계보입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 특징값(Feature) | 모델에 넣는 입력값으로, 최근 30일 평균 잔액처럼 원천 데이터를 가공해 만듭니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 파일럿 라이트(Pilot Light) | DB 같은 핵심 자원만 복구 지역에 켜 두고, 애플리케이션 서버는 장애 때 켜는 재해 복구 방식입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 패싯(Facet) | OpenLineage 에서 데이터셋 · 작업 · 실행에 붙이는 추가 메타데이터 조각입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 펜싱 토큰(Fencing Token) | 락을 내줄 때마다 커지는 번호로, 저장소가 이미 본 번호보다 작은 번호의 쓰기를 거절하는 데 씁니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 합의 알고리즘(Consensus Algorithm) | 일부 서버가 죽거나 끊겨도 여러 서버가 같은 값에 동의하게 하는 절차로, Raft · Paxos 가 대표적입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 확률적 조기 만료(Probabilistic Early Expiration) | 만료 시각에 가까울수록 높아지는 확률로 각 요청이 미리 재계산할지 정하게 해 재계산 시점을 흩어 놓는 방법입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
