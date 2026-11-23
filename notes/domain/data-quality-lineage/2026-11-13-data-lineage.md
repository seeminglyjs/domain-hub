# 데이터 계보(Data Lineage) — 숫자가 어디서 왔는지 거슬러 찾기

> 분류: 도메인 › 데이터 품질·계보 · 작성: 2026-11-13 · 실린 노트: [2026-11-13](../../../daily/2026-11-13.md) · [전체 색인](../../../README.md)

**한 줄 정의**: 데이터가 어느 원천에서 출발해 어떤 작업과 변환을 거쳐 지금의 테이블 · 컬럼 · 화면 숫자가 되었는지 기록한 연결 정보로, 숫자의 원인을 거슬러 찾거나 변경의 영향을 미리 알아보는 데 씁니다.

**그날 소식과 연결**: 이번 회차 Constellation 사례는 Unity Catalog 의 계보 기능을 공용 데이터 기반의 한 축으로 두었으며, 이 글은 그 계보가 무엇을 기록하고 어디까지 믿을 수 있는지 다룹니다.

## 1. 왜 필요한가

월요일 아침 관리 화면의 어제 주문 금액이 평소의 절반으로 표시된다고 가정합니다. 담당자는 이 숫자가 실제 판매 감소인지, 중간 집계 작업이 일부만 돈 결과인지, 원천 테이블의 컬럼 의미가 바뀐 결과인지 가려야 합니다. 그런데 이 숫자를 만드는 테이블이 어떤 테이블을 읽고, 그 테이블은 또 어떤 정기 작업이 채우는지 적힌 곳이 없으면 코드를 하나씩 열어 거꾸로 따라가야 합니다.

반대 방향의 질문도 같습니다. 원천 테이블의 컬럼 하나를 지우려는 개발자는 그 컬럼을 읽는 집계 · 화면 · 외부 내보내기가 어디인지 먼저 알아야 합니다. 데이터 계보(Data Lineage)는 '이 숫자는 어디서 왔는가'와 '이것을 바꾸면 어디가 영향을 받는가'라는 두 질문에 기록으로 답하려는 개념입니다.

## 2. 동작 원리

계보는 데이터셋(Dataset, 테이블이나 파일처럼 읽고 쓰는 데이터 묶음) · 작업(Job, 데이터를 읽어 다른 데이터를 만드는 처리) · 실행(Run, 작업이 한 번 돈 기록) 세 요소로 이루어진 그래프입니다. OpenLineage(데이터 계보 정보를 주고받는 형식을 정한 오픈 표준)는 이 세 요소를 기본 모델로 두고, 나머지 정보는 패싯(Facet, 기본 요소에 붙이는 추가 메타데이터 조각)으로 덧붙입니다. OpenLineage 는 LF AI & Data 재단(Linux Foundation 산하의 AI · 데이터 오픈소스 재단)의 졸업 프로젝트입니다.

기록은 작업이 도는 순간에 남깁니다. OpenLineage 명세는 실행마다 START · RUNNING · COMPLETE · ABORT · FAIL · OTHER 상태의 이벤트를 보내게 하고, 각 이벤트에 작업 · 실행 · 입력 데이터셋 · 출력 데이터셋 정보를 담습니다. COMPLETE · ABORT · FAIL 은 마지막 상태여서, 그 뒤에는 같은 실행의 이벤트를 보내지 않습니다.

계보의 세밀함은 두 단계로 나뉩니다. 테이블 수준 계보는 어느 테이블이 어느 테이블을 읽는지를 기록하고, 컬럼 수준 계보(Column-level Lineage)는 출력 컬럼 하나가 어느 입력 컬럼에서 왔는지를 기록합니다. OpenLineage 의 컬럼 계보 패싯은 값을 그대로 옮기거나 계산한 관계를 DIRECT 로, 조인 · 필터 · 정렬처럼 값에 영향만 준 관계를 INDIRECT 로 나눠 적습니다.

