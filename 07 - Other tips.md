# 7. 기타 쿼리 작성 팁

## 1. CASE WHEN
- 여타 프로그래밍 언어의 `IF-ELSE` 구문을 SQL에서는 `CASE-WHEN`으로 구현 가능
- `WHERE`문은 조건에 부합하는 데이터를 반환; `CASE-WHEN`은 조건에 부합하는 경우에 **지정한 결과**를 반환
- 용법은 아래와 같음:
```sql
--용법
case
    when 조건
    then 조건이 참인 경우의 결과
    else 조건이 거짓인 경우의 결과 end

--예제
case
    when n%2=1
    then '홀수'
    else '짝수' end
```
- 만약 복수의 조건으로 구분짓고 싶다면 아래와 같이 쿼리 작성 가능
```sql
--용법
case
    when 조건1
    then 조건1이 참인 경우의 결과
    when 조건2
    then 조건2가 참인 경우의 결과
    else 조건이 모두 거짓인 경우의 결과 end
    
--예제
case
    when n>100 
    then '3자리 이상의 수'
    when n>10
    then `2자리 수`
    else '1자리 수' end
```

## 2. DECLARE
- 반복적으로 특정 값이 호출 되는 경우, 쿼리문에 모든 파라미터를 상수로 하드코딩 하는 프로세스는 비효율적일 뿐 아니라 재활용하기 비효율적
- 이 경우 변수를 사전에 정의 내리고 파라미터에 변수를 소프트코딩 하는 것이 바람직함
- SQL에서 변수를 정의하는 방법은 `DECLARE`를 활용하는 방법이 가장 손쉬운 방법 중 하나
- `DECLARE`의 용법은 아래와 같음:
```sql
--용법 (단일값)
declare @변수명 데이터타입 = 값

--용법 (테이블)
declare @테이블명 table (컬럼1 데이터타입, 컬럼2 데이터타입);
insert @테이블명 (컬럼1) values
    (값1), (값2), (값3)...;

--예제 (단일값)
declare @today datetime = getdate()
declare @yesterday datetime = getdate()-1

select @today
select @yesterday

--예제 (테이블)
declare @menu table (menu varchar(10), price int);
insert into @menu(menu, price) values
	('americano', 1000), ('steak', 30000);

select * from @menu
```

## 3. With
- `DECLARE`와 유사하지만, `WITH`의 경우 Nested query문을 작성할 때 용이
- Nested query문이라 함은 여러개의 SQL 문을 중첩으로 사용하는 것을 의미하며, 이 때 상위 procedure에서 호출한 qeury문을 `WITH`로 정의, 재상용 할 수 있음
- `WITH`의 용법은 아래와 같음:
```sql
--용법 (단일값)
With tbl_sample as (
SELECT ....
)
```


## 4. UNION
- `JOIN`을 활용하면 복수의 테이블을 동일 조건의 열 기준으로 병합하며, 이 때 **가로 방향으로** 테이블을 확장
- `UNION`의 경우 **같은 칼럼**을 갖는 복수의 테이블을 **세로로 확장**
- `UNION`만 활용할 경우 중복값은 제외, `UNION ALL` 활용 시 중복값을 포함하여 테이블을 확장
- 테이블 크기(용량) 때문에 기간 단위로 별도의 테이블을 생성하는 경우(PARTITION) `UNION`을 활용하여 데이터 복원 가능
- 혹은 하나의 테이블을 여러번 중복 호출하여 작업할 경우 마찬가지로 `UNION` 함수를 효과적으로 활용 가능
- `UNION`의 용법은 `JOIN` 과 유사:

```sql
--용법
select
	*
from
	테이블1
union
	테이블2

--예제
select
	*
from
	menu
union
	menu
```
