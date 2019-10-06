# 3. 데이터 조작하기

### RECAP
> - `SELECT`와 `FROM`문으로 데이터 추출이 가능
>- 보다 정교하게 데이터를 추출하려면 `WHERE`문을 활용해서 조건을 부여하면 됨

### SUMMARY
> - 데이터양이 많거나, 반복적으로 동일업무를 수행하는 경우는 Query문을 통해 데이터를 조작까지 SQL을 통해 자동화 하는것이 효율적
> - `SELECT`문을 통해 데이터베이스의 열을 추출가능할 뿐 아니라 연산을 통해 을 생성하는 것도 가능
> - `GROUP BY`문을 통해 피벗테이블 형태로 데이터 조작 가능

## 1. 열 생성하기
- `SELECT`문을 통해 데이터 열을 추출하는 것 말고 생성하는 것도 가능
- 이 때 주로 수학적 연산이나 함수를 활용해서 새로운 열을 생성
- 만드는 방법은 `DISTINCT` 함수를 사용할 때 처럼 `SELECT` 문에 연산이나 함수를 넣고 열이름을 정의하면 됨
- 예를 들어 아래와 같은 테이블이 있다고 가정:  

|Name|dollar|
|---|---|
|candy|1|
|crisp|3|
|ice cream|5|
|candy|7|
- `Dollar`와 `Dollar`보다 1이 적은 값을 가져오고 싶다면 아래와 같이 Qeury 생성이 가능
```sql
SELECT
    Dollar
    , Dollar - 1
FROM
    테이블
```
- 그럼 `Dollar` 값을 한화로 (1,100원) 보려면 어떻게 할수 있을까?
```sql







SELECT
    Name
    , Dollar
    , Dollar * 1100
FROM
    테이블
```
- 위와 같이 새로운 열을 생성해서 값을 확인 할 수 있음
- 날짜 데이터도 년(`YEAR`), 월(`MONTH`), 일(`WEEKDAY`) 단위로 추출할 수 있음

- 위의 예제들을 보면, SQL을 통해 데이터를 가져오는 프로세스는 테이블에 국한되는 것이 아니고 하나의 열 단위로 프로세스가 이루어짐을 볼 수 있음
- 그렇다는 이야기는, 동일 열만 반복적으로 추출하는 것도 가능
```sql
SELECT
    Dollar, Dollar,Dollar, Dollar
FROM
    테이블
```
- 추가로 열을 호출하면서 Alias라는 기능을 활용하기도 하는데, 열을 선택 한 뒤에 `as '열이름'`을 추가하여 열이름을 지정하는 것도 가능
```sql
SELECT
    Dollar as '달러', Dollar as 'USD', Dollar as 'Money', Dollar as 'Price'
FROM
    테이블
```



## 2. 피벗 테이블 만들기 `GROUP BY`
- 엑셀의 가장 강력한 기능 중 하나인 피벗테이블도 `GROUP BY`문을 통해 SQL에서 구현이 가능
- 사용 로직은 **열 기준**으로 동일 값을 모으고, **집계함수**를 활용해서 결과 값을 반환
- 피벗에서 열 항목을 할당한 후, 갯수를 볼지, 합계를 볼지, 평균을 볼지 정하는 것과 동일한 로직
- 이 때 `GROUP BY`문의 위치는 `FROM` 뒤쪽이 됨
- 예를 들어, 위의 테이블에서 이름 기준으로 데이터를 병합한 후 `Dollar`의 합계를 보고 싶다면 다음과 같이 Query문을 작성 가능
```sql
SELECT
    Name
    , sum(Dollar)
FROM
    테이블
GROUP BY
    Name
```
- 주의할 점은 데이터가 병합이 되는 구조다 보니, `SELECT`문에 `*`를 지정한다면 오류 발생
- `SELECT` 문에는 적어도 **하나 이상의 집계함수**가 필요하고, `GROUP BY`의 대상은 필수가 아님

## 3. `GROUP BY` 후 필터는 `HAVING`을 사용
- 데이터테이블에서 필터는 `WHERE`를 사용하였으나, `GROUP BY` 후에 필터는 `HAVING`을 사용
- 예를 들어 위에서 추출한 `Dollar` 합계 중 3 이상 값을 갖는 데이터만 보고 싶다면 아래와 같이 Query 작성 가능
```sql
SELECT
    Name
    , sum(Dollar)
FROM
    테이블
GROUP BY
    Name
HAVING
    sum(Dollar) >= 3
```

## 4. 결과 값을 정렬 할 때에는 `ORDER BY` 사용
- 피벗 테이블 처럼 중요한 값 위주로 결과를 확인하고 싶은 경우가 많음
- 이때 `ORDER BY`를 사용하며 위치는 Query문의 제일 뒤가 됨
- 위의 데이터에서 `Dollar` 합계 내림차 순으로 데이터를 확인하고 싶다면 아래와 같이 query문 작성
```sql
SELECT
    Name
    , sum(Dollar)
FROM
    테이블
GROUP BY
    Name
HAVING
    sum(Dollar) >= 3
ORDER BY
    sum(Dollar)
```
- 올림차순으로 확인하고 싶다면 `ORDER BY`문 뒤에 `desc`를 명기
```sql
SELECT
    Name
    , sum(Dollar)
FROM
    테이블
GROUP BY
    Name
HAVING
    sum(Dollar) >= 3
ORDER BY
    sum(Dollar) desc
```