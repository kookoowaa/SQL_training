# 7. 데이터 조작 (심화과정)

## RECAP
>- ...

## SUMMARY
>- `WINDOW` 함수를 사용하여 

- NPS 보낼 사람 추출하고 샘플링 할때 `WINDOW` 함수를 활용하고 있음
```sql
SELECT
    t.AssignAgent
    , t.TimeOfCS
    , t.CallID
    , t.AssignAgent
    , t.TelNumber
    , t.WrapupCode
    , t.WorkgroupID

FROM
(
    select  
        a.AssignAgent,
        convert(varchar(10), a.insertDate,112) + replace(convert(varchar(8),a.insertdate,108),':','') as TimeOfCS,
        a.CallID,
        a.AssignAgent,
        a.TelNumber,
        SUBSTRING(b.wrapupCode, 1, 2) as WrapupCode,
        b.WorkgroupID,
        Row_number() over (order by SUBSTRING(wrapupCode, 1, 2),
        convert(varchar(10), a.insertDate,112) + replace(convert(varchar(8),a.insertdate,108),':','')) as 'row_number'
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
) t

where
    t.row_number %3 =0
```
