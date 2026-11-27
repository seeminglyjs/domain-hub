# 그레이스풀 셧다운(Graceful Shutdown): 배포 중에 처리 중인 요청을 끊지 않고 서버 내리기

> 분류: 기술 › 서비스 운영 · 작성: 2026-11-27 · 실린 노트: [2026-11-27](../../../daily/2026-11-27.md) · [전체 색인](../../../README.md)

**한 줄 정의**: 서버를 내리기 전에 새 요청을 받지 않고, 이미 받은 요청은 정해진 시간 안에 끝낸 뒤 프로세스를 종료하는 방법입니다.

**그날 소식과 연결**: Mock Service Worker v3 처럼 의존 라이브러리를 올려 새로 배포할 때마다 서버 교체가 일어나므로, 교체 순간의 요청 처리 방식이 함께 중요합니다.

## 1. 왜 필요한가

서버를 여러 대 띄운 서비스는 배포할 때마다 기존 서버를 하나씩 내리고 새 서버를 올립니다.

종료 신호를 받은 서버가 곧바로 프로세스를 끝내면 그 순간 처리 중이던 요청은 응답을 받지 못하고 끊깁니다.

결제 승인이나 주문 저장처럼 DB 쓰기가 섞인 요청이라면 사용자는 오류를 보았는데 실제로는 저장이 끝난 상태가 생길 수 있습니다.

부하 분산기(Load Balancer)가 아직 그 서버로 새 요청을 보내고 있다면 연결 거부 오류도 함께 늘어납니다.

배포가 하루에 여러 번이면 이런 짧은 오류가 매번 쌓여 오류율 지표와 사용자 경험을 함께 해칩니다.

## 2. 동작 원리

그레이스풀 셧다운은 세 단계로 나뉩니다.

첫째 단계에서 서버는 새 요청을 받지 않습니다. Spring Boot 문서는 Jetty · Reactor Netty · Tomcat 이 네트워크 계층에서 새 요청을 더 받지 않는다고 설명합니다.

둘째 단계에서 이미 받은 요청은 끝까지 처리할 수 있게 둡니다. 이 대기에는 상한 시간이 있고, Spring Boot 에서는 spring.lifecycle.timeout-per-shutdown-phase 속성으로 정합니다.

셋째 단계에서 상한 시간이 지나면 남은 요청이 있어도 애플리케이션 컨텍스트를 닫고 커넥션 풀(Connection Pool) 같은 자원을 정리한 뒤 프로세스를 끝냅니다.

컨테이너 환경에서는 바깥 순서가 하나 더 있습니다. Kubernetes 는 파드(Pod)를 종료 중으로 표시하고, preStop 훅(preStop Hook, 종료 신호 전에 컨테이너 안에서 돌리는 명령)이 있으면 먼저 실행한 뒤 SIGTERM(정상 종료를 요청하는 신호)을 보냅니다.

종료 유예 시간(terminationGracePeriodSeconds)의 기본값은 30초이고, 이 시간이 지나도 남은 컨테이너에는 SIGKILL(즉시 강제 종료 신호)을 보냅니다.

## 3. 예시 장면

가상의 예약 서비스가 API 서버 세 대를 운영하고 있고, 오후 2시에 새 버전을 배포합니다.

배포 도구가 1번 서버에 종료를 요청하면, Kubernetes 는 1번 파드를 서비스 엔드포인트 목록에서 제외하는 작업과 파드 종료 작업을 같은 시점에 시작합니다.

부하 분산기가 바뀐 목록을 받기까지 수 초가 걸리므로, preStop 훅에서 예시 수치로 5초를 기다린 뒤에 SIGTERM 이 전달되게 합니다.

SIGTERM 을 받은 Spring Boot 는 새 연결을 받지 않고, 처리 중이던 예약 저장 요청 12건(예시 수치)이 끝나기를 최대 20초(예시 수치) 기다립니다.

요청이 모두 끝나면 커넥션 풀을 닫고 프로세스가 끝나며, 같은 과정이 2번과 3번 서버에서 차례로 반복됩니다.

