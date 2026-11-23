# 용어 색인

> 개념 풀이에 나온 용어를 모았습니다 · [전체 색인](README.md)

| 용어 | 뜻 | 나온 글 |
|---|---|---|
| @Cacheable | Spring 에서 메서드 결과를 캐시에 저장하고 다음 호출에 저장된 값을 돌려주게 하는 애너테이션입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| ANALYZE | 테이블의 값 분포 통계를 새로 모아 질의 계획기가 알맞은 실행 계획을 고르게 하는 PostgreSQL 명령입니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| BI 도구(Business Intelligence) | 데이터를 불러와 대시보드와 보고서를 만드는 도구입니다. Tableau · Looker 가 대표적입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| COPY | 파일이나 DB 연결로 받은 여러 행을 한 명령으로 테이블에 넣거나 꺼내는 PostgreSQL 전용 명령입니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| DTO 프로젝션(DTO Projection) | 엔티티 전체 대신 화면에 필요한 열만 골라 전용 객체로 읽는 조회 방식입니다. | [요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유](notes/tech/framework/2026-11-23-open-session-in-view.md) |
| HikariCP | Spring Boot 가 기본으로 쓰는 JDBC 커넥션 풀 라이브러리입니다. | [요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유](notes/tech/framework/2026-11-23-open-session-in-view.md) |
| Marquez | OpenLineage 이벤트를 받아 메타데이터를 모으고 보여 주는 LF AI & Data 재단의 참조 구현입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| MetricFlow | dbt Semantic Layer 에서 지표 정의를 읽고 요청에 맞는 조인과 집계를 처리하는 엔진입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| N+1 문제(N+1 Query Problem) | 목록 쿼리 한 번 뒤에 항목마다 추가 쿼리가 한 번씩 더 나가는 현상입니다. | [요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유](notes/tech/framework/2026-11-23-open-session-in-view.md) |
| ODCS(Open Data Contract Standard) | Bitol 프로젝트가 공개한, 데이터 계약을 YAML 파일로 적는 표준입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| OpenLineage | 작업 실행 중에 계보 정보를 주고받는 형식을 정한 오픈 표준으로, LF AI & Data 재단의 졸업 프로젝트입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| REPACK | PostgreSQL 19 에서 VACUUM FULL 과 CLUSTER 를 대체하는 명령이며, CONCURRENTLY 옵션으로 읽기 · 쓰기를 막지 않고 테이블을 다시 씁니다. | [다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유](notes/tech/database/2026-11-20-postgresql-mvcc-vacuum-dead-tuples.md) |
| ShedLock | 정기 작업이 여러 서버에서 겹쳐 돌지 않도록 DB · Redis 등에 락을 거는 Java 라이브러리입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| TTL(Time To Live) | 캐시 항목이 저장된 뒤 만료될 때까지의 시간입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| Tomcat | Spring Boot 웹 애플리케이션이 기본으로 내장하는 Java 웹 서버이자 서블릿 컨테이너입니다. | [작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지](notes/tech/server/2026-11-19-tomcat-thread-pool-accept-queue.md) |
| Unity Catalog | Databricks 의 데이터 · 권한 · 계보 관리 기능으로, Databricks 에서 실행한 쿼리의 계보를 자동으로 남깁니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| VLDB(Very Large Data Bases) | 데이터베이스 분야의 국제 학회로, 이 글의 확률적 조기 만료 논문이 실린 곳입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| WAL(Write-Ahead Log) | PostgreSQL 이 데이터 파일을 바꾸기 전에 변경 내용을 먼저 적어 두는 기록 파일로, 계속 보관하면 시점 복구에 씁니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| ZooKeeper | 분산 시스템의 설정 · 락 · 리더 선출을 맡는 Apache 오픈소스 조정 서비스입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| autovacuum | 죽은 튜플 수가 기준값을 넘은 테이블을 골라 VACUUM 과 ANALYZE 를 자동으로 실행하는 PostgreSQL 의 백그라운드 프로세스입니다. | [다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유](notes/tech/database/2026-11-20-postgresql-mvcc-vacuum-dead-tuples.md) |
| dbt | 데이터 웨어하우스 안의 데이터를 SQL 로 가공 · 관리하는 오픈소스 도구로, dbt Semantic Layer 는 그 위에서 지표를 정의하는 기능입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| pg_fastload | pg_bulkload 를 바탕으로 읽기 · 파싱 단계를 병렬로 나눈 PostgreSQL 대량 적재 오픈소스 도구입니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| somaxconn | Linux 에서 listen 대기열 길이의 상한을 정하는 커널 설정 값(net.core.somaxconn)입니다. | [작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지](notes/tech/server/2026-11-19-tomcat-thread-pool-accept-queue.md) |
| 가비지 컬렉션(Garbage Collection) | JVM 같은 런타임이 쓰지 않는 메모리를 회수하는 작업으로, 이 동안 애플리케이션 스레드가 잠시 멈출 수 있습니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 가상 스레드(Virtual Thread) | Java 21 부터 정식 기능이 된, 운영체제 스레드보다 훨씬 가볍게 많이 만들 수 있는 JVM 관리 스레드입니다. | [작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지](notes/tech/server/2026-11-19-tomcat-thread-pool-accept-queue.md) |
| 깨지는 변경(Breaking Change) | 컬럼 삭제나 자료형 변경처럼 기존 소비자가 고치지 않으면 실패하게 되는 변경입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 다음 최선 행동(Next Best Action) | 고객마다 지금 할 수 있는 행동 후보 가운데 규칙과 점수로 하나를 골라 실행하는 의사결정 방식입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) | 행의 여러 버전을 함께 두어 읽기와 쓰기가 서로를 막지 않게 하는 방식입니다. | [다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유](notes/tech/database/2026-11-20-postgresql-mvcc-vacuum-dead-tuples.md) |
| 단조 증가(Monotonically Increasing) | 값이 줄지 않고 늘기만 하는 성질로, 토큰의 앞뒤를 비교하려면 이 성질이 필요합니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 대량 적재(Bulk Loading) | 많은 행을 DB 에 한꺼번에 넣는 작업과, 그때 행마다 반복되는 비용을 줄이는 방법입니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| 데이터 계보(Data Lineage) | 데이터가 어느 원천에서 어떤 작업을 거쳐 지금의 테이블 · 컬럼이 되었는지 기록한 연결 정보입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 데이터 계약(Data Contract) | 데이터를 만드는 쪽이 쓰는 쪽에게 구조 · 품질 · 갱신 주기 · 책임자를 문서로 약속하고 기계로 검사하는 방식입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 데이터 웨어하우스(Data Warehouse) | 여러 원천의 데이터를 분석용으로 모아 둔 저장소입니다. Snowflake 가 대표적인 클라우드 제품입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 데이터셋(Dataset) | 테이블이나 파일처럼 작업이 읽고 쓰는 데이터 묶음입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 도그파일링(Dog-piling) | 캐시 스탬피드의 다른 이름으로, 여러 요청이 한 자원에 한꺼번에 몰리는 모습을 가리킵니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 도메인 스튜어드(Domain Steward) | 한 업무 영역의 용어와 지표 정의를 승인하고 관리하는 담당자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 동결(Freezing) | 충분히 오래된 행을 모든 트랜잭션에게 보이는 상태로 표시해 트랜잭션 ID 재사용에 대비하는 작업입니다. | [다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유](notes/tech/database/2026-11-20-postgresql-mvcc-vacuum-dead-tuples.md) |
| 리스(Lease) | 정해진 시간이 지나면 저절로 풀리는 락으로, 락을 쥔 서버가 죽어도 영원히 잠기지 않게 합니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 리틀의 법칙(Little's Law) | 시스템 안의 평균 요청 수는 평균 도착률과 평균 체류 시간의 곱과 같다는 대기 이론의 관계식입니다. | [작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지](notes/tech/server/2026-11-19-tomcat-thread-pool-accept-queue.md) |
| 마이그레이션(Migration) | DB 의 테이블 · 컬럼 구조를 바꾸는 스크립트와 그 적용 과정입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 모델 계약(Model Contract) | dbt 에서 모델 결과의 컬럼 이름 · 자료형 · 제약을 약속하고, 맞지 않으면 빌드를 실패시키는 설정입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 복구 시간 목표(Recovery Time Objective) | RTO 라고 줄여 쓰며, 서비스가 끊긴 때부터 다시 열릴 때까지 허용할 수 있는 최대 시간입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 복구 시점 목표(Recovery Point Objective) | RPO 라고 줄여 쓰며, 마지막 복구 지점 이후 잃어도 되는 데이터의 최대 시간 범위입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 분산 락(Distributed Lock) | 여러 서버 가운데 한 번에 하나만 어떤 작업을 하도록 공유 저장소에 거는 잠금입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 서비스 수준 약속(Service-Level Agreement) | 갱신 주기 · 가용성처럼 데이터나 서비스가 지켜야 할 수준을 정한 약속으로, 흔히 SLA 라고 줄여 씁니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 성향(Propensity) | 고객이 특정 제안을 받아들일 것으로 모델이 예측한 확률입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 스키마(Schema) | 컬럼 이름 · 자료형 · 빈 값 허용 여부처럼 데이터의 구조를 정한 정의입니다. | [데이터 계약(Data Contract) — 데이터를 넘기는 쪽과 받는 쪽의 약속](notes/domain/data-contracts/2026-11-11-data-contract.md) |
| 스테이징 테이블(Staging Table) | 원천 데이터를 본 테이블에 반영하기 전에 잠시 받아 두고 검사하는 테이블입니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| 시맨틱 레이어(Semantic Layer) | 원천 테이블과 화면 사이에서 업무 용어와 지표의 정의를 한 곳에 모아 두는 계층입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 시점 복구(Point-in-Time Recovery) | PITR 이라고 줄여 쓰며, 기본 백업에 변경 기록을 원하는 시각까지 재생해 그 시각의 상태로 되돌리는 복구 방식입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 실행(Run) | 작업이 한 번 돈 기록으로, 시작 · 완료 · 실패 같은 상태를 가집니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 연결 대기열(Accept Queue) | 운영체제가 연결 수립을 끝냈지만 서버 프로그램이 아직 받아 가지 않은 연결을 쌓아 두는 자리입니다. | [작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지](notes/tech/server/2026-11-19-tomcat-thread-pool-accept-queue.md) |
| 영속성 컨텍스트(Persistence Context) | JPA 가 읽어 온 엔티티를 기억하고 변경을 추적하는 작업 공간입니다. | [요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유](notes/tech/framework/2026-11-23-open-session-in-view.md) |
| 온톨로지(Ontology) | 어떤 분야의 개념과 개념 사이 관계를 정해 둔 체계로, 여기서는 업무 용어의 승인된 의미를 담습니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 웜 스탠바이(Warm Standby) | 작은 규모로 계속 돌아가는 사본을 복구 지역에 두고, 장애 때 규모만 키우는 재해 복구 방식입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 자격 조건(Eligibility) | 법이나 업무상 반드시 지켜야 하는 조건으로, 어긋나는 후보를 가장 먼저 제외합니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 자동 커밋(Auto-commit) | 트랜잭션으로 묶지 않은 SQL 문장을 하나씩 바로 확정하는 실행 모드입니다. | [요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유](notes/tech/framework/2026-11-23-open-session-in-view.md) |
| 자동 커밋(Autocommit) | 문장 하나를 실행할 때마다 바로 커밋하는 동작으로, JDBC 연결의 기본값입니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| 작업 스레드 풀(Worker Thread Pool) | 요청을 실제로 처리하는 스레드를 미리 만들어 두고 돌려 쓰는 묶음입니다. | [작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지](notes/tech/server/2026-11-19-tomcat-thread-pool-accept-queue.md) |
| 재해 복구(Disaster Recovery) | 서버 · 데이터센터 · 지역 단위의 큰 장애 뒤에 서비스와 데이터를 되살리는 계획과 절차입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 적용성 조건(Applicability) | 고객이 이미 가진 상품처럼 지금 상황에 맞지 않는 후보를 제외하는 조건입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 적합성 조건(Suitability) | 고객에게 실제로 도움이 되는 제안인지를 보고 맞지 않는 후보를 제외하는 조건입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 접촉 정책(Contact Policy) | 노출 · 클릭 · 무시 횟수를 기간별로 세어 같은 행동을 일정 기간 보이지 않게 하는 규칙입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 죽은 튜플(Dead Tuple) | 지워졌고 어떤 트랜잭션에게도 더 이상 보이지 않아 회수할 수 있는 행 버전입니다. | [다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유](notes/tech/database/2026-11-20-postgresql-mvcc-vacuum-dead-tuples.md) |
| 준비된 문장(Prepared Statement) | 한 번 파싱 · 계획한 문장을 값만 바꿔 여러 번 실행하는 방식입니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| 중재(Arbitration) | 걸러 내기를 통과한 후보에 우선순위 점수를 매겨 맨 위 행동을 고르는 단계입니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 지연 로딩(Lazy Loading) | 연관된 데이터를 처음 실제로 쓰는 순간에 쿼리를 보내 읽는 방식입니다. | [요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유](notes/tech/framework/2026-11-23-open-session-in-view.md) |
| 지표(Metric) | 매출 · 객단가처럼 원천 값을 집계해 업무 판단에 쓰는 숫자입니다. | [시맨틱 레이어(Semantic Layer) — 지표 정의를 한 곳에 두기](notes/domain/semantic-layer/2026-11-10-semantic-layer.md) |
| 체크포인트(Checkpoint) | 메모리에서 바뀐 데이터 페이지를 모두 디스크에 기록하는 시점으로, 잦아지면 디스크 쓰기가 늘어납니다. | [대량 적재(Bulk Loading) — PostgreSQL 에 수백만 행을 빠르게 넣기](notes/tech/high-volume-processing/2026-11-18-bulk-loading.md) |
| 캐시 미스(Cache Miss) | 요청한 값이 캐시에 없어 원래 저장소에서 다시 읽거나 계산해야 하는 상황입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
| 커넥션 풀(Connection Pool) | DB 연결을 미리 몇 개 열어 두고 요청마다 빌려 쓰는 구조로, 개수가 정해져 있어 다 쓰이면 나머지 요청이 기다립니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) · [요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유](notes/tech/framework/2026-11-23-open-session-in-view.md) |
| 컬럼 수준 계보(Column-level Lineage) | 출력 컬럼 하나가 어느 입력 컬럼에서 어떤 변환을 거쳐 왔는지까지 기록한 계보입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 테이블 부풀음(Table Bloat) | 죽은 튜플과 회수된 빈 공간 때문에 테이블 · 인덱스 파일이 실제 데이터보다 커진 상태입니다. | [다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유](notes/tech/database/2026-11-20-postgresql-mvcc-vacuum-dead-tuples.md) |
| 트랜잭션 ID 랩어라운드(Transaction ID Wraparound) | 32비트 트랜잭션 ID 가 한 바퀴 돌아 오래된 행이 미래의 것으로 잘못 보일 수 있는 상황입니다. | [다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유](notes/tech/database/2026-11-20-postgresql-mvcc-vacuum-dead-tuples.md) |
| 특징값(Feature) | 모델에 넣는 입력값으로, 최근 30일 평균 잔액처럼 원천 데이터를 가공해 만듭니다. | [다음 최선 행동(Next Best Action) — 걸러내기와 점수 매기기로 지금 할 제안 하나 고르기](notes/domain/recommendation-systems/2026-11-17-next-best-action.md) |
| 파일럿 라이트(Pilot Light) | DB 같은 핵심 자원만 복구 지역에 켜 두고, 애플리케이션 서버는 장애 때 켜는 재해 복구 방식입니다. | [복구 시간 목표 · 복구 시점 목표(RTO · RPO, Recovery Time · Point Objective) — 얼마나 빨리, 어디까지 되살릴지 정하기](notes/tech/incident-response/2026-11-16-rto-rpo.md) |
| 패싯(Facet) | OpenLineage 에서 데이터셋 · 작업 · 실행에 붙이는 추가 메타데이터 조각입니다. | [데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기](notes/domain/data-quality-lineage/2026-11-13-data-lineage.md) |
| 펜싱 토큰(Fencing Token) | 락을 내줄 때마다 커지는 번호로, 저장소가 이미 본 번호보다 작은 번호의 쓰기를 거절하는 데 씁니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 폴러(Poller) | Tomcat NIO 커넥터에서 여러 연결을 지켜보다가 데이터가 도착한 연결만 작업 스레드에 넘기는 스레드입니다. | [작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지](notes/tech/server/2026-11-19-tomcat-thread-pool-accept-queue.md) |
| 합의 알고리즘(Consensus Algorithm) | 일부 서버가 죽거나 끊겨도 여러 서버가 같은 값에 동의하게 하는 절차로, Raft · Paxos 가 대표적입니다. | [펜싱 토큰(Fencing Token) — 락이 만료된 뒤 늦게 도착한 쓰기 막기](notes/tech/architecture/2026-11-12-fencing-token.md) |
| 확률적 조기 만료(Probabilistic Early Expiration) | 만료 시각에 가까울수록 높아지는 확률로 각 요청이 미리 재계산할지 정하게 해 재계산 시점을 흩어 놓는 방법입니다. | [캐시 스탬피드(Cache Stampede) — 만료 순간 몰리는 재계산 막기](notes/tech/overload/2026-11-10-cache-stampede.md) |
