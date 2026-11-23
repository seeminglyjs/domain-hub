# 작업 스레드 풀과 연결 대기열(Worker Thread Pool · Accept Queue): Tomcat 이 요청을 받아 처리하기까지

> 분류: 기술 › 서버 · 작성: 2026-11-19 · 실린 노트: [2026-11-19](../../../daily/2026-11-19.md) · [전체 색인](../../../README.md)

**한 줄 정의**: 웹 서버가 동시에 처리할 요청 수(작업 스레드)와 기다리게 할 연결 수(연결 대기열)를 어떻게 나누어 정하는지 설명합니다.

**그날 소식과 연결**: Java4Shell 대응처럼 JVM 을 재시작하는 순간에도 이 대기열에 쌓인 연결이 어떻게 처리되는지가 사용자 체감 지연을 좌우합니다.

## 1. 왜 필요한가

예약 서비스가 오전 10시에 예약을 열면 수천 개의 요청이 1초 안에 동시에 도착합니다.

서버는 요청마다 DB 조회와 외부 API 호출을 기다리는 동안 처리 자원을 붙잡고 있습니다.

처리 자원이 모두 차면 새 요청은 어딘가에서 기다려야 하고, 그 자리가 어디인지 모르면 지연과 연결 실패의 원인을 찾지 못합니다.

이때 서버 CPU 사용률은 낮은데도 응답 시간만 길어지는 현상이 자주 나타납니다.

그래서 동시에 처리할 요청 수와 기다리게 할 연결 수를 따로 정하고, 각 단계에 몇 건이 쌓였는지 볼 수 있어야 합니다.

## 2. 동작 원리

Spring Boot 의 기본 웹 서버인 Tomcat 은 요청 하나가 들어와 처리되기까지 세 단계의 대기 자리를 거칩니다.

첫째 단계에서 운영체제는 TCP 연결 수립(3-way handshake)이 끝난 연결을 연결 대기열(Accept Queue)에 둡니다. 이 길이는 Tomcat 의 acceptCount(기본 100)로 요청하지만, Linux 에서는 net.core.somaxconn 값보다 크게 잡을 수 없습니다.

둘째 단계에서 Tomcat 은 maxConnections(기본 8192)개까지 연결을 받아들이고, 폴러(Poller) 스레드가 이 연결들을 지켜보다가 요청 데이터가 들어온 연결만 작업 스레드에 넘깁니다.

셋째 단계에서 작업 스레드(기본 최대 200개, 최소 10개 유지)가 요청을 처리하며, 비동기가 아닌 요청은 응답을 다 쓸 때까지 스레드 하나를 계속 점유합니다.

작업 스레드가 모두 바쁘면 연결은 Tomcat 안에서 기다리고, 연결 수가 maxConnections 에 닿으면 운영체제 대기열에 쌓이며, 그 대기열도 가득 차면 새 연결은 거절되거나 시간 초과됩니다.

예시 수치로 요청 하나의 평균 처리 시간이 0.2초이고 작업 스레드가 200개이면, 서버 한 대가 감당하는 처리량은 초당 약 1,000건입니다(리틀의 법칙: 동시 처리 수 = 처리량 × 처리 시간).

## 3. 예시 장면

가상의 예약 서비스가 서버 세 대에서 Tomcat 기본 설정으로 동작한다고 가정합니다(아래 수치는 모두 예시 수치입니다).

10:00:00 에 예약이 열리고 서버 한 대에 초당 약 800건의 요청이 들어옵니다. 요청 하나가 0.2초 걸리므로 작업 스레드 약 160개가 바쁘게 돌아가고 응답은 정상입니다.

10:00:30 에 외부 결제 API 응답이 2초로 늘어나면서 요청 하나의 처리 시간이 약 2초가 됩니다. 같은 처리량을 내려면 스레드 약 1,600개가 필요하므로 작업 스레드 200개가 금방 모두 점유됩니다.

10:00:40 부터 새 연결은 받아들여지지만 처리되지 않고 Tomcat 안에서 기다리며, 사용자는 화면이 멈춘 것처럼 느낍니다.

같은 포트로 들어오는 로드 밸런서의 상태 확인(Health Check) 요청도 작업 스레드를 기다리다 시간 초과되어, 로드 밸런서가 이 서버를 제외하고 남은 두 대로 요청을 몰아 장애가 번집니다.

원인은 스레드 개수가 아니라 외부 호출에 시간 제한(Timeout)이 없어서 스레드가 오래 붙잡힌 것이며, 결제 호출에 시간 제한을 걸고 실패를 빠르게 돌려주는 것이 먼저입니다.

## 4. 실무에서 확인할 점