기록을 모으는 방법은 크게 둘입니다. 하나는 Databricks(데이터 · AI 분석 플랫폼 회사)의 Unity Catalog(Databricks 의 데이터 · 권한 · 계보 관리 기능)처럼 쿼리를 실행하는 엔진이 자동으로 남기는 방식이고, 다른 하나는 작업 코드나 스케줄러가 이벤트를 직접 보내는 방식입니다. Marquez(LF AI & Data 재단의 메타데이터 수집 · 시각화 프로젝트)는 OpenLineage 이벤트를 받는 참조 구현(Reference Implementation, 표준을 그대로 따른 기준 구현)입니다.

## 3. 예시 장면

아래는 설명을 위한 가상 장면이며, 시각과 수치는 모두 예시 수치입니다. 숙소 예약 서비스는 매일 03:00 에 원천 테이블 reservations 를 읽어 daily_bookings 를 만드는 작업 A 와, 04:00 에 daily_bookings 를 읽어 지역별 요약 region_summary 를 만드는 작업 B 를 돌립니다.

어느 날 09:00 관리 화면의 지역별 예약 금액이 전날의 절반으로 표시됩니다. 담당자는 계보 화면에서 region_summary 의 total_amount 컬럼을 고르고, 이 컬럼이 daily_bookings 의 amount 를 거쳐 reservations 의 price 에서 왔다는 것을 확인합니다.

같은 화면에서 작업 A 의 03:00 실행은 FAIL 로, 작업 B 의 04:00 실행은 COMPLETE 로 남아 있습니다. 작업 B 가 일부만 채워진 daily_bookings 를 읽었다는 것을 바로 알 수 있으므로, 담당자는 작업 A 를 다시 돌린 뒤 작업 B 를 다시 돌립니다.

다음 주에 다른 개발자가 reservations.price 의 자료형을 바꾸려 할 때는 같은 계보를 반대 방향으로 따라갑니다. 그 컬럼을 읽는 작업 두 개와 관리 화면 한 곳이 보이므로, 개발자는 변경 전에 그 담당자들과 일정을 맞춥니다.

## 4. 실무에서 확인할 점

PostgreSQL 뷰(View, 저장해 둔 조회문)로 지표를 만드는 서비스라면, information_schema 의 view_column_usage 로 어떤 뷰가 어떤 테이블의 어떤 컬럼을 읽는지 조회할 수 있습니다. PostgreSQL 문서는 이 뷰가 현재 활성화된 역할이 소유한 테이블의 컬럼만 보여 준다고 적으므로, 조회하는 계정의 권한을 먼저 확인합니다. 이 정보는 DB 안의 뷰 정의에 한정되고, 애플리케이션 코드나 정기 작업이 읽고 쓰는 관계는 담지 않습니다.

Spring 정기 작업이 테이블을 읽어 집계 테이블을 채우는 구조라면, 작업마다 읽은 테이블 · 쓴 테이블 · 시작과 끝 시각 · 성공 여부를 남기는 실행 기록 테이블을 두는 것만으로 테이블 수준 계보의 기본을 얻습니다. 서버를 여러 대 띄운다면 이 기록을 서버 메모리나 로컬 로그가 아니라 공유 DB 에 남겨야, 어느 서버가 돌렸든 한 곳에서 볼 수 있습니다.

이 기록의 모양을 OpenLineage 의 실행 이벤트(작업 · 실행 · 입력 · 출력 · 상태)에 맞춰 두면, 나중에 Marquez 같은 수집 도구로 옮기기 쉽습니다. 화면 숫자에 기준 시각을 함께 보이는 서비스라면, 그 시각이 원천 작업의 마지막 COMPLETE 실행 시각과 맞는지도 확인합니다.

```json
"columnLineage": {
  "fields": {
    "order_delivery_time": {
      "inputFields": [
        {"namespace": "food_delivery", "name": "public.delivery_7_days",
         "field": "order_placed_on",
         "transformations": [{"type": "DIRECT", "subtype": "TRANSFORMATION"}]},
        {"namespace": "food_delivery", "name": "public.delivery_7_days",
         "field": "order_delivered_on",
         "transformations": [{"type": "DIRECT", "subtype": "TRANSFORMATION"}]}
      ]
    }
  },
  "dataset": [
    {"namespace": "food_delivery", "name": "public.delivery_7_days",
     "field": "order_delivery_time",
     "transformations": [{"type": "INDIRECT", "subtype": "SORT"}]}
  ]
}
```

