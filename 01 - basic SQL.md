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
- 현재 IKEA의 Interaction Desktop이 사용하는 DB는 MSSQL로 Transact-SQL을 사용하며, 이번 과정에서는 Google의 BIGQUERY와 Standard-SQL을 주로 활용하게 될 예정





## 2. SQL query 구조
- SQL query는 n개의 명령어로 구성할 수 있음  
```
SELCT
FROM
WHERE
HAVING
GROUP BY
LIMIT
ORDER BY
```
- 그 중 필수적으로 요구되는 구문은 `SELECT`와 `FROM`, 2개 