Spring Boot 에서는 server.tomcat.threads.max · server.tomcat.threads.min-spare · server.tomcat.accept-count · server.tomcat.max-connections 로 세 단계의 크기를 정합니다.

Actuator 의 tomcat.threads.busy · tomcat.threads.config.max 지표를 비교하면 작업 스레드가 얼마나 찼는지 볼 수 있으며, 이 Tomcat 지표는 server.tomcat.mbeanregistry.enabled=true 일 때 수집됩니다.

작업 스레드 수를 DB 커넥션 풀 크기와 함께 봅니다. 스레드가 200개이고 HikariCP 풀이 10개이면 나머지 스레드는 커넥션을 기다리므로 hikaricp.connections.pending 지표가 함께 올라갑니다.

Linux 서버나 컨테이너에서는 ss -lnt 로 듣기 소켓의 Send-Q(대기열 최대 길이)와 Recv-Q(지금 쌓인 연결 수)를, nstat 의 ListenOverflows 로 대기열이 넘친 횟수를 확인합니다.

서버를 여러 대 띄우는 서비스라면 상태 확인 요청이 작업 스레드를 기다리지 않도록 관리용 포트(management.server.port)를 따로 두는지 검토합니다.

## 5. 흔한 함정

작업 스레드 수만 크게 올리는 경우입니다. DB 커넥션 풀이 그대로이면 대기 자리가 스레드에서 커넥션 풀로 옮겨 갈 뿐이고, 스레드마다 스택 메모리와 문맥 교환(Context Switch) 비용이 늘어 컨테이너 메모리 제한에 먼저 닿을 수 있습니다.

acceptCount 만 크게 올리고 효과가 있다고 믿는 경우입니다. Linux 는 listen 대기열 길이를 somaxconn 값으로 조용히 잘라 내므로(커널 5.4 이후 기본 4096, 그 전에는 128) 운영체제 설정을 함께 확인해야 합니다.

가상 스레드(spring.threads.virtual.enabled=true)를 켠 뒤에도 server.tomcat.threads.max 로 동시 처리 수를 제한한다고 믿는 경우입니다. Spring Boot 문서는 가상 스레드를 켜면 이 설정이 효과가 없다고 밝히므로, 동시 요청이 그대로 DB 커넥션 풀로 몰리지 않도록 별도의 동시 실행 제한을 둡니다.

```yaml
server:
  tomcat:
    threads:
      max: 200        # 작업 스레드 최대 수
      min-spare: 10   # 늘 유지하는 최소 스레드 수
    accept-count: 100       # 모든 스레드가 바쁠 때 운영체제 연결 대기열 길이
    max-connections: 8192   # 동시에 받아들여 처리하는 최대 연결 수
```

*Spring Boot 공통 애플리케이션 속성(Common Application Properties) 문서의 Server Properties 절에 있는 Tomcat 설정 키와 기본값을 옮긴 것입니다.*

## 용어 풀이

| 용어 | 뜻 |
|---|---|
| 작업 스레드 풀(Worker Thread Pool) | 요청을 실제로 처리하는 스레드를 미리 만들어 두고 돌려 쓰는 묶음입니다. |
| 연결 대기열(Accept Queue) | 운영체제가 연결 수립을 끝냈지만 서버 프로그램이 아직 받아 가지 않은 연결을 쌓아 두는 자리입니다. |
| Tomcat | Spring Boot 웹 애플리케이션이 기본으로 내장하는 Java 웹 서버이자 서블릿 컨테이너입니다. |
| 폴러(Poller) | Tomcat NIO 커넥터에서 여러 연결을 지켜보다가 데이터가 도착한 연결만 작업 스레드에 넘기는 스레드입니다. |
| somaxconn | Linux 에서 listen 대기열 길이의 상한을 정하는 커널 설정 값(net.core.somaxconn)입니다. |
| 리틀의 법칙(Little's Law) | 시스템 안의 평균 요청 수는 평균 도착률과 평균 체류 시간의 곱과 같다는 대기 이론의 관계식입니다. |
| 가상 스레드(Virtual Thread) | Java 21 부터 정식 기능이 된, 운영체제 스레드보다 훨씬 가볍게 많이 만들 수 있는 JVM 관리 스레드입니다. |

## 더 읽을 자료

1. [Apache Tomcat 11 HTTP Connector 설정 문서](https://tomcat.apache.org/tomcat-11.0-doc/config/http.html)
2. [Spring Boot 공통 애플리케이션 속성 문서](https://docs.spring.io/spring-boot/appendix/application-properties/index.html)
3. [Linux man page listen(2)](https://man7.org/linux/man-pages/man2/listen.2.html)
