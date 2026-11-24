# 컨테이너 메모리 제한(Container Memory Limit)과 JVM: 힙 밖 메모리(Off-Heap Memory)까지 계산해야 OOM Kill 을 피한다

> 분류: 기술 › 시스템 · 작성: 2026-11-24 · 실린 노트: [2026-11-24](../../../daily/2026-11-24.md) · [전체 색인](../../../README.md)

**한 줄 정의**: 컨테이너는 JVM 프로세스 전체가 쓰는 메모리로 종료 여부를 판단하므로, 힙 크기는 스레드 스택 · Metaspace · 코드 캐시 · 직접 버퍼를 제외한 나머지로 정해야 합니다.

**그날 소식과 연결**: JDK 28 의 JEP 545 적응형 힙 옵션은 힙 상한을 물리 메모리 100% 까지 올리므로, 컨테이너 메모리 제한과 힙 밖 메모리를 함께 계산해야 하는 이유와 이어집니다.

## 1. 왜 필요한가

메모리 제한이 2GB 인 컨테이너에서 Spring Boot 서버를 띄우고 힙 사용량 그래프를 보면 1GB 근처에 머물러 있는 경우가 있습니다.

그런데도 트래픽이 몰리는 시간에 컨테이너가 로그 한 줄 없이 사라지고, 오케스트레이터에는 종료 사유가 OOMKilled 로 남습니다.

이 일은 JVM 이 힙이 아니라 프로세스 전체 메모리로 판단되기 때문에 생깁니다.

리눅스 커널은 컨테이너의 cgroup(Control Group, 프로세스 묶음별 자원 제한 기능) 메모리 사용량이 제한을 넘으면 그 안의 프로세스를 강제로 종료합니다.

힙 바깥에서 쓰는 메모리를 계산에 넣지 않으면 힙에 여유가 있어도 프로세스는 종료됩니다.

## 2. 동작 원리

JVM 프로세스의 메모리는 크게 힙(Heap)과 힙 밖 메모리로 나뉩니다.

힙 밖 메모리에는 클래스 정보를 담는 Metaspace, JIT(Just-In-Time) 컴파일러가 만든 기계어를 담는 코드 캐시(Code Cache), 스레드마다 하나씩 잡히는 스레드 스택(Thread Stack), NIO 직접 버퍼(Direct Buffer), GC 가 쓰는 내부 자료 구조가 들어갑니다.

-Xmx 나 -XX:MaxRAMPercentage 는 힙 상한만 정하며, 위 영역의 합계를 제한하지 않습니다.

JDK 문서 기준으로 MaxRAMPercentage 기본값은 25% 이고, 컨테이너 제한을 인식하는 JVM 은 이 비율을 컨테이너 메모리 제한에 곱해 최대 힙을 정합니다.

예시 수치로 제한 2GB 에서 비율을 90% 로 올리면 힙 상한은 약 1.8GB 가 됩니다. 이 경우 Linux x64 기본 스레드 스택 1MB 를 쓰는 스레드 250개와 코드 캐시 기본 상한 240MB 만 더해도 이론상 합계가 제한을 넘을 수 있습니다.

Paketo Java 빌드팩의 메모리 계산기는 이 관계를 '힙 = 컨테이너 전체 메모리 - (직접 메모리 + Metaspace + 코드 캐시 + 스레드 스택 × 스레드 수) - 여유분' 으로 계산합니다.

## 3. 예시 장면

가상의 예약 서비스가 서버 컨테이너마다 메모리 제한 2GB 를 주고 MaxRAMPercentage 를 90% 로 설정했습니다.

평일 낮에는 요청 스레드가 수십 개만 돌아서 프로세스 전체 사용량이 1.6GB 안팎에 머뭅니다.

휴가철 예약 오픈 시각에 요청이 몰리자 Tomcat 작업 스레드가 200개 가까이 늘고, 외부 결제 API 호출용 HTTP 클라이언트가 직접 버퍼를 더 잡습니다.

힙은 아직 1.2GB 인데 프로세스 전체 사용량이 2GB 를 넘는 순간 커널이 프로세스를 종료하고, 컨테이너가 다시 뜨는 동안 남은 서버로 요청이 몰립니다.

힙 덤프는 남지 않습니다. JVM 의 OutOfMemoryError 가 아니라 커널의 강제 종료여서 -XX:+HeapDumpOnOutOfMemoryError 가 동작할 기회가 없기 때문입니다.

## 4. 실무에서 확인할 점

