# 다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) 와 VACUUM: PostgreSQL 에 죽은 튜플(Dead Tuple)이 쌓이는 이유

> 분류: 기술 › DB · 작성: 2026-11-20 · 실린 노트: [2026-11-20](../../../daily/2026-11-20.md) · [전체 색인](../../../README.md)

**한 줄 정의**: PostgreSQL 은 UPDATE · DELETE 때 옛 행을 바로 지우지 않으므로, VACUUM 이 그 공간을 제때 회수해야 테이블과 인덱스가 커지지 않습니다.

**그날 소식과 연결**: 같은 날 정식 출시된 PostgreSQL 19 는 테이블을 막지 않는 REPACK (CONCURRENTLY) 와 autovacuum 우선순위 점수를 더했습니다.

## 1. 왜 필요한가

배달 앱의 주문 테이블은 주문 하나의 상태가 접수 · 조리 · 배달 · 완료로 여러 번 바뀝니다.

행 수는 그대로인데 몇 주 뒤 테이블 파일과 인덱스 크기가 몇 배로 커지고, 같은 조회가 점점 느려지는 일이 생깁니다.

원인은 PostgreSQL 이 UPDATE 마다 새 행 버전을 쓰고 옛 버전을 그 자리에 남겨 두기 때문입니다.

남은 옛 버전을 죽은 튜플(Dead Tuple)이라고 부르며, 이것이 회수되지 않고 쌓인 상태를 테이블 부풀음(Table Bloat)이라고 부릅니다.

VACUUM 은 이 죽은 튜플을 정리해 공간을 다시 쓸 수 있게 만드는 작업입니다.

## 2. 동작 원리

다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control)는 읽는 트랜잭션과 쓰는 트랜잭션이 서로를 막지 않게 하려고 행의 여러 버전을 함께 둡니다.

각 행 버전에는 그 버전을 만든 트랜잭션 ID 와 지운 트랜잭션 ID 가 기록되고, 각 트랜잭션은 자기 시점에 보이는 버전만 읽습니다.

UPDATE 는 기존 버전에 '지움' 표시를 하고 새 버전을 쓰며, DELETE 는 '지움' 표시만 합니다.

지워진 버전은 커밋 순간에 사라지지 않고, 그 버전을 볼 수 있는 트랜잭션이 하나도 남지 않을 때 비로소 죽은 튜플이 됩니다.

일반 VACUUM 은 테이블과 인덱스에서 죽은 튜플을 지우고 그 자리를 이후 쓰기에 다시 쓸 수 있다고 표시하지만, 테이블 끝 페이지가 완전히 비는 특수한 경우가 아니면 디스크 공간을 운영체제에 돌려주지 않습니다.

공식 문서 기준으로 autovacuum 은 죽은 튜플 수가 '50 + 0.2 × 테이블 행 수'(autovacuum_vacuum_threshold 기본값 50 · autovacuum_vacuum_scale_factor 기본값 0.2)를 넘으면 그 테이블을 VACUUM 하며, 워커는 기본 3개(autovacuum_max_workers)이고 데이터베이스마다 기본 1분(autovacuum_naptime) 간격으로 확인합니다.

## 3. 예시 장면

가상의 배달 앱에 주문 행이 100만 개 있다고 가정합니다(예시 수치).

기본 설정에서는 죽은 튜플이 약 20만 개를 넘어야 autovacuum 이 시작되므로, 그 사이 테이블은 최대 20% 정도 부풀어 있습니다.

저녁 피크에 상태 변경이 몰리는 동안 분석 담당자가 몇 시간짜리 리포트 쿼리를 하나의 트랜잭션으로 열어 둡니다.

그 트랜잭션이 끝나기 전에 지워진 행 버전은 그 트랜잭션에게 아직 보일 수 있으므로, autovacuum 이 돌아도 회수하지 못합니다.

리포트가 끝난 뒤 autovacuum 이 공간을 회수하지만, 파일 크기는 줄지 않고 빈 자리만 늘어난 채로 남습니다.

담당자가 크기를 줄이려고 VACUUM FULL 을 실행하면 작업 내내 테이블 전체가 ACCESS EXCLUSIVE 락에 걸려 주문 조회와 변경이 모두 멈춥니다.

## 4. 실무에서 확인할 점

pg_stat_user_tables 의 n_dead_tup · n_live_tup · last_autovacuum 을 정기적으로 조회해, 죽은 튜플이 많은데 autovacuum 이 오래 돌지 않은 테이블을 찾습니다.

