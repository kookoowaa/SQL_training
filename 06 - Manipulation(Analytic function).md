# 7. 데이터 조작 (심화과정)

## RECAP
>- `SELECT`와 `FROM`문을 통해 데이터 추출
>- `WHERE`문을 활용하면 조건에 부합하는 데이터만 추출 (이 때, 조건은 열 기준으로 적용)
>- `GROUP BY`문을 활용하면 엑셀의 피벗테이블 형태로 데이터를 가공한 후 추출 (집계함수 필수)
>- `JOIN`을 활용하면 두개 이상의 테이블을 병합 가능하며 일반적으로 `INNER JOIN`과 `LEFT JOIN`을 자주 사용
>- 데이터는 저장할 때 타입이 정해져 있으며, 데이터 타입별로 가능한 연산의 종류가 달라 데이터 타입과 구연하고자 하는 기능을 파악하고 있어야 함

## SUMMARY
>- `WINDOW` 함수는 특정 조건의 행들에 대하여 `집계함수`, `탐색함수`, `번호지정함수`를 활용하여 결과값을 반환
>- 엑셀에서 사용하기 어려운 조건들은 한두줄 코딩으로 구현할 수 있음

## 1. Analytic(Window) FUNCTION
- 집계함수와 유사한 기능을 하지만, 결과를 반환하는 방식이 다름
- 집계함수가 행 **그룹에 대해 하나의 값을 반환**하는데 반해, WINDOW FUNCTION은 **각 행마다 연산의 결과 값을 반환**
- 예를 들어, A 그룹의 콜수 대해 `SUM()` 함수를 사용하는 경우, 집계함수는 A 그룹의 콜수를 모두 더하여 반환하지만, ANALYTIC FUNCTION의 경우 A 그룹에 속한 각 co-worker의 기록은 유지하되, 합쳐진 A 그룹의 콜수를 반환
- 이 같은 기능이 유용한 이유는 집계함수보다 `탐색함수`나 `번호지정함수`를 사용할 때 유용하게 사용할 수 있음


## 2. 예제-1 (`ROW_NUMBER()`)
- 아래의 예제는, NPS 발송 대상을 추출하는 코드임
- NPS 결과는 특정 렙업코드에 치우치지 않아야 하며, 따라서 **렙업 코드 별로** 동일 비중(1/3)을 샘플링 할 필요가 있음
- 그냥 데이터를 추출하여 샘플링 할 경우 한쪽으로 치우칠 수 있어 ANALYTIC FUNCTION을 활용하기로 함
- 아래 예제에서 활용한 ANALYTIC FUNCTION은 번호지정 함수 `Row_number()`로 데이터를 `PARTITION BY` 조건을 활용하여 렙업코드 기준으로 정렬한 후 그룹 내 1, 2, 3, 4 ... 순으로 번호를 부여하는 연산을 수행
- 위에서 부여한 번호가 3으로 나누어 떨어지는 경우만 발송 대상으로 선정하여, 고르게 샘플링할수 있도록 설계
![](window_function1.png)


```sql
select  
    a.AssignAgent,
    convert(varchar(10), a.insertDate,112) + replace(convert(varchar(8),a.insertdate,108),':','') as TimeOfCS,
    a.CallID,
    a.TelNumber,
    SUBSTRING(b.wrapupCode, 1, 2) as WrapupCode,
    b.WorkgroupID,
    row_number() over (partition by SUBSTRING(wrapupCode, 1, 2) order by SUBSTRING(wrapupCode, 1, 2)) as 'row_number'
from
    I3_IC.dbo.T_SMS_Info as a,
    I3_IC.dbo.InteractionWrapup b
where
    a.CallID = b.interactionIDKey
    and TelNumber like '010%'
    and (AssignAgent <> 'eukan6' and AssignAgent <> 'habae'
        and AssignAgent <> 'hykim44' and AssignAgent <> 'hysim1'
        and AssignAgent <> 'julee31' and AssignAgent <> 'julim11'
        and AssignAgent <> 'kupar1' and AssignAgent <> 'mijan46'
        and AssignAgent <> 'mocho12' and AssignAgent <> 'ohpar'
        and AssignAgent <> 'siyan14' and AssignAgent <> 'yecho6'
        and AssignAgent <> 'T1' and AssignAgent <> 'T2'
        and AssignAgent <> 'T3' and AssignAgent <> 'T4'
        and AssignAgent <> 'T5')
    and wrapupCode <> 'NS'
    and a.InsertDate >= DATEADD(hh,15,DATEADD(dd,DATEDIFF(dd,0,GETDATE()-1),0))
    and a.InsertDate < DATEADD(hh,15,DATEADD(dd,DATEDIFF(dd,0,GETDATE()),0))
```

## 3. 예제-2 (`LEAD()`)
- 아래 예제는 버블콜 비중을 구분하기 위한 예제임
- 특정 기간 동안 1콜이상 걸려온 번호 중에 반복문의 빈도를 추출
- `LEAD()` 함수는 `ORDER BY`조건이 주어졌을 때 이전 데이터의 값을 가져오는 기능을 수행
- `PARTITION BY`와 함께 사용한다면, 전화번호 별로 이전 문의 일자 또는 `Null` 값을 반환하게 할 수 있음 (`PARTITION BY`가 없다면 데이터를 쭉 나열한 후 이전 데이터의 문의 일자를 반환하게 됨)
- 결과물은 다음과 같은 데이터를 반환하게 됨:
![](window_function2.png)



```sql
SELECT
	Call_number,
	Contact_date,
	Previous_contact,
    abs(datediff(day, Contact_date, Previous_contact)) as 'Date_difference'
	

FROM
(SELECT
    RemoteNumber 'Call_number',
    cast(initiatedDate as date) as 'Contact_date',
    cast(lag(InitiatedDate, 1) over (partition by remotenumber order by InitiatedDate) as date) as 'Previous_contact',
	count(remotenumber) over (partition by remotenumber) as 'total_contact'

    FROM
        I3_IC.dbo.calldetail_viw

    Where
        InitiatedDate >= '2019-09-01' and InitiatedDate < '2019-10-01'
        and CallDirection = 'Inbound'
        and LineId = 'UDP5060'
        and LocalUserId = '-'
        and len(remotenumber) between 9 and 11
        and AssignedWorkGroup = '-'
        and remoteName not like '%cj%'
        and remoteName not like '%gl%'
        and remoteName not like N'%엔텍%'
        and remotenumber not like '00%'
) t

WHERE
	total_contact >1

ORDER BY
    Call_number
```
- 아래 그림을 참조하면 `LEAD()`, `PARTITION BY`의 작동원리를 보다 쉽게 이해할 수 있음
![](ref_window_function.png)