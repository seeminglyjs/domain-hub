# 요청 끝까지 열린 영속성 컨텍스트(OSIV, Open Session in View): Spring 이 DB 커넥션을 오래 잡는 이유

> 분류: 기술 › 프레임워크 · 작성: 2026-11-23 · 실린 노트: [2026-11-23](../../../daily/2026-11-23.md) · [전체 색인](../../../README.md)

**한 줄 정의**: OSIV 는 HTTP 요청 처리 전체 동안 JPA 영속성 컨텍스트를 열어 두는 방식이며, 그만큼 DB 커넥션을 오래 붙잡아 커넥션 풀을 빨리 고갈시킬 수 있습니다.

## 1. 왜 필요한가

주문 목록 화면을 그리는 도중에 연관된 상품 정보를 읽어야 하는 상황을 생각해 봅니다.

서비스 메서드의 트랜잭션은 이미 끝났는데 화면이나 JSON 변환 단계에서 아직 읽지 않은 연관 데이터를 건드리면 지연 로딩(Lazy Loading)이 실패합니다.

이 실패를 피하려고 요청이 끝날 때까지 영속성 컨텍스트(Persistence Context)를 열어 두는 방식이 OSIV 입니다.

편리한 대신 DB 커넥션을 쥐고 있는 시간이 트랜잭션 길이가 아니라 요청 길이로 늘어납니다.

요청 처리 시간이 길어지는 순간 커넥션 풀(Connection Pool)이 바닥나고 다른 요청이 커넥션을 기다리다 실패합니다.

## 2. 동작 원리

첫째, 요청이 들어오면 인터셉터가 JPA EntityManager 를 만들어 요청을 처리하는 스레드에 묶습니다.

둘째, 서비스 계층의 트랜잭션은 새 EntityManager 를 만들지 않고 스레드에 묶인 것을 찾아 씁니다.

셋째, 트랜잭션이 커밋된 뒤에도 EntityManager 는 닫히지 않으므로 컨트롤러와 뷰 단계에서 지연 로딩 쿼리가 더 나갈 수 있습니다.

넷째, 요청 응답이 끝나야 EntityManager 가 닫히고, Vlad Mihalcea 의 분석에 따르면 그때까지 DB 커넥션도 뷰 렌더링 내내 붙잡혀 있습니다.

Spring Boot 는 웹 애플리케이션에서 이 인터셉터(OpenEntityManagerInViewInterceptor)를 기본으로 등록하고, spring.jpa.open-in-view 속성으로 켜고 끕니다.

## 3. 예시 장면

가상의 예약 서비스에서 예약 상세 API 가 예약 한 건을 읽은 뒤 외부 결제사 API 를 불러 결제 상태를 붙인다고 가정합니다.

예시 수치로 DB 조회는 20ms, 외부 결제사 응답은 평소 200ms 이고 서버 한 대의 커넥션 풀은 10개입니다.

OSIV 가 켜져 있으면 DB 조회가 끝난 뒤에도 커넥션이 외부 호출 200ms 동안 함께 붙잡혀 있습니다.

어느 저녁 결제사 응답이 3초로 늘어나자 커넥션 10개가 모두 외부 호출을 기다리는 요청에 묶입니다.

DB 는 한가한데도 다른 API 요청이 커넥션을 받지 못해 대기 시간 초과로 실패하고, 장애 화면에는 DB 문제처럼 보입니다.

## 4. 실무에서 확인할 점

Spring Boot 애플리케이션 설정에 spring.jpa.open-in-view 값을 명시했는지 확인합니다. 값을 정하지 않으면 기본값 true 가 적용되고 시작할 때 경고 로그가 남습니다.

HikariCP 의 대기 중 스레드 수와 커넥션 획득 대기 시간 지표(Actuator 의 hikaricp 지표)가 DB 의 실제 쿼리 시간보다 크게 튀는지 봅니다.