pg_stat_activity 에서 age(backend_xmin) 이 큰 세션을 찾으면 회수를 막는 오래된 트랜잭션을 확인할 수 있으며, Spring 의 @Transactional 범위 안에서 외부 API 를 기다리는 코드가 흔한 원인입니다.

'idle in transaction' 상태로 커넥션 풀에 남은 세션도 같은 효과를 내므로, idle_in_transaction_session_timeout 같은 시간 제한을 검토합니다.

수백만 행 이상이고 자주 바뀌는 테이블은 기본 20% 비율이 너무 느슨할 수 있으므로, 테이블별로 scale factor 를 낮추는 것을 검토합니다.

서버 여러 대가 같은 DB 를 쓰고 정기 작업이 큰 일괄 UPDATE 를 돌린다면, 그 작업 직후 죽은 튜플 수와 autovacuum 실행 시각을 함께 확인합니다.

복제 슬롯이 오래 소비되지 않는 경우에도 회수와 동결이 밀리므로, 공식 문서는 wraparound 복구 절차에서 오래된 복제 슬롯을 확인하라고 안내합니다.

## 5. 흔한 함정

첫째, 부풀음을 없애려고 운영 시간에 VACUUM FULL 을 실행하면 ACCESS EXCLUSIVE 락 때문에 그 테이블을 쓰는 모든 요청이 멈추고, 작업 중에는 테이블 크기만큼 디스크가 더 필요합니다.

PostgreSQL 19 부터는 읽기 · 쓰기를 막지 않는 REPACK (CONCURRENTLY) 를 대안으로 검토할 수 있습니다.

둘째, 부하를 줄이려고 autovacuum 을 끄면 죽은 튜플이 계속 쌓이고, 트랜잭션 ID 가 32비트라서 오래된 행을 동결(Freezing)하지 못하면 결국 새 트랜잭션 ID 할당이 거부될 수 있습니다.

공식 문서는 autovacuum 을 꺼도 autovacuum_freeze_max_age(기본 2억 트랜잭션)를 넘으면 랩어라운드 방지용 VACUUM 이 강제로 실행된다고 설명합니다.

셋째, 행을 주기적으로 전부 지우는 테이블에 DELETE 를 쓰면 죽은 튜플만 늘어나므로, 공식 문서는 이런 경우 MVCC 의미가 달라지는 점을 감안해 TRUNCATE 를 권합니다.

## 용어 풀이

| 용어 | 뜻 |
|---|---|
| 다중 버전 동시성 제어(MVCC, Multi-Version Concurrency Control) | 행의 여러 버전을 함께 두어 읽기와 쓰기가 서로를 막지 않게 하는 방식입니다. |
| 죽은 튜플(Dead Tuple) | 지워졌고 어떤 트랜잭션에게도 더 이상 보이지 않아 회수할 수 있는 행 버전입니다. |
| 테이블 부풀음(Table Bloat) | 죽은 튜플과 회수된 빈 공간 때문에 테이블 · 인덱스 파일이 실제 데이터보다 커진 상태입니다. |
| autovacuum | 죽은 튜플 수가 기준값을 넘은 테이블을 골라 VACUUM 과 ANALYZE 를 자동으로 실행하는 PostgreSQL 의 백그라운드 프로세스입니다. |
| 동결(Freezing) | 충분히 오래된 행을 모든 트랜잭션에게 보이는 상태로 표시해 트랜잭션 ID 재사용에 대비하는 작업입니다. |
| 트랜잭션 ID 랩어라운드(Transaction ID Wraparound) | 32비트 트랜잭션 ID 가 한 바퀴 돌아 오래된 행이 미래의 것으로 잘못 보일 수 있는 상황입니다. |
| REPACK | PostgreSQL 19 에서 VACUUM FULL 과 CLUSTER 를 대체하는 명령이며, CONCURRENTLY 옵션으로 읽기 · 쓰기를 막지 않고 테이블을 다시 씁니다. |

## 더 읽을 자료

1. [PostgreSQL 문서: Routine Vacuuming](https://www.postgresql.org/docs/current/routine-vacuuming.html)
2. [PostgreSQL 문서: Vacuuming 설정(autovacuum 기본값)](https://www.postgresql.org/docs/current/runtime-config-vacuum.html)
3. [PostgreSQL 19 Released! (REPACK · autovacuum 우선순위)](https://www.postgresql.org/about/news/postgresql-19-released-3449/)
