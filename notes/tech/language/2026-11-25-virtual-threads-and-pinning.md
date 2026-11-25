# 가상 스레드(Virtual Thread)와 고정(Pinning): 적은 OS 스레드로 많은 요청을 기다리게 하기

> 분류: 기술 › 언어 · 작성: 2026-11-25 · 실린 노트: [2026-11-25](../../../daily/2026-11-25.md) · [전체 색인](../../../README.md)

**한 줄 정의**: 가상 스레드(Virtual Thread)는 JDK 가 관리하는 가벼운 스레드이며, 블로킹 대기 중에는 OS 스레드를 내려놓지만 고정(Pinning)되면 OS 스레드를 붙잡은 채 기다립니다.

## 1. 왜 필요한가

요청 하나를 스레드 하나가 처리하는 서버는 동시에 처리할 수 있는 요청 수가 스레드 수에 묶입니다.

외부 API 응답이 2초씩 늦어지면 스레드 대부분이 응답을 기다리기만 하며, CPU 는 놀고 있는데 새 요청은 대기열에서 밀립니다.

스레드를 수천 개로 늘리면 스레드마다 OS 가 스택 메모리를 잡고 문맥 전환(Context Switch) 비용도 커집니다.

비동기 · 리액티브 코드로 바꾸면 이 문제를 피할 수 있지만 코드가 콜백과 연산자로 쪼개져 읽기와 디버깅이 어려워집니다.

가상 스레드(Virtual Thread)는 블로킹 방식으로 쓴 코드를 그대로 두고 동시에 기다리는 요청 수를 늘리려고 만든 기능입니다.

## 2. 동작 원리

Java 의 기존 스레드는 플랫폼 스레드(Platform Thread)이며 OS 스레드 하나를 1:1 로 감쌉니다.

가상 스레드는 JDK 21 에서 정식 기능(JEP 444)이 되었고, OS 스레드가 아니라 JVM 힙 위의 객체로 존재합니다.

가상 스레드는 실행될 때만 캐리어 스레드(Carrier Thread)라 부르는 플랫폼 스레드 위에 올라가며, 캐리어 스레드는 기본적으로 CPU 코어 수만큼 있는 ForkJoinPool 에서 나옵니다.

가상 스레드가 소켓 읽기나 sleep 같은 블로킹 호출을 만나면 JDK 가 그 스택을 힙에 옮겨 두고 캐리어 스레드를 다른 가상 스레드에 내줍니다. 이 동작을 언마운트(Unmount)라고 합니다.

고정(Pinning)은 가상 스레드가 블로킹되어도 언마운트되지 못하고 캐리어 스레드를 붙잡은 채 기다리는 상태입니다.

JDK 21~23 에서는 synchronized 블록 안에서 블로킹하면 고정되었고, JDK 24 의 JEP 491 이 이 경우를 고쳤습니다. 네이티브 메서드나 외부 함수를 거치는 호출 중에는 지금도 고정됩니다.

## 3. 예시 장면

가상의 예약 서비스가 Spring Boot 3 과 JDK 21 위에서 가상 스레드를 켜고 운영한다고 가정합니다.

오후 7시에 할인 행사가 열려 동시 요청이 평소의 열 배로 늘어납니다(예시 수치).

요청 처리 스레드 수는 더 이상 한도가 아니므로 요청 수천 개가 한꺼번에 서비스 코드에 들어갑니다.

이 요청들이 같은 DB 커넥션 풀(Connection Pool)에서 커넥션을 얻으려 하므로 풀 크기 10개(예시 수치)를 넘는 요청은 커넥션 대기에 들어가고, 커넥션 대기 시간 초과 오류가 쏟아집니다.

같은 시각에 synchronized 로 감싼 캐시 갱신 코드가 원격 호출을 기다리며 고정되고, 캐리어 스레드 8개(예시 수치)가 모두 붙잡히면 다른 가상 스레드는 실행 차례를 얻지 못해 응답이 멈춘 것처럼 보입니다.

담당 개발자는 JFR(JDK Flight Recorder, JVM 내장 기록 도구)의 jdk.VirtualThreadPinned 이벤트로 고정 위치를 찾고, 커넥션 풀 대기 지표로 병목이 DB 커넥션이라는 것을 확인합니다.