외부 HTTP 호출 · 파일 업로드 · 긴 JSON 변환처럼 DB 와 상관없는 작업이 커넥션을 쥔 채 실행되는 API 가 있는지 찾습니다.

서버를 여러 대 띄우면 서버 수 곱하기 풀 크기가 PostgreSQL 의 max_connections 안에 들어가야 하므로, 풀을 키우기 전에 커넥션 점유 시간부터 줄입니다.

OSIV 를 끄기 전에는 서비스 계층 안에서 필요한 연관 데이터를 미리 읽거나 DTO 로 바꿔 내보내는지 검사로 확인합니다.

```yaml
spring:
  jpa:
    open-in-view: false
  datasource:
    hikari:
      connection-timeout: 30000
      leak-detection-threshold: 2000
```

*Spring Boot 속성 목록의 spring.jpa.open-in-view 와 spring.datasource.hikari.* 바인딩, HikariCP README 의 connectionTimeout(기본 30000ms) · leakDetectionThreshold(기본 0, 최소 2000ms) 설명을 줄여 옮긴 것입니다.*

## 5. 흔한 함정

경고 로그만 없애려고 open-in-view 를 true 로 명시하면 커넥션 점유 문제는 그대로 남습니다.

준비 없이 false 로 바꾸면 컨트롤러나 JSON 변환 단계에서 LazyInitializationException 이 터지므로, 운영에 올리기 전에 주요 API 를 검사로 돌려야 합니다.

OSIV 가 켜진 채로 뷰나 JSON 변환에서 연관 데이터를 읽으면 목록 한 줄마다 쿼리가 나가는 N+1 문제(N+1 Query Problem)가 눈에 띄지 않게 생깁니다.

트랜잭션 밖에서 나가는 이 쿼리들은 자동 커밋(Auto-commit) 모드로 실행되므로 한 화면의 값이 서로 다른 시점의 데이터일 수 있습니다.

## 용어 풀이

| 용어 | 뜻 |
|---|---|
| 영속성 컨텍스트(Persistence Context) | JPA 가 읽어 온 엔티티를 기억하고 변경을 추적하는 작업 공간입니다. |
| 지연 로딩(Lazy Loading) | 연관된 데이터를 처음 실제로 쓰는 순간에 쿼리를 보내 읽는 방식입니다. |
| 커넥션 풀(Connection Pool) | DB 연결을 미리 만들어 두고 요청마다 빌려주고 돌려받는 장치입니다. |
| N+1 문제(N+1 Query Problem) | 목록 쿼리 한 번 뒤에 항목마다 추가 쿼리가 한 번씩 더 나가는 현상입니다. |
| 자동 커밋(Auto-commit) | 트랜잭션으로 묶지 않은 SQL 문장을 하나씩 바로 확정하는 실행 모드입니다. |
| HikariCP | Spring Boot 가 기본으로 쓰는 JDBC 커넥션 풀 라이브러리입니다. |
| DTO 프로젝션(DTO Projection) | 엔티티 전체 대신 화면에 필요한 열만 골라 전용 객체로 읽는 조회 방식입니다. |

## 더 읽을 자료

1. [Spring Boot 레퍼런스: SQL Databases(JPA · Open EntityManager in View)](https://docs.spring.io/spring-boot/reference/data/sql.html)
2. [Spring Framework Javadoc: OpenEntityManagerInViewInterceptor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/orm/jpa/support/OpenEntityManagerInViewInterceptor.html)
3. [Vlad Mihalcea: The Open Session In View Anti-Pattern](https://vladmihalcea.com/the-open-session-in-view-anti-pattern/)
4. [Spring Boot 이슈 #47547: 4.0 에서 open-in-view 기본값 재검토(보류로 닫힘)](https://github.com/spring-projects/spring-boot/issues/47547)
5. [HikariCP README(설정 항목)](https://github.com/brettwooldridge/HikariCP)