## 4. 실무에서 확인할 점

Spring Boot 버전 문서에서 그레이스풀 셧다운이 기본으로 켜져 있는지 확인합니다. 최신 참조 문서는 세 내장 서버에서 기본으로 켜져 있고, server.shutdown=immediate 로 끌 수 있다고 적습니다.

spring.lifecycle.timeout-per-shutdown-phase 값은 가장 오래 걸리는 정상 요청의 응답 시간보다 길게 잡고, 그 값과 preStop 대기 시간을 더한 값보다 terminationGracePeriodSeconds 를 크게 잡습니다.

Actuator 의 /actuator/health/readiness 를 준비 상태 프로브(Readiness Probe)로 쓰면, 셧다운이 시작될 때 준비 상태가 REFUSING_TRAFFIC 로 바뀝니다.

정기 작업이나 메시지 소비자처럼 HTTP 요청 밖에서 도는 일은 웹 서버 셧다운이 기다려 주지 않으므로, 작업 단위를 짧게 나누거나 다시 실행해도 결과가 같게 만들었는지 확인합니다.

배포 시각 전후의 5xx 응답 수와 연결 재설정 오류 수를 부하 분산기 지표로 비교하면 셧다운 설정이 제대로 동작하는지 알 수 있습니다.

## 5. 흔한 함정

첫째, 컨테이너 시작 명령을 셸 스크립트로 감싸면 SIGTERM 이 셸에만 전달되고 JVM 에는 닿지 않을 수 있습니다. 그러면 유예 시간 내내 아무 정리도 하지 않다가 SIGKILL 로 끝납니다.

둘째, preStop 대기 없이 SIGTERM 을 바로 받으면 엔드포인트 목록이 갱신되기 전에 새 연결 받기를 멈추게 됩니다. 그 사이 들어온 요청은 연결 거부 오류를 받습니다.

셋째, 애플리케이션의 셧다운 타임아웃을 terminationGracePeriodSeconds 보다 길게 두면 정리 도중에 SIGKILL 을 받습니다. Kubernetes 문서는 preStop 훅이 유예 시간을 넘기면 2초를 한 번만 더 준다고 설명합니다.

## 용어 풀이

| 용어 | 뜻 |
|---|---|
| 그레이스풀 셧다운(Graceful Shutdown) | 새 요청을 막고 처리 중인 요청을 마친 뒤 프로세스를 끝내는 종료 방식입니다. |
| 종료 유예 시간(terminationGracePeriodSeconds) | Kubernetes 가 SIGTERM 을 보낸 뒤 SIGKILL 을 보내기까지 기다리는 최대 시간이며 기본값은 30초입니다. |
| preStop 훅(preStop Hook) | Kubernetes 가 컨테이너에 종료 신호를 보내기 전에 컨테이너 안에서 실행하는 명령이나 HTTP 호출입니다. |
| SIGTERM | 프로세스에 정상 종료를 요청하는 신호이며, 프로세스가 받아서 정리 작업을 할 수 있습니다. |
| SIGKILL | 프로세스를 즉시 강제로 끝내는 신호이며, 프로세스가 받아서 처리할 수 없습니다. |
| 준비 상태 프로브(Readiness Probe) | 컨테이너가 지금 요청을 받을 수 있는지 Kubernetes 가 주기적으로 묻는 검사입니다. |
| 엔드포인트 슬라이스(EndpointSlice) | Kubernetes 서비스가 요청을 보낼 파드 주소 목록을 담는 객체입니다. |

## 더 읽을 자료

1. [Spring Boot 참조 문서: Graceful Shutdown](https://docs.spring.io/spring-boot/reference/web/graceful-shutdown.html)
2. [Kubernetes 문서: Termination of Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod-termination/)
3. [Spring Boot 참조 문서: Actuator Endpoints(Kubernetes Probes)](https://docs.spring.io/spring-boot/reference/actuator/endpoints.html)
