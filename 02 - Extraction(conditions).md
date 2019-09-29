# 2. 조건문

### recap
> - SQL은 데이터베이스를 관리하기 위해 설계된 프로그래밍 언어로, 데이터 추출에 사용됨
> - 기본적인 SQL query문은 `SELECT`와 `FROM` 2가지 구문으로 구성되며, 이를 활용하여 데이터를 추출할 수 있음
> - 오늘 다룰 내용은 `SELECT`와 `FROM` 외 조건문을 활용하여 원하는 데이터를 제한하는 방법임

## 1. TOP, LIMIT

### `SELECT`, `FROM` 쿼리문의 한계
- 챕터1의 과제(실습)를 수행하다 보면, 데이터양의 너무 방대하여 추출하고 활용하는데 제약이 생김
- 특히 Google의 Big Query를 사용할 때와는 달리 MSSQL로 수행하는 작업의 경우 시스템 리소스의 한계로 오래 걸리는 경우가 잦음
- 또한 추출된 데이터의 량이 너무 많은 경우, 로컬 환경에서 살펴보기 어려움

### `TOP`, `LIMIT`
- `TOP`과 `LIMIT`는 유사한 기능을 수행: 출력될 데이터 수를 제한
- 차이점은 Standard SQL(Google BigQuery)에서는 `LIMIT`만 지원하고, Transact SQL(MSSQL)에서는 `TOP`만 지원한다는 점이 있음
- `TOP`은 데이터의 처음 부분의 제한된 행 수만 출력하고, `LIMIT`는 데이터의 끝 부분의 제한된 행 수만 출력
- 그리고 `TOP`은 `SELECT`문의 함수 형태로 사용되고, `LIMIT`는 query 하단에 `SELECT` 문과 동등한 레벨에서 사용됨
- 아래 예시를 보면 확인 가능

|구분|`TOP`|`LIMIT`|
|---|---|---|
|호환성|Standard SQL|Transact SQL|
|사용 위치|`SELECT`문|query 끝 부분|
|예시|`SELECT top(5) * FROM table`|`SELECT * FROM table LIMIT 5`|
|결과| 1~5번째 행까지의 데이터| 끝에서 5번째부터 끝까지의 데이터|

## 2. WHERE
- SQL에서 가장 중요하다고 해도 과언이 아닌 구문
- `SELECT`와 마찬가지로 열 기준으로 인덱싱 하지만, 조건에 부합하는 행 데이터를 반환하므로 원하는 조건의 데이터만 추출이 가능
- 엑셀의 필터 기능과 매우 유사하다고 볼 수 있으나, 기능 및 함수 사용 측면에서 활용도가 무궁무진함 (예: `가`로 시작해서 `하`로 끝나는 문자열 모두 반환)

## 3. WHERE의 조건문
### 가. =
### 나. >, <, <>
### 다. LIKE
### 라. IN
### 마. NOT

## 3. 실습
1. Bigquery에서 제공하는 `bigquery-public-data` DB의 `austin_bikeshare` 데이터세트의 `bikeshare_stations` 테이블 정보를 확인
2. Bigquery에서 제공하는 `bigquery-public-data` DB의 `austin_bikeshare` 데이터세트의 `bikeshare_trips` 테이블에서 출발지와 도착지 정보를 확인
- 참조: DB와 데이터세트와 테이블을 연결하는 연산자로 `.`(마침표)를 사용 `bigquery-public-data.austin_bikeshare.bikeshare_stations` 

## 4. 과제
- CSC의 SQL Server로부터 아래 테이블에 접속하여 테이블이 제공하는 정보 파악

>- `calldetail_viw`
>- `InteractionWrapuip`
>- `IWrkgrpQueueStats`
>- `AgentServiceLevel_viw`
>- `AgentActivityLog`
