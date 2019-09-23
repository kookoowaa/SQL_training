# 1. 기초 SQL

## 1. SQL이란?

### Before we start
- SQL<sup>Structured Query Language</sup>은 RDBMS를 관리하기 위해 설계된 프로그래밍 언어
- RDBMS<sup>Relation Database Management System</sup> 간단하게 설명해서, 엑셀처럼 행과 열로 이루어진 2차원 표가 담겨 있는 시스템을 의미
- SQL이 RDBMS를 관리하기 위한 언어라고 하였는데, 데이터 관리라고 함은 단순히 데이터를 추출하는데 그치지 않고 생성하거나 권한을 부여하는 등 폭넓은 의미를 가지며, SQL도 단순히 데이터를 보기 위하는데 사용되지 않음
- 단, 이번 과정은 데이터 추출에 국한하여 진행 될 예정

### DB의 종류
- 99.999% 이상의 경우 다른 프로그래밍 언어가 아닌, SQL로 DB를 관리
- DB(이하 RDBMS)의 종류는 MYSQL, MSSQL, ORACLE, BIGQUERY 등 다양하며 속도, 가격, Query 난이도 등에 따라 사용 목적이 상이
- 사용되는 SQL 방식도 미묘하게 다르긴 하지만, 한 종류의 SQL에 익숙해지면 다른 종류의 DB를 관리하는 것도 크게 어렵지 않음
- 현재 IKEA의 Interaction Desktop이 사용하는 DB는 MSSQL로 Transact-SQL을 사용하며, 이번 과정에서는 Google의 BIGQUERY와 Standard-SQL을 주로 활용할 예정

### Query란?
- Query란 한국어로 질의 자체를 의미
- ICBM에서 파라미터를 정의하여 리포트를 요청하는 것도 Query이고, Google에서 검색하는 것도 Query이고, 사이트에서 로그인 하는 것도 Query의 일환
- CSC에서는 일종의 Report 개념으로 사용하고 있는데, 이는 Query를 통해 반환된 결과값으로 해석하는 것이 적절 (외부와 커뮤니케이션 시)
- 이렇게 중구난방으로 데이터를 주고 받는 것을 구조화하고 (프로그래밍) 언어로 만든 것이 Structured Query Language로, DB에 데이터를 요청하는(관리하는) 질의를 전송

## 2. SQL 이해하기
- 일반적인 SQL의 query문은 아래 6개의 명령어로 구성할 수 있음  
```
SELCT
FROM
WHERE
HAVING
GROUP BY
ORDER BY
```
- 그 중 query에서 필수적으로 요구되는 구문은 `SELECT`와 `FROM`, 2개이며 1회차에는 이 2가지 구문만으로 데이터를 추출하는 방법을 알아볼 예정
- 일반적인 데이터의 구조는 아래와 같음:

|데이터테이블 |열1|열2|열3|
|---|---|---|---|
|행1|
|행2|
|행3|
|행4|

> - 여기서 `SELECT`문은 `열 이름`을 의미함
> ```
> SELECT
>    열1, 열2
>    
>=> 열1과 열2를 선택한다
>```

> - `FROM`문은 `테이블 이름`을 의미
>```
>SELECT
>    열1, 열2
>FROM
>    데이터테이블
>
>=> 데이터테이블에서 열1과 열2를 선택한다
>```

>- 열 이름 대신 `*`를 기입하면, 모든 열을 선택하도록 함
>```
>SELECT
>    *
>FROM
>    데이터테이블
>
>=> 데이터테이블에서 모든 열을 선택한다
>```

- SQL QUERY는 `테이블 이름`과 `열 이름`으로 데이터를 추출하는 것을 기본으로 함
- 기본적으로 선택한 `열`로 구성된 `테이블`에서, 조건/필터를 활용하여 원하는 `행`의 값을 추출하는 구조로 구성
- 조건/필터를 활용하는 방법은 2장에서 배울 `WHERE`, `GROUP BY` 등의 구문을 활용

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