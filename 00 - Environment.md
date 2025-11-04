# 0. Environment setup

## 1. SQL을 사용한다는 의미는?

- 사용자가 DB와 **통신**하기 위해서 SQL이란 언어를 사용
- 다만 SQL이란 언어를 전달하는 클라이언트는 다양할 수 있음
- 통상적으로 JetBrain의 DataGrip이 대표적인 클라이언트로 사용되지만, DBeaver라는 오픈소스 프로그램도 빈번하게 사용됨
- 상기 GUI 기반 클라이언트와는 다르게, CLI (DOS 같은 command line)나 흔히 언급되는 Python을 활용해서 SQL과 통신을 주고받기도 함
- SQL을 사용한다는 의미는, DB에 원하는 데이터 양식을 전달하고 내려받는 통신 인터페이스를 의미한다고 볼 수 있음

## 2. 사용 환경 설정

- 본 프로젝트에서는 클라이언트로 DBeaver를 사용하기로 함
 > DBeaver 설치는 **"시작 > Microsoft Store > DBeaver CE 검색 > 설치"** 로 설치 가능
- 처음 접속 시 샘플 DB를 확인 할 수 있으며 (좌측 Database Navigator), 이로 추후 연습하는 목적으로 사용하는 것도 가능
- 업무 scope에서 사용할 DB는 **DuckDB**로, SQLITE와 더불어 별도의 서버 없이 사용가능한 가벼운 DB로 알려져 있음음
- 다만 DB와의 연결은 여타 서버형 DB와 같이 세팅해 주어야 함
> Database Navigator > 우클릭 > Create > Connection > DuckDB 선택 > Path > Open > 파일 선택  
> Connection details에서 **Read-only connection** 체크
- 정상적으로 세팅된다면 좌측 패널에서 DB를 확인할 수 있음
- 연결된 DB를 우클릭하여 SQL 편집기를 클릭하면 DB에 연결하여 SQL문을 Query할/통신할 준비가 되었다고 볼 수 있음  