## 4. 실무에서 확인할 점

Spring Boot 3.2 이상에서 spring.threads.virtual.enabled 를 true 로 두면 내장 웹 서버의 요청 처리와 기본 작업 실행기(applicationTaskExecutor)가 가상 스레드를 씁니다. Spring Boot 문서는 JDK 21 이상을 요구하고 JDK 24 이상을 권합니다.

가상 스레드를 켜도 HikariCP(Spring Boot 의 기본 JDBC 커넥션 풀 라이브러리)의 maximumPoolSize 는 그대로이므로, hikaricp.connections.pending 같은 커넥션 대기 지표를 함께 봅니다.

JDK 21~23 을 쓰는 서비스라면 JFR 녹화에서 jdk.VirtualThreadPinned 이벤트를 찾아 synchronized 안에서 I/O 를 기다리는 코드를 확인합니다.

외부 API 나 Redis 같은 공유 캐시를 부르는 코드는 동시 호출 수가 크게 늘 수 있으므로, 세마포어(Semaphore)나 클라이언트 쪽 커넥션 상한으로 동시 호출 수를 제한합니다.

스레드 덤프는 jcmd <pid> Thread.dump_to_file -format=json <파일> 명령으로 가상 스레드까지 포함해 받을 수 있습니다.

## 5. 흔한 함정

가상 스레드를 스레드 풀에 담아 재사용하는 경우입니다. 가상 스레드는 만들고 버리는 비용이 작도록 설계되었으므로 작업마다 새로 만들고, 동시 실행 수 제한은 풀 크기가 아니라 세마포어로 겁니다.

ThreadLocal 에 큰 객체를 캐시하는 경우입니다. 요청마다 가상 스레드가 새로 생기므로 캐시가 재사용되지 않고, 동시 가상 스레드 수만큼 메모리를 차지합니다.

CPU 를 오래 쓰는 계산 작업에 가상 스레드를 쓰는 경우입니다. 가상 스레드는 기다리는 시간을 줄이는 기능이며 계산 속도를 높이지 않으므로, 계산 작업이 캐리어 스레드를 오래 차지해 다른 요청의 응답이 늦어집니다.

## 용어 풀이

| 용어 | 뜻 |
|---|---|
| 가상 스레드(Virtual Thread) | JDK 가 스케줄링하는 가벼운 스레드이며, 블로킹 대기 중에는 OS 스레드를 내려놓습니다. |
| 플랫폼 스레드(Platform Thread) | OS 스레드 하나를 1:1 로 감싸는 Java 의 기존 스레드입니다. |
| 캐리어 스레드(Carrier Thread) | 가상 스레드를 실제로 실행하는 플랫폼 스레드이며, 기본적으로 CPU 코어 수만큼 있습니다. |
| 고정(Pinning) | 가상 스레드가 블로킹되어도 캐리어 스레드에서 내려오지 못하고 그 스레드를 붙잡은 상태입니다. |
| 문맥 전환(Context Switch) | OS 가 CPU 에서 실행할 스레드를 바꾸며 레지스터와 상태를 저장하고 복원하는 작업입니다. |
| JFR | JDK Flight Recorder 의 약자이며, JVM 안에서 이벤트를 낮은 비용으로 기록하는 내장 도구입니다. |
| HikariCP | Spring Boot 가 기본으로 쓰는 JDBC 커넥션 풀 라이브러리입니다. |
| 세마포어(Semaphore) | 동시에 들어갈 수 있는 작업 수를 허가 개수로 제한하는 동기화 도구입니다. |

## 더 읽을 자료

1. [JEP 444: Virtual Threads (OpenJDK)](https://openjdk.org/jeps/444)
2. [JEP 491: Synchronize Virtual Threads without Pinning (OpenJDK)](https://openjdk.org/jeps/491)
3. [Virtual Threads (Oracle Java SE 26 Core Libraries 문서)](https://docs.oracle.com/en/java/javase/26/core/virtual-threads.html)
4. [Spring Boot 문서: Task Execution and Scheduling](https://docs.spring.io/spring-boot/3.4/reference/features/task-execution-and-scheduling.html)
