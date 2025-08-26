# 2. 조건문

### RECAP
> - SQL은 데이터베이스를 관리하기 위해 설계된 프로그래밍 언어로, 데이터 추출에 사용됨
> - 기본적인 SQL query문은 `SELECT`와 `FROM` 2가지 구문으로 구성되며, 이를 활용하여 데이터를 추출할 수 있음

### SUMMARY
> - `LIMIT` 같은 구문을 활용하여 추출하고자 하는 데이터 양을 제한할 수 있음
> - 데이터를 추출할 때도 사용하지만, 사전에 데이터를 파악할 때에 유용하게 사용
> - `WHERE` 구문을 사용하면 엑셀을 필터처럼 원하는 조건의 행 값만을 추출 가능

## 1. 데이터를 효율적으로 파악하려면 TOP(), LIMIT, DISTINCT()

### `SELECT`, `FROM` 쿼리문의 한계
- 챕터1의 과제(실습)를 수행하다 보면, 데이터양의 너무 방대하여 추출하고 활용하는데 제약이 생김
- 특히 Google의 Big Query를 사용할 때와는 달리 MSSQL로 수행하는 작업의 경우 시스템 리소스의 한계로 오래 걸리는 경우가 잦음
- 또한 추출된 데이터의 량이 너무 많은 경우, 로컬 환경에서 살펴보기 어려움

### `TOP()`, `LIMIT`
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

### `DISTINCT()`
- 데이터를 살펴보는 효과적인 방법 중 하나로 `DISTINCT` 함수 활용 가능
- `DISTINCT` 함수는 엑셀에서 `중복된 항목 제거`와 유사한 기능을 제공하며, 특정 열에 어떤 데이터 값들이 있는지 확인 할 수 있음
- 일반적인 사용 방법은 ( )괄호안에 열 이름을 입력하여, 해당 열의 고유 값들을 확인
- 또는 다양한 활용 방법 중 하나로 `WHERE` 구문에 중첩 SQL Query문을 엮어서 자동화 가능
- 사용 방법은 아래와 같음:
```sql
SELECT
    DISTINCT(<열이름>)
FROM
    <테이블이름>
```

## 2. WHERE
- SQL에서 가장 중요하다고 해도 과언이 아닌 구문
- `SELECT`와 마찬가지로 열 기준으로 인덱싱 하지만, 조건에 부합하는 행 데이터를 반환하므로 원하는 조건의 데이터만 추출이 가능
- 엑셀의 필터 기능과 매우 유사하다고 볼 수 있으나, 기능 및 함수 사용 측면에서 활용도가 무궁무진함 (예: `가`로 시작해서 `하`로 끝나는 문자열 모두 반환)
- 사용법은 간단하며, `FROM` 구문 이후 `WHERE`문과 `조건`을 입력
- 예시: "서랍에 있는 연필 중 길이가 10cm 이상인 연필을 찾고 싶음"
```sql
SELECT
    연필
FROM
    서랍
WHERE
    길이 > 10
```

### `WHERE`의 조건문
- 안타깝게도, SQL은 엑셀만큼의 편의성을 보장해 주지는 않는 만큼 조건을 일일히 입력해야 함
- 조건은 흔히 사용하는 엑셀의 함수와(SUM(), COUNT(), AVERAGE() 등) 유사한 만큼 몇가지 기호 및 함수만 알게 되면 크게 어렵지 않음
- 원하는 기능이 있는데 어떤 함수를 써야할 지 모르겠다면 **GOOGLE**검색에 이미 있을 확률이 다분 하니 검색을 생활화! (지식인 성격의 [Stackoverflow](http://stackoverflow.com) 추천)

### 조건 기호
- `WHERE` 조건문에서 활용되는 주요 기호들은 아래 표를 참조

|기호|의미|예제|
|---|---|---|
|=| 같다|`WHERE userid = 'chpar10'` 
|!=| 같지 않다| `WHERE date != '2019-09-30'`
|<>| 같지 않다| `WHERE date <> '2019-09-30'`
|<| 미만| `WHERE pickuptime < 31`
|>=| 이상| `WHERE nCount >= 500`
|like| 포함하다(문자)| `WHERE 이름 like 'PABLO PA%'`
|in|포함하다(데이터)| `WHERE 이름 in (SELECT 이름 FROM KMI)`

### 복수의 조건문
- 조건을 추가하고 싶은 경우는 `AND`를 활용
- 예시: "서랍에 있는 연필 중 길이가 10cm 이상이고 색상이 빨간 연필을 찾고 싶음"
```sql
SELECT
    연필
FROM
    서랍
WHERE
    길이 > 10
    AND 색상 = '빨간색'
```
- 특정 컬럼에 **여러 조건**을 추가하고 싶은 경우는 `OR`를 활용
- 예시: "서랍에 있는 연필 중 길이가 10cm 이상이고 색상이 빨갛거나 파란 연필을 찾고 싶음"
```sql
SELECT
    연필
FROM
    서랍
WHERE
    길이 > 10
    AND (색상 = '빨간색' OR 색상 = '파란색')
```
- 위와 동일한 결과는 `OR` 대신 `in`을 사용해서도 추출할 수 있음
```sql







SELECT
    연필
FROM
    서랍
WHERE
    길이 > 10
    AND 색상 in ('빨간색', '파란색')
```


## 3. 실습
1. Bigquery에서 제공하는 `bigquery-public-data` DB의 `iowa_liquor_sales` 데이터세트의 `sales` 테이블을 사용하여 `Prime Mart / Broadway Waterloo`의 판매금액과 판매 날짜를 모두 추출
2. Bigquery에서 제공하는 `bigquery-public-data` DB의 `iowa_liquor_sales` 데이터세트의 `sales` 테이블을 사용하여 판매 금액이 $1,000 이상인 판매데이터를 모두 추출


## 4. 과제
- tbd