Kubernetes 에서는 kubectl describe pod 결과의 Last State 에서 Reason 이 OOMKilled 이고 종료 코드가 137 인지 확인합니다. 이 조합이면 JVM 이 아니라 커널이 프로세스를 종료한 것입니다.

컨테이너 안에서 cgroup v2 의 memory.current 와 memory.max 값을 읽어 커널이 보는 사용량과 제한을 직접 비교합니다.

JVM 쪽에서는 -XX:NativeMemoryTracking=summary 로 띄운 뒤 jcmd <pid> VM.native_memory 명령으로 영역별 예약량과 사용량을 봅니다. Oracle 문서는 NMT 를 켜면 성능이 약 5~10% 떨어진다고 적고 있어 진단할 때만 켭니다.

Spring Boot Actuator 를 쓴다면 jvm.memory.used 를 area 태그로 heap 과 nonheap 을 나눠 보고, jvm.buffer.memory.used 로 직접 버퍼 사용량을 함께 봅니다.

힙 비율을 정할 때는 server.tomcat.threads.max 같은 최대 스레드 수와 커넥션 풀 크기를 함께 놓고, 스레드 스택 합계와 여유분을 먼저 제외한 나머지를 힙에 줍니다.

## 5. 흔한 함정

첫째, 힙 사용량 그래프만 보고 메모리가 남는다고 판단합니다. 그 결과 컨테이너 제한을 줄이거나 힙 비율을 올려 강제 종료를 더 자주 부릅니다.

둘째, 제한을 인식하지 못하는 오래된 JVM 이나 컨테이너 밖 기준으로 힙을 고정한 설정을 그대로 씁니다. 이 경우 힙 상한이 호스트 메모리 기준으로 잡혀 제한보다 커질 수 있습니다.

셋째, 컨테이너가 종료될 때 힙 덤프를 기다립니다. 커널의 강제 종료는 JVM 에 알리지 않으므로 덤프와 마지막 로그가 남지 않고, 원인을 찾으려면 cgroup 지표와 NMT 기록을 미리 모아 두어야 합니다.

## 용어 풀이

| 용어 | 뜻 |
|---|---|
| 컨테이너 메모리 제한(Container Memory Limit) | 컨테이너 하나가 쓸 수 있는 메모리 상한이며, 넘으면 커널이 그 안의 프로세스를 강제로 종료합니다. |
| 힙 밖 메모리(Off-Heap Memory) | JVM 프로세스가 힙 바깥에서 쓰는 메모리로 Metaspace · 코드 캐시 · 스레드 스택 · 직접 버퍼 등을 포함합니다. |
| 메타스페이스(Metaspace) | JVM 이 불러온 클래스의 구조 정보를 힙 바깥 네이티브 메모리에 담아 두는 영역입니다. |
| 코드 캐시(Code Cache) | JIT 컴파일러가 바이트코드를 기계어로 바꾼 결과를 담는 영역이며 JDK 21 기본 상한은 240MB 입니다. |
| 직접 버퍼(Direct Buffer) | NIO 가 입출력 복사를 줄이려고 힙 바깥에 잡는 메모리 버퍼입니다. |
| 네이티브 메모리 추적(NMT, Native Memory Tracking) | JVM 이 내부 영역별로 쓰는 네이티브 메모리를 집계해 jcmd 로 보여 주는 진단 기능입니다. |
| cgroup(Control Group) | 리눅스 커널이 프로세스 묶음마다 CPU · 메모리 같은 자원 사용량을 제한하고 집계하는 기능입니다. |
| Paketo | 소스 코드에서 컨테이너 이미지를 만드는 Cloud Native Buildpacks 구현이며 Java 이미지에 메모리 계산기를 넣습니다. |

## 더 읽을 자료

1. [Oracle JDK 21 java 명령 문서(-XX:MaxRAMPercentage · -XX:ReservedCodeCacheSize)](https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html)
2. [Oracle JDK 21 문제 해결 가이드: Diagnostic Tools(Native Memory Tracking)](https://docs.oracle.com/en/java/javase/21/troubleshoot/diagnostic-tools.html)
3. [Paketo Java Buildpack Reference: Memory Calculator](https://paketo.io/docs/reference/java-reference/)
4. [Red Hat Developer: Overhauling memory tuning in OpenJDK containers updates](https://developers.redhat.com/articles/2023/03/07/overhauling-memory-tuning-openjdk-containers-updates)
5. [Kubernetes 문서: Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