*OpenLineage 문서 'Column Level Lineage Dataset Facet' 의 배달 시간 예시를 줄여 옮겼습니다(_producer · _schemaURL · description · masking 키는 생략했습니다).*

## 5. 흔한 함정

첫째, 계보를 문서나 그림으로 손으로 그려 두는 경우입니다. 코드가 바뀔 때마다 그림을 고치는 사람이 없으므로, 몇 달 뒤에는 실제 흐름과 다른 그림을 믿고 판단하게 됩니다.

둘째, 자동 수집 계보가 모든 관계를 담는다고 믿는 경우입니다. Databricks 문서는 Unity Catalog 계보가 UDF(User-Defined Function, 사용자가 만든 함수) 안에서 참조한 테이블이나 함수를 기록하지 않고, 이름을 바꾼 테이블 · 컬럼은 이전 계보 기록을 잃는다고 적습니다. 문서는 트랜잭션이 롤백되어도 이미 남은 계보는 그대로 남는다고도 밝힙니다.

셋째, 컬럼 계보를 너무 거칠게 남기는 경우입니다. OpenLineage 문서는 Spark(대용량 데이터를 여러 서버에서 나눠 처리하는 오픈소스 엔진)의 기본 설정이 출력 컬럼마다 모든 입력 컬럼에 INDIRECT 연결을 붙여, 거의 모든 쌍이 이어진 비효율적인 모양이 된다고 설명하고 dataset 목록을 쓰는 압축 모양을 권장합니다. 모든 컬럼이 서로 이어진 계보로는 원인을 좁힐 수 없습니다.

## 용어 풀이

| 용어 | 뜻 |
|---|---|
| 데이터 계보(Data Lineage) | 데이터가 어느 원천에서 어떤 작업을 거쳐 지금의 테이블 · 컬럼이 되었는지 기록한 연결 정보입니다. |
| 데이터셋(Dataset) | 테이블이나 파일처럼 작업이 읽고 쓰는 데이터 묶음입니다. |
| 실행(Run) | 작업이 한 번 돈 기록으로, 시작 · 완료 · 실패 같은 상태를 가집니다. |
| 패싯(Facet) | OpenLineage 에서 데이터셋 · 작업 · 실행에 붙이는 추가 메타데이터 조각입니다. |
| 컬럼 수준 계보(Column-level Lineage) | 출력 컬럼 하나가 어느 입력 컬럼에서 어떤 변환을 거쳐 왔는지까지 기록한 계보입니다. |
| OpenLineage | 작업 실행 중에 계보 정보를 주고받는 형식을 정한 오픈 표준으로, LF AI & Data 재단의 졸업 프로젝트입니다. |
| Unity Catalog | Databricks 의 데이터 · 권한 · 계보 관리 기능으로, Databricks 에서 실행한 쿼리의 계보를 자동으로 남깁니다. |
| Marquez | OpenLineage 이벤트를 받아 메타데이터를 모으고 보여 주는 LF AI & Data 재단의 참조 구현입니다. |

## 더 읽을 자료

1. [OpenLineage 문서: 개요](https://openlineage.io/docs/)
2. [OpenLineage 문서: Run Cycle(실행 상태)](https://openlineage.io/docs/spec/run-cycle)
3. [OpenLineage 문서: Column Level Lineage Dataset Facet](https://openlineage.io/docs/spec/facets/dataset-facets/column_lineage_facet)
4. [Databricks 문서: Unity Catalog 데이터 계보](https://docs.databricks.com/aws/en/data-governance/unity-catalog/data-lineage)
5. [PostgreSQL 문서: view_column_usage](https://www.postgresql.org/docs/current/infoschema-view-column-usage.html)
